# # Exceptions

# ## 8.1. Errors: notification and propagation

Errors may occur when executing a program. They may result from choices made by the programmer who may specify certain conditions, which must stop the execution. For example, a program to compute the average of a list of ages will not be able to handle a negative age value in the same way as a positive value. Other kinds of errors may occur that are not directly related to the programmer's intent, for example an attempt to divide by 0 or an attempt to illegally access a file due to missing permissions.

To produce a robust program, any programmer must be aware of these possible errors and should prevent their consequences when they occur. The most basic solution is to include multiple checks throughout a program and to implement a mechanism to highlight incorrect results, from error detection to management. The result obtained from each function call should be checked and if this value indicates that an error occurred during the execution of this call, then an error must be returned. A number of possible approaches for error notification and handling are outlined below using a simple pseudo-language.

```
f ()
    ...
    if ... then return error
    ...
g ()
    r = f ()
    if r is error then return error
    ...
h ()
    ...
    r = g ()
```