+++
title = 'Variables And Mutability'
date = 2024-09-09T15:50:33-04:00
series = ['Documentation']
series_order = 3
+++

## Let Bindings

Variables are bound in Sage using the `let` keyword. You can optionally include the type of the variable to be typechecked against.

```rs
// Create a variable named a, with the value 5
let a = 5;
let b = {x=5, y=6};
let c: (Int, Int, Int) = (1, 2, 3); // Optionally include the type

println(a);
println(b);
println(c);
```
{{< alert "code" >}}
**Output:**<br/>
5<br/>
\{x=5, y=6\}<br/>
(1, 2, 3)<br/>
{{< /alert >}}

### Mutability

You can make a variable as mutable with the `mut` keyword. If you try to assign to a variable that *isn't* mutable, the compiler will yell at you!

```rs
let mut a = 5;
println(a);
a = 6;
println(a);
```
{{< alert "code" >}}
**Output:**<br/>
5<br/>
6<br/>
{{< /alert >}}

## Destructured Let Bindings

In many instances, users want to be able to decompose the attributes of structures or tuples into variables.

```rs
let (a, {mut x, y}) = (1, {x=5, y=6});
println(a, ", ", x, ", ", y);
x = 999;
println(a, ", ", x, ", ", y);
```
{{< alert "code" >}}
**Output:**<br/>
1, 5, 6<br/>
1, 999, 6<br/>
{{< /alert >}}

## Variables And Scope

Variables are not captured by functions -- functions mainly act as procedures.
Additionally, variables may not be referenced before their definition.

```rs
println(a);
let a = 5;
```
{{< alert >}}
**Output:**<br/>
```
error: Error at text-box:0:8:8
  ┌─ text-box:1:9
  │
1 │ println(a);
  │         ^ LIR error: symbol a not defined
```
{{< /alert >}}
