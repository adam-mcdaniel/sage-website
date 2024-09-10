+++
title = 'Modules'
date = 2024-09-10T18:03:48-04:00
series = ['Documentation']
series_order = 12
+++

# Modules Within A File

Modules are a way to organize code within a file. They allow you to group related code together and make it easier to manage. Below is an example of a module declared within a file.

```rs
mod math {
    fun gcd(a: Int, b: Int): Int {
        if b == 0 {
            return a;
        }
        return gcd(b, a % b);
    }
}

println(math.gcd(12, 18)); // Output: 6
```
{{<alert "code">}}
**Output:**<br/>
6
{{</alert>}}

In the example above, we have a module called `math` that contains a function `gcd` which calculates the greatest common divisor of two numbers. We can access the `gcd` function by using the module name followed by a dot and the function name.

# Modules Across Files

Modules can also be imported from other files. You can do this with the `mod` keyword followed by the filename.

{{< alert "code" >}}
**Filename:** `math.sg`
{{< /alert >}}
```rs
fun gcd(a: Int, b: Int): Int {
    if b == 0 {
        return a;
    }
    return gcd(b, a % b);
}
```

{{< alert "code" >}}
**Filename:** `main.sg`
{{< /alert >}}
```rs
mod math;

println(math.gcd(12, 18)); // Output: 6
```
{{<alert "code">}}
`sage main.sg`<br/>
**Output:**<br/>
6
{{</alert>}}
<br/>

# Importing Items From Modules

You can import items from modules using the `from` and `import` keywords. Below is an example of importing the `gcd` function from the `math` module.

```rs
mod math {
    fun gcd(a: Int, b: Int): Int {
        if b == 0 {
            return a;
        }
        return gcd(b, a % b);
    }
}

from math import gcd;
println(gcd(12, 18)); // Output: 6
```
{{<alert "code">}}
**Output:**<br/>
6
{{</alert>}}

You may also import all items from a module using the `*` wildcard.

```rs
mod math {
    fun gcd(a: Int, b: Int): Int {
        if b == 0 {
            return a;
        }
        return gcd(b, a % b);
    }
}

from math import *;
println(gcd(12, 18)); // Output: 6
```
{{<alert "code">}}
**Output:**<br/>
6
{{</alert>}}
<br/>

# Renaming Imported Items

You can rename imported items using the `as` keyword. Below is an example of importing the `gcd` function from the `math` module and renaming it to `greatest_common_divisor`.

```rs
mod math {
    fun gcd(a: Int, b: Int): Int {
        if b == 0 {
            return a;
        }
        return gcd(b, a % b);
    }
}

from math import gcd as greatest_common_divisor;
println(greatest_common_divisor(12, 18)); // Output: 6
```
{{<alert "code">}}
**Output:**<br/>
6
{{</alert>}}
