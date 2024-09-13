+++
title = 'Sage-Lisp Preprocessor Macros'
date = 2024-09-13T00:49:19-04:00
series = ['Documentation']
series_order = 15
+++

{{<alert "lightbulb">}}
**The Sage Preprocessor:** Sage also provides a preprocessor to write *AST-based macros*. These macros are written in **Lisp**, and operate on the AST before the code is handed to the compiler.
This lets you write compile-time code that configures the compiler, or rewrites your own code before it is compiled!
{{</alert>}}

Here's a simple demonstration of a macro that squares constant integers before they are compiled:

```swift
// Use the `#![...]` syntax to start a compile time directive.
// Our directive is a function definition, which will be used as
// a macro. It takes an AST node `x`, and returns a new AST node
// that squares the constant integer.
#![(defun square_sage_const(x) {
    (define n x@"ConstExpr"@"Int")
    ["ConstExpr" ["Int" (* n n)]]
})]

// Now we can use the macro to square our constant integer!
println(#[square_sage_const] 5);
```

{{<alert "code">}}
**Output:**<br/>
25
{{</alert>}}

You can write entire programs that compile and run at compile-time -- the possibilities are endless! If you want to inspect the AST of a value being compiled, you can define a macro that simply prints the AST-node and returns it.

```swift
#![{
    (println "Why not just use Sage as a Sage-Lisp interpreter?")

    (defun fact(n) {
        (if n <= 1
            {1}
            {n * (fact n - 1)})
    })

    (println (fact 5))
}]
```

You can learn more about Sage-Lisp at the repository [here](https://github.com/adam-mcdaniel/sage-lisp). 