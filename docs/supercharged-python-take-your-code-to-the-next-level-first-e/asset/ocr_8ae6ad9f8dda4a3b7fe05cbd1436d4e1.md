use of `nonlocal` is rare.
See the `global` statement for more information.

# # pass Statement

This statement has a simple syntax:

```
```
pass
```
```

This statement is essentially a no-op. It does nothing at run time, and its major use is to act as a stand-in or placeholder inside a class or function definition—that is, it holds a place to be filled in later with other statements.

Click here to view code image

```
```
class Dog:
    pass      # This class has no methods yet.
```
```

# # raise Statement

This statement has the following syntax, in which square brackets are not intended literally but indicate optional items.

Click here to view code image

```
```
raise [exception_class [(args)]]
```
```

The effect is to raise the specified exception, with optional arguments. Once an exception is raised, it must be handled by the program or else it causes rude and abrupt termination. An exception handler can rethrow an exception by using