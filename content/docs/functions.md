+++
title = 'Functions'
date = 2024-09-09T17:03:51-04:00
series = ['Documentation']
series_order = 6
+++

In Sage, functions are declared with the `fun` keyword.

```rs
fun add(a: Int, b: Int): Int {
  return a + b;
}

println(add(2, 3));
```
{{< alert "code" >}}
**Output:**<br/>
5<br/>
{{< /alert >}}

### Alternate Syntax

Functions may also be defined with a single expression and the `=` operator.

```rs
fun add(a: Int, b: Int): Int = a + b;

println(add(2, 3));
```
{{< alert "code" >}}
**Output:**<br/>
5<br/>
{{< /alert >}}

## Function Types

Functions can be passed as arguments to other functions. The type of a function is written as `fun(T1, T2, ..., Tn) -> T`.

```rs
// Define `f` as a function that takes two ints and
// returns another int.
// Call it on the next two arguments.
fun test(f: fun(Int, Int) -> Int, a: Int, b: Int): Int {
    return f(a, b);
}

// The function to pass to `test`
fun add(a: Int, b: Int): Int = a + b;

// Call `test` with our other function!
println(test(add, 2, 3));
```
{{< alert "code" >}}
**Output:**<br/>
5<br/>
{{< /alert >}}

## Templates

Functions may also take template parameters:

```rs
fun first<A, B>(a: (A, B)): A = a.0;
fun second<A, B>(a: (A, B)): B = a.1;

println(first<Int, Float>((2, 3.0)));
println(second<Int, Float>((2, 3.0)));
```
{{< alert "code" >}}
**Output:**<br/>
2<br/>
3<br/>
{{< /alert >}}