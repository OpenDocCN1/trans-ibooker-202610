# SIDEBAR

# Variable-naming conventions

It's a common convention to use names like f, g, and h for parameters to a higher-order function. In functional programming, we tend to use very short variable names, even one-letter names. This is usually because HOFs are so general that they have no opinion on what the argument should actually do. All they know about the argument is its type. Many functional programmers feel that short names make code easier to read, since it makes the structure of the code easier to see at a glance.

Our function `abs` from before matches that type; it accepts an `Int` and returns an `Int`. Likewise, `factorial` accepts an `Int` and returns an `Int`, which also matches the `(Int) -> Int` type. We can therefore pass `abs` or `factorial` as the `f` argument to `formatResult` as we do in the following two cases inside our `main` method:

```
fun main() {
    println(formatResult("factorial", 7, ::factorial))
    println(formatResult("absolute value", -42, ::abs))
}
```

A namespace prefix, `::`, is added in order to reference the `factorial` and `abs` functions. You can find more explanation about accessing and namespacing function references in the *Functions as values* sidebar.