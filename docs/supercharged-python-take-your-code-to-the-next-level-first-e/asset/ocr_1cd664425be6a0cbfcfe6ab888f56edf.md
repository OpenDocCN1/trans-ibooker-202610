variables. But so does the following code, which—upon being
executed—creates a variable `foo` if it does not already exist.

Click here to view code image

```
```
def my_func():
    global foo
    foo = 5     # Create foo if it does not already
                # exist (as a global).
    print(foo)
```
```

Assuming `foo` does not already exist, the effect of this
function is to create `foo` and set it to 5. It cannot be created as a
local—because of the statement **global `foo`**—and therefore
`foo` is created as a global variable. This works even though the
assignment to `foo` is not part of module-level code.

In general, there is a golden rule about global and local
variables in Python. It’s simply this:

> *
> If there’s any chance that a function might attempt to assign a value to a global
> variable, use the `global` statement so that it’s not treated as local.

# # CHAPTER 1 SUMMARY

Chapter 1 covers the fundamentals of Python except for class
definitions, advanced operations on collections, and specialized
parts of the library such as file operations. The information
presented here is enough to write many Python programs.

So congratulations! If you understand everything in this