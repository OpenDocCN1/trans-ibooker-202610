After following the above steps, the expected output looks like the following:

```
------ Activity 1 ------

------ Delegate - no args ------
Hello, World
------ Delegate - string argument ------
hello!-hello?-hello@-
------ Delegate - binary int argument ------
52 + 4 = 56
52 - 4 = 48
52 * 4 = 208
52 / 4 = 13

Complete.
```

Figure 4.34: Output from the successful implementation of Delegate

# Note

The solution for this activity can be found on page 673.

# Summary

In this chapter, we implemented a data and method hiding design approach, PIMPL, that has the added benefit of reducing dependencies and reducing build times. We then implemented function objects directly as custom classes and then as lambda functions. We then expanded our template programming skills by diving into variadic templates culminating in a template which can be used for event callback processing. In the next chapter, we will learn how to use the features of C++ to develop programs with multiple thread and manage their co-operation through concurrency constructs.