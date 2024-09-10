+++
title = 'Methods And Associated Constants'
date = 2024-09-10T10:45:48-04:00
series = ['Documentation']
series_order = 10
+++

## Associated Constants

Types can have associated constants, which allow users to access the constant through the `.` operator. Associated constants are defined with the `const` keyword in an `impl` block.

```rs
struct Point {
    x: Int,
    y: Int,
}

// The `impl` block allows us to define methods and constants
// which are associated with the given type.
impl Point {
    const ORIGIN = {x=0, y=0};
}

let p = Point.ORIGIN;
println(p);
```
{{< alert "code" >}}
**Output:**<br/>
\{x=0, y=0\}
{{< /alert >}}

## Associated Functions

Associated functions are similar to associated constants, but they are functions instead of values. They are defined with the `fun` keyword in an `impl` block.

```rs

struct Point {
    x: Int,
    y: Int,
}

impl Point {
    const ORIGIN = {x=0, y=0};

    // A constructor takes no `self` parameter.
    // It is a static function that returns a new
    // instance of the type.
    fun new(x: Int, y: Int): Point {
        return {x, y};
    }
}

let p = Point.new(4, 5);
println(p);
```
{{< alert "code" >}}
**Output:**<br/>
\{x=4, y=5\}
{{< /alert >}}

These functions don't take an instance of `Point`, but they're still accessed through the `Point` type. This is useful for creating constructors or other static functions that are associated with the type.

## Methods

This same mechanism allows us to define methods for our types. Methods are associated functions that allow us to mutate the underlying data, or perform operations on it without mutating it.

```rs
// A function for computing the absolute value of an integer
fun abs(x: Int): Int {
    if x < 0 {
        return -x;
    }
    return x;
}

struct Point {
    x: Int,
    y: Int,
}

impl Point {
    const ORIGIN = {x=0, y=0};

    // A constructor takes no `self` parameter.
    // It is a static function that returns a new
    // instance of the type.
    fun new(x: Int, y: Int): Point {
        return {x, y};
    }

    // A method takes a `self` parameter.
    // This method mutates the `Point` instance,
    // so it takes a mutable reference.
    fun translate(&mut self, dx: Int, dy: Int) {
        self.x += dx;
        self.y += dy;
    }

    // Calculate the manhattan distance between two points
    // This method does not mutate the `Point` instance,
    // so it takes an immutable reference.
    fun manhattan_distance(&self, other: &Point): Int {
        return abs(self.x - other.x) + abs(self.y - other.y);
    }

    // You can also define methods that don't take
    // a reference to `self`
    fun print(self) {
        println("{x=", self.x, ", y=", self.y, "}");
    }
}

// Create a new point, translate it, and print it
let mut p = Point.new(4, 5);
println(p);
p.translate(-5, 10);
p.print();
// Print the point's distance to the origin
let p2 = Point.ORIGIN;
println("Distance: ", p.manhattan_distance(&p2));
```
{{< alert "code" >}}
**Output:**<br/>
\{x=4, y=5\}<br/>
\{x=-1, y=15\}<br/>
Distance: 16
{{< /alert >}}

