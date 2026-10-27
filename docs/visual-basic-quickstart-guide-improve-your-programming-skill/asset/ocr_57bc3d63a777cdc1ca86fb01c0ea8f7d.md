An infinite loop is a loop that continues indefinitely without a condition that can make it terminate. The endless loop can happen unintentionally or as part of the program’s logic. Infinite loops can cause a program to become unresponsive, consume excessive system resources, or crash.

Here’s an example of an infinite loop in VB:

```
While 1=1
    'code to run
End While
```

In this example, the 1=1 condition always evaluates to true so that the loop will continue indefinitely. To break out of this loop, you would need to manually interrupt the program execution, such as by pressing Ctrl + C.

While infinite loops are usually unintended and considered bugs, there are cases where intentionally creating infinite loops can be helpful. For instance, an infinite loop can be used in server applications to listen for and handle client requests continuously.

It’s crucial to ensure that the loop condition is appropriately defined and will eventually evaluate to a false value to avoid unintended infinite loops. Additionally, using loop control statements such as exit or return inside the loop can provide a way to exit the loop under certain conditions.

# Summary

This chapter introduced iteration with For, While, and Do loops. Iteration is used to execute blocks of code multiple times. We also discussed special statements to break out of loops. In the next chapter, we will continue our journey with a look at modularizing our code to make it easier to reuse code.