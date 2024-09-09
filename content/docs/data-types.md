+++
title = 'Data Types'
date = 2024-09-09T16:40:07-04:00
series = ['Documentation']
series_order = 5
+++


{{< alert "lightbulb" >}}
**About the typesystem:** Sage's type system is based on [parameterized algebraic data types](https://en.wikipedia.org/wiki/Generalized_algebraic_data_type). This gives Sage a much more expressive set of primitives than languages like C! 
{{< /alert >}}

Sage's type system is built on structural type equality, meaning that two types are equal if they have the same structure. This allows for more flexible type definitions, and makes it easier to work with complex data structures.

## Primitive Types

The following are the primitive types in Sage:

- *`Int`*: an integer, the word size of the target architecture. If you're compiling to a 64-bit target, `Int` is 64 bits.
- *`Cell`*: a raw word-sized value, used for reinterpreting memory. To reinterpret a `Float` as an `Int`, you can cast it to a `Cell` first: `5.0 as Cell as Int`.
- *`Float`*: a floating point number, the word size of the target architecture. This is a `double` for 64-bit targets.
- *`Bool`*: a boolean value, either `True` or `False`.
- *`Char`*: a word-sized character.
- *`[T * N]`*: an array of `N` elements of type `T`.
- *`&T`*: an immutable pointer to a value of type `T`.
- *`&mut T`*: a mutable pointer to a value of type `T`.
- *`fun(T1, T2, ..., Tn) -> T`*: a function that takes arguments of types `T1`, `T2`, ..., `Tn` and returns a value of type `T`.

## Pointers

To get a pointer of a value, you can use the `&` operator. To get a mutable pointer, you can use the `&mut` operator.

```rs
let a = 5;
let p: &Int = &a;

let mut b = 6;
let p2 = &mut b;

println(*p);
*p2 = 1000;
println(*p2);
```
{{< alert "code" >}}
**Output:**<br/>
5<br/>
1000<br/>
{{< /alert >}}


If you take a mutable reference of an immutable value, the compiler will throw an error.
```rs
let a = 5;
let p: &mut Int = &mut a;
```
{{< alert >}}
**Error:**<br/>
`2 │ let p: &mut Int = &mut a;`<br/>
LIR error: invalid refer expression &mut a
{{< /alert >}}

## Algebraic Data Types

Sage supports algebraic data types, which are a way of defining complex data structures. These types are defined using the `type`, `struct`, and `enum` keywords, and can be parameterized with other types.

### Structs

Structs are a way of defining a collection of named fields. They are defined using the `struct` keyword.

```rs
struct Point {
  x: Int,
  y: Int,
}

let p: Point = { x=5, y=6 };
println(p);
```
{{< alert "code" >}}
**Output:**<br/>
\{x=5, y=6\}<br/>
{{< /alert >}}

### Enums

Enums are a way of defining a type that can be one of several variants. They are defined using the `enum` keyword.

```rs
enum Option<T> {
  Some(T),
  Nothing,
}

let o = Option<Int> of Some(5);

match o {
    of Some(val) => println("there is a value: ", val),
    of Nothing => println("there is no value")
}
```
{{< alert "code" >}}
**Output:**<br/>
there is a value: 5<br/>
{{< /alert >}}

### Type Aliases

Type aliases are a way of defining a new name for an existing type. They are defined using the `type` keyword.

```rs
type Point = { x: Int, y: Int };

let p: Point = { x=5, y=6 };
println(p);
```
{{< alert "code" >}}
**Output:**<br/>
\{x=5, y=6\}<br/>
{{< /alert >}}

### Type Parameters

Types can be parameterized with other types. This allows for more flexible type definitions.

```rs
type Pair<T, U> = { first: T, second: U };

let p: Pair<Int, Char> = { first=5, second='a' };
println(p);
```
{{< alert "code" >}}
**Output:**<br/>
\{first=5, second='a'\}<br/>
{{< /alert >}}


### Const-Generics

Sage supports const-generics, which are a way of defining types that are parameterized by constants.

```rs
struct Matrix<Elem, const Rows: Int, const Cols: Int> {
    data: [[Elem * Cols] * Rows],
}

let m: Matrix<Int, 2, 2> = { data=[[1, 2], [3, 4]] };
println(m);
```
{{< alert "code" >}}
**Output:**<br/>
\{data=[[1, 2], [3, 4]]\}<br/>
{{< /alert >}}
