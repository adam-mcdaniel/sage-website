+++
title = 'Calling C Code'
date = 2024-09-10T10:59:15-04:00
series = ['Documentation']
series_order = 11
+++

## Defining Foreign Functions In Sage

Sage allows you to call functions from target backends, such as C, using the `extern` keyword. This keyword tells the compiler to expect the symbol to be provided by the target backend.

```rs
// Signatures for the C functions `getchar` and `putchar`.
extern fun getchar(): Char;
extern fun putchar(ch: Char);

// A signature for a function that wraps the C function `memcpy`.
extern fun memcpy(dst: &mut Int, src: &Int, cells: Int);

let a = [1, 2, 3, 4, 5];
let mut b = [0, 0, 0, 0, 0];

// Call our C function to copy the first 5 elements of `a` into `b`.
memcpy(&mut b, &a, 5);
```

{{< alert "lightbulb" >}}
**About Sage:** The [web-demo](../../playgrounds/playgound) of Sage provides only two `extern` functions in the backend: `alert` and `eval`. You can use `eval` to run arbitrary JavaScript code, so it's still complete to interoperate with other code in the browser.
{{< /alert >}}

## Writing C Code For Sage

Now that we can tell Sage to expect C functions, we need to write the C code that will be called. The compiled output will automatically include an `ffi.h` file containing your foreign function implementations if you provide one! Copy and paste your C function definitions into the `ffi.h` file, and Sage will take care of the rest.

### Simple `add` Function Example

To demonstrate how to write C code for Sage, let's write a simple `add` function in C.
This will take two integers from the Sage VM and give back their sum.

The Sage FFI performs communication with `extern` functions using a buffer. This buffer contains the arguments and return values for foreign functions. Whenever Sage calls an `extern` function, it pushes its arguments onto the buffer and then calls the function. The function reads the arguments from the buffer, performs its computation, and then pushes the return value onto the buffer. Sage then reads the return value from the buffer and continues execution.


So, our `add` function will read two integers from the buffer, add them together, and then push the result back onto the buffer.

{{< alert "code" >}}
**Filename:** `main.sg`
{{< /alert >}}
```rs
// Our external function we want to call from Sage
extern fun add(a: Int, b: Int): Int;

// Call our C function to add 2 and 3
println(add(3, 4));
```

This will call our `add` function defined below in `ffi.h` and compute the result `7`.
<!-- {{< alert "code" >}}
**Output:**<br/>
7
{{< /alert >}} -->


{{< alert "code" >}}
**Filename:** `ffi.h`
{{< /alert >}}
```c
// The C backend implementation for
// 
//     `extern fun add(a: Int, b: Int): Int`
// 
// The backend automatically adds two underscores to the
// function name to avoid conflicts with other symbols.
void __add() {
    // Get our two numbers from the buffer.
    // Arguments are passed in order. There are two arguments,
    // and the last value is always at ffi_ptr[0].
    // So, the first argument is at ffi_ptr[-1], and the
    // second is at ffi_ptr[0].
    cell a = ffi_ptr[-1], b = ffi_ptr[0];
    // Pop the arguments off the buffer.
    ffi_ptr -= 2;

    // Add them together
    cell result;
    result.i = a.i + b.i;

    // Push the result back onto the buffer
    ++ffi_ptr; // Add space for the result
    *ffi_ptr = result; // Push the result
}
```

{{< alert "code" >}}
**Output:**<br/>
7
{{< /alert >}}

### Calling `memcpy` From Sage

Let's write a more complex example: calling the C function `memcpy` from Sage. This function copies an array of bytes from one location to another. This is useful for efficiently copying large amounts of data.

{{< alert "code" >}}
**Filename:** `main.sg`
{{< /alert >}}
```rs
// Our external function we want to call from Sage
extern fun memcpy(dst: &mut Int, src: &Int, cells: Int);

let a = [1, 2, 3, 4, 5];
let mut b = [0, 0, 0, 0, 0];

// Call our C function to copy the first 5 elements of `a` into `b`.
memcpy(&mut b, &a, 5);
println(b);
```

This will call our `memcpy` function defined below in `ffi.h` and print the result `[1, 2, 3, 4, 5]`.

{{< alert "code" >}}
**Filename:** `ffi.h`
{{< /alert >}}
```c
#include <string.h>

// The C backend implementation for
//
//     `extern fun memcpy(dst: &mut Int, src: &Int, cells: Int)`
//
// The backend automatically adds two underscores to the
// function name to avoid conflicts with other symbols.
void __memcpy() {
    // Get our three numbers from the buffer.
    // Arguments are passed in order. There are three arguments,
    // and the last value is always at ffi_ptr[0].
    // So, the first argument is at ffi_ptr[-2], the
    // second is at ffi_ptr[-1], and the third is at ffi_ptr[0].
    cell dst = ffi_ptr[-2], src = ffi_ptr[-1], cells = ffi_ptr[0];
    // Pop the arguments off the buffer.
    ffi_ptr -= 3;

    // Copy the memory
    memcpy(dst.p, src.p, cells.i * sizeof(cell));
}
```

{{< alert "code" >}}
**Output:**<br/>
[1, 2, 3, 4, 5]
{{< /alert >}}