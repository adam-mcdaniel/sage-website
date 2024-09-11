+++
title = 'Memory Management'
date = 2024-09-10T21:11:36-04:00
series = ['Documentation']
series_order = 13
+++

{{< alert >}}
**Note:** Sage is a systems programming language with *manual memory management*. This means that the compiler does not automatically insert `free` or `delete` calls for you.
{{< /alert >}}

## Allocating Memory

Memory can be allocated on the heap multiple ways.

1. Using the `std.mem.malloc<T>(count: Int): &mut T` function.
2. Using the `new` unary operator.

### `std.mem.malloc`

The `std.mem.malloc` function is a generic function that allocates memory on the heap. It takes a type parameter `T` and an integer `count` that specifies how many elements of type `T` to allocate.

```rs
from std.mem import malloc;

struct Point {
    x: Int,
    y: Int
}

let p = malloc<Point>(1);
p.x = 5;
p.y = 10;

println(*p);
```
{{< alert "code" >}}
**Output:**<br/>
\{x=5, y=10\}
{{< /alert >}}

### `new` Unary Operator

The `new` unary operator takes a given value, then moves it to the heap and returns a pointer to it.

```rs
struct Point {
    x: Int,
    y: Int
}

let p: &mut Point = new {x=5, y=10};
p.x = 7;

println(*p);
```
{{< alert "code" >}}
**Output:**<br/>
\{x=7, y=10\}
{{< /alert >}}

## Freeing Memory

All memory is free'd using the builtin `free` function. This function takes a pointer to the memory to free.

```rs
let p = new {x=5, y=10};
println(*p);
free(p);
```

## Memory Leaks

Memory leaks occur when memory is allocated on the heap and never free'd. This can lead to a program running out of memory and crashing.

Using the C backend, we can detect memory leaks using valgrind.

{{< alert "code" >}}
**Filename:** `main.sg`
{{< /alert >}}
```rs
let p = new {x=5, y=10};
println(*p);
```
{{< alert "code" >}}
`sage -tc main.sg`<br/>
`gcc -o main out.c`<br/>
`valgrind ./main`<br/>
**Output:**<br/>
\{x=5, y=10\}<br/>
==1158997==<br/>
==1158997== HEAP SUMMARY:<br/>
==1158997==     in use at exit: 272 bytes in 1 blocks<br/>
==1158997==   total heap usage: 2 allocs, 1 frees, 1,296 bytes allocated<br/>
{{< /alert >}}

As you can see, there's a mismatch in the number of allocations and frees, which indicates a memory leak.

## Future Work

Target backends currently have the power to attach garbage collectors or automatic reference counting, but these features are not used in the current backends. Lifetimes plus ownership and borrow checking seem to be the most desirable path forward for Sage.