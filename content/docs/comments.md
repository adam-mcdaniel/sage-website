+++
title = 'Comments'
date = 2024-09-10T10:39:51-04:00
series = ['Documentation']
series_order = 8
+++


## Single Line Comments

Single line comments are created with `//`. Anything after `//` on a line is ignored by the compiler.

```rs
// This is a comment
let a = 5; // This is another comment
println(a); // This is a third comment
```
{{< alert "code" >}}
**Output:**<br/>
5<br/>
{{< /alert >}}

## Multi Line Comments

Multi line comments are created with `/*` and `*/`. Anything between `/*` and `*/` is ignored by the compiler.

```rs
/* This is a comment
   that spans multiple lines */
let a = 5;
println(a);
```
{{< alert "code" >}}
**Output:**<br/>
5<br/>
{{< /alert >}}

## Nested Comments

Sage allows nested comments, which can be useful for commenting out large blocks of code.

```rs
/* This is a comment
   /* that is nested */
   // This is a nested single line comment in a block comment
   and spans multiple lines */
let a = 5;
println(a);
```
{{< alert "code" >}}
**Output:**<br/>
5<br/>
{{< /alert >}}