+++
title = 'Control Flow'
date = 2024-09-10T10:42:38-04:00
series = ['Documentation']
series_order = 9
+++

## If Statements

If statements in Sage are similar to those in other languages. The syntax is as follows:

```rs
let a = 5;
if a == 5 {
    println("a is 5!");
} else {
    println("a is not 5!");
}
```
{{< alert "code" >}}
**Output:**<br/>
a is 5!<br/>
{{< /alert >}}

You can also use `else if` to chain multiple conditions together.

```rs
let a = 6;
if a == 5 {
    println("a is 5!");
} else if a == 6 {
    println("a is 6!");
} else {
    println("a is not 5 or 6!");
}
```
{{< alert "code" >}}
**Output:**<br/>
a is 6!<br/>
{{< /alert >}}

## Loops

Sage has two types of loops: `for` and `while`.

### For Loops

For loops in Sage are similar to those in C -- they have an initialization, a condition, and an increment.

```rs
for let mut i=0; i<5; i+=1; {
    println(i);
}
```
{{< alert "code" >}}
**Output:**<br/>
0<br/>
1<br/>
2<br/>
3<br/>
4<br/>
{{< /alert >}}

### While Loops

While loops are also similar to those in C.

```rs
let mut i = 0;
while i < 5 {
    println(i);
    i += 1;
}
```
{{< alert "code" >}}
**Output:**<br/>
0<br/>
1<br/>
2<br/>
3<br/>
4<br/>
{{< /alert >}}

## Match Statements

Match statements in Sage are similar to `switch` statements in C, but more powerful. They can be used to destructure data from `struct`s, `enum`s, tuples, and match against constant expressions.

```rs
enum Direction {
    North,
    South,
    East,
    West
}

let dir = Direction of North;
match dir {
    of North => println("Going North!"),
    of South => println("Going South!"),
    of East => println("Going East!"),
    of West => println("Going West!")
}

let point = {x=0, y=1};
match point {
    {x=0, y=0} => println("At the origin!"),
    {x=0, y} => println("On the y-axis at (0, ", y, ")"),
    {x, y=0} => println("On the x-axis at (", x, ", 0)"),
    _ => println("Somewhere else!")
}
```
{{< alert "code" >}}
**Output:**<br/>
Going North!<br/>
At the origin!<br/>
{{< /alert >}}

## If Let Statements

If let statements are a shorthand for matching against an enum and binding the value to a variable.

```rs
enum Option<T> {
    Some(T),
    Nothing
}

let o = Option<Int> of Some(5);

if let of Some(x) = o {
    println("The value is ", x);
} else {
    println("The value is Nothing");
}
```
{{< alert "code" >}}
**Output:**<br/>
The value is 5<br/>
{{< /alert >}}