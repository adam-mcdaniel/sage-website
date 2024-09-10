+++
title = 'FAQ'
date = 2024-09-10T18:18:10-04:00
+++

# Frequently Asked Questions

## What is Sage?

Sage is a statically-typed, compiled (native) programming language focused on maximum portability -- run your code on a Flipper Zero, a Raspberry Pi, or a desktop computer with the same VM code! Even though Sage has a VM, it still can compile to native.

## What's Sage's License?

Sage is licensed under the **MIT license**. You can find the license in the [GitHub repository](https://github.com/adam-mcdaniel/sage).

## What languages is Sage inspired by?

Sage is inspired by Rust, C, and Python. Sage's type system includes [parameterized algebraic datatypes](https://en.wikipedia.org/wiki/Generalized_algebraic_data_type), which are powerful programming constructs. Sage also provides Python-like modules, helpful functions like `print`, and it doesn't require a `main` function in order to start writing code!

```rs
from std.math import gcd;

match gcd(12, 18) {
    6 => println("GCD is 6"),
    _ => println("Oh no! Incoherent universe!")
}
```
{{< alert "code" >}}
**Output:**<br/>
GCD is 6
{{< /alert >}}

## Is Sage Memory Safe?

No, you can still blow your foot off with Sage. Sage is a systems programming language, and as such, it allows you to do things like use-after-free, buffer overflows, and other unsafe operations. However, Sage does have a strong type system that can help prevent some of these issues. For example, you can wrap all of your pointer types with `std.fallible.Option` to prevent null pointer dereferences, and typecheck pointer accesses.

## What is the Sage VM?

The Sage VM is an abstract virtual machine that Sage compiles to. Its instruction set is simple enough to port to any modern architecture, and it is designed to be easy to implement. The VM is Turing-tape based.

{{< alert "lightbulb" >}}
**What does the VM look like?** The Sage VM is very tiny, [you can see the transpiler for the VM code to C here!](https://github.com/adam-mcdaniel/sage/blob/main/src/targets/c.rs) The main compile function is ~150 lines of Rust!
{{< /alert >}}

The VM code can be thought of as a Turing-tape/register based virtual machine that can directly be lowered to machine code. All of the VM instructions operate on addresses and CPU-word sized values.

## How do I get started with Sage?

You can get started with Sage by visiting the [Getting Started](../getting-started) page. This page will walk you through installing Sage and writing your first program.

## How do I contribute to Sage?

Join the community from the [Community](../community) page and we'll get you started!

## What does the Sage feature roadmap look like?

[Here's the current roadmap for Sage!](https://github.com/adam-mcdaniel/sage?tab=readme-ov-file#feature-roadmap)
