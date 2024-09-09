+++
title = 'About The Sage Programming Language'
date = 2024-09-08T21:37:06-04:00
series = ['Documentation']
series_order = 1
+++

## Table Of Contents
1. [Overview](#overview)
2. [Community](#community)
3. [Code Examples And Syntax](#code-examples-and-syntax)
4. [Learn More](#learn-more)
5. [About The Author](#about-the-author)

## Overview

Sage is a small programming language with a unique blend of features from Rust and Python. It's **statically typed**, **portable**, and **easy-to-use**.

{{< alert "lightbulb" >}}
**Why Sage?** The Sage programming language was created as an alternative to languages like C, but with a nicer type system and development experience. Users are able to `print` out modules, functions, types. `enum`s are more powerful, and can be used for better error management.
Immutability is by default, with enforced typechecking and mutability rules. Sage also includes a module system for organizing code.
{{< /alert >}}

Sage can be used for anything from [operating systems development](https://github.com/adam-mcdaniel/sage-os), to [web development](../../playgrounds/playground).

## Community

Join the [Discord server](https://discord.gg/2GX4Jggy6d) to chat about Sage! Let us know if you have any thoughts or comments about the language!

## Code Examples And Syntax

Without further ado, let's see what Sage's syntax looks like!

#### Hello World!

Sage code is like Python -- you can just start writing without a `main` function!

To `print` something, just use `print` on whatever arguments you want!

```rs
println("Hello world!");
```
{{< alert "code" >}}
**Output:**<br/>
Hello world!<br/>
{{< /alert >}}


`print` also works on custom data structures.

```rs
println("Hello! ", {x = 5, y = 6, z = 7}, " ", [1, 2, 3, 4]);
```
{{< alert "code" >}}
**Output:**<br/>
Hello! {x=5, y=6, z=7} [1, 2, 3, 4]<br/>
{{< /alert >}}

#### Functions

You can write functions in Sage using the `fun` keyword. Here's an example of the `gcd` function implemented in Sage!

```rs
// Calculate the greatest common divisor of two numbers
// using Euclid's algorithm
fun gcd(a: Int, b: Int): Int {
    if b == 0 {
        return a;
    }
    return gcd(b, a % b);
}

println(gcd(12, 15));
```
{{< alert "code" >}}
**Output:**<br/>
3<br/>
{{< /alert >}}

#### Standard Library Imports

You can get started with the standard library by importing some modules!

```rs
// Import some stuff!
from std.fallible import Option, Result, panic;
from std.collections import Vec, HashMap, List;
from std.io import *;

// Create a vector and push stuff onto it!
let mut v = Vec.make<Int>();
v.push(1);
v.push(2);
v.push(3);

v.print();
```
{{< alert "code" >}}
**Output:**<br/>
[1, 2, 3]<br/>
{{< /alert >}}

#### Structs And Methods

Here's how we can define custom `struct` types, and imbue them with methods we can call!

```rs
// Create a structure named Point, with members `x` and `y`
struct Point {
    x: Int,
    y: Int
}

impl Point {
    // A method for creating a new point
    fun new(x: Int, y: Int): Point {
        return {x = x, y = y};
    }

    // Shift this point by a given amount in the X
    // and Y directions
    fun translate(&mut self, dx: Int, dy: Int) {
        self.x += dx;
        self.y += dy;
    }
}

// Create a mutable point at (4, 5)
let mut p = Point.new(4, 5);
// Print the point
println(p);
// Translate the X by -5, and the Y by 10
p.translate(-5, 10);
// Print the translated point
println(p);
```
{{< alert "code" >}}
**Output:**<br/>
\{x=4, y=5\}<br/>
\{x=-1, y=15\}<br/>
{{< /alert >}}

#### Sum Types

Sage supports generic algebraic datatypes, and typechecks them with structural equality! This lets you define types linked-lists in a much more canonical way than most languages.

```rs
enum List<Elem> {
    Cons {
        data: Elem, 
        next: &List<Elem>
    },
    Nil
}

let l = List<Int> of Cons {
    data=5,
    next=new List<Int> of Nil
};

match l {
    of Cons {data, next} => {
        println("l is cons with data=", data, " and next=", next);
    },
    of Nil => {
        println("Got nil");
    }
}
```
{{< alert "code" >}}
**Output:**<br/>
l is cons with data=5 and next=&(38999)<br/>
{{< /alert >}}


#### Const-Generics

Sage's typesystem is also equipped to handle types with `const` parameters. This can come in handy for **many** use-cases. For example, this feature allows you to typecheck matrix dimensions in matrix multiplications!

```rs
// Define a constant sized matrix with a generic element type
// and parameterized width and height.
struct Matrix<T, const Rows: Int, const Cols: Int> {
    arr: [[T * Cols] * Rows]
}

// Add some methods to our matrix
impl Matrix<T, Rows, Cols> {
    // Create a new matrix populated with initial values
    fun new(x: T): Matrix<T, Rows, Cols> {
        return {arr=[[x] * Cols] * Rows};
    }

    // Get a value from a matrix
    fun get(&self, row: Int, col: Int): &T {
        return &self.arr[row][col];
    }

    // Multiply with another matrix
    fun mul<const NewCols: Int>(
        &self,
        other: &Matrix<T, Cols, NewCols>,
        zero: T,
        add: fun(T, T) -> T,
        mul: fun(T, T) -> T
    ): Matrix<T, Rows, NewCols> {
        let mut result = Matrix.new<T, Rows, NewCols>(zero);
        // Perform the actual matrix multiplication on the rows and cols
        // This uses the naive algorithm for matrix multiplication
        for let mut j=0; j<NewCols; j+=1; {
            for let mut i=0; i<Rows; i+=1; {
                let mut sum = zero;
                for let mut k=0; k<Cols; k+=1; {
                    sum = add(sum, mul(self.arr[i][k], other.arr[k][j]));
                }
                result.arr[i][j] = sum;
            }
        }
        result
    }
}
```

## Virtual Machine Instruction Set

{{< alert "lightbulb" >}}
**History:**
What makes Sage special is not its frontend, however, but its backend. The backend was originally born out of a **SIMD**-extended Brainf$%&@ compiler.
{{< /alert >}}

*Sage's instruction set is tiny enough to port in an afternoon, but efficient enough to compile high level code efficiently.*
The virtual machine is a simple Turing-tape based architecture, not a stack or register based virtual machine. This is one of the contributing factors to Sage's portability.

| Instruction | C Equivalent    |
| ----------- | --------------- |
| `while`     | `while (reg[0]) {` |
| `if`        | `if (reg[0]) {`    |
| `else`      | `} else {`      |
| `end`       | `}`             |
| `set N_0, N_1, ..., N_X`     | `reg[0] = N_0; reg[1] = N_1; ... reg[x] = N_X;` |
| `call`      | `funs[reg[0]]();`  |
| `ret`       | `return;`       |
| `load N`    | `memcpy(reg, tape_ptr, N * sizeof(cell));` |
| `store N`   | `memcpy(tape_ptr, reg, N * sizeof(cell));` |
| `move N`    | `tape_ptr += N;`   |
| `where`     | `reg[0].p = tape_ptr;`   |
| `deref`     | `push(tape_ptr); tape_ptr = *tape_ptr;` |
| `refer`     | `tape_ptr = pop();` |
| `index N`   | `for (int i=0; i<N; i++) reg[i].p += tape_ptr->i;` |
| `offset O, N` | `for (int i=0; i<N; i++) reg[i].p += O;` |
| `swap N` | `for (int i=0; i<N; i++) swap(reg + i, tape_ptr + i);` |
| `add N` | `for (int i=0; i<N; i++) reg[i].i += tape_ptr[i].i;` |
| `sub N` | `for (int i=0; i<N; i++) reg[i].i -= tape_ptr[i].i;` |
| `mul N` | `for (int i=0; i<N; i++) reg[i].i *= tape_ptr[i].i;` |
| `div N` | `for (int i=0; i<N; i++) reg[i].i /= tape_ptr[i].i;` |
| `rem N` | `for (int i=0; i<N; i++) reg[i].i %= tape_ptr[i].i;` |
| `or N` | `for (int i=0; i<N; i++) reg[i].i \|\|= tape_ptr[i].i;` |
| `and N` | `for (int i=0; i<N; i++) reg[i].i &&= tape_ptr[i].i;` |
| `not N` | `for (int i=0; i<N; i++) reg[i].i = !reg[i].i;` |
| `bitand N` | `for (int i=0; i<N; i++) reg[i].i &= tape_ptr[i].i;` |
| `bitor N` | `for (int i=0; i<N; i++) reg[i].i \|= tape_ptr[i].i;` |
| `bitxor N` | `for (int i=0; i<N; i++) reg[i].i ^= tape_ptr[i].i;` |
| `lsh N` | `for (int i=0; i<N; i++) reg[i].i <<= tape_ptr[i].i;` |
| `l-rsh N` | `for (int i=0; i<N; i++) reg[i].i = (uint64_t)reg[i].i >> tape_ptr[i].i;` |
| `a-rsh N` | `for (int i=0; i<N; i++) reg[i].i >>= tape_ptr[i].i;` |
| `gez N` | `for (int i=0; i<N; i++) reg[i].i = reg[i].i >= 0;` |

## Learn More

{{< alert >}}
**Documentation:** First, [take a look at the docs](../)!
{{< /alert >}}


You can read [my blog post](https://adam-mcdaniel.github.io/blog/compilers-for-the-future) (~20 minute read) about the programming language to learn more about the implementation!

[Here's a 23 minute YouTube video that covers how compilers work, and delves into Sage!](https://www.youtube.com/watch?v=QdnxjYj1pS0)

Join the [Discord server](https://discord.gg/rSGkM4bcdP) to chat about Sage!

## How do I contribute?

If you want to contribute, you can open an issue or a pull request. [Adding backends for other architectures is a great way to contribute!](https://github.com/adam-mcdaniel/sage/blob/main/src/targets/c.rs) We also need a VSCode syntax highlighting extension!
## About The Author

[I'm a computer science PhD student](https://adam-mcdaniel.net) at the [University of Tennessee, Knoxville🍊](https://www.youtube.com/watch?v=-8MlEo02u54). Rust is my favorite language, and [I've](https://github.com/adam-mcdaniel/oakc) [written](https://github.com/adam-mcdaniel/harbor) [many](https://github.com/adam-mcdaniel/tsar) [other](https://github.com/adam-mcdaniel/free) [compilers](https://github.com/adam-mcdaniel/xasm). This is the last project I started as a teenager, and I was the only author to touch any of the code up to version `v0.0.2-alpha` (12/25/2023)! I'm looking for work opportunities for Summer 2024 (after I finish my Masters degree), so if you're interested in hiring me, please reach out to me at [amcdan23@vols.utk.edu](mailto:amcdan23@vols.utk.edu)!
