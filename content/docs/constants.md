+++
title = 'Constants'
date = 2024-09-09T16:39:07-04:00
series = ['Documentation']
series_order = 5
+++

## Constants

Constants are immutable values that are defined at compile time. They are useful for defining compile time parameters, such as the size of an array.

```rs
const A = 5;
println(A);
```
{{< alert "code" >}}
**Output:**<br/>
5<br/>
{{< /alert >}}


You can use constants in type definitions as well.

```rs
const A = 5;
type FiveLong<T> = [T * A];

let a: FiveLong<Int> = [1, 2, 3, 4, 5];
println(a);
```
{{< alert "code" >}}
**Output:**<br/>
[1, 2, 3, 4, 5]<br/>
{{< /alert >}}

For now, constants are limited to very simple expressions, but we plan to expand this in the future. Ideally, users should be able to run arbitrary Sage code at compile time to define constants.