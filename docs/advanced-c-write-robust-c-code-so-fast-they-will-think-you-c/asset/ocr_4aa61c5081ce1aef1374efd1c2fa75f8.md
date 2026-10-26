```
```cpp
/// work on error condition
// if needed exit program gracefully.
return -1;
}
return 0;
}
```
```

+   5. Open the terminal, compile our file, and run the binary. You will see the following output:

![](bbox=[0.074, 0.275, 0.867, 0.423])

**Figure 7.8: Example execute code with exception handling**

The preceding example throws an exception for **ip** being **NULL** and exits gracefully.

+   6. Now, modify the value of **ip** in the **main** function by providing more than **15** characters. Write the following code to do this:

```
```cpp
ip = "111.111.111.11111";
```
```

+   7. Open the terminal, compile our file, and run the binary. You will see the following output:

![](bbox=[0.084, 0.653, 0.866, 0.827])

**Figure 7.9: Another example of exception handling**

It throws an error with a **length mismatch** for the **ip string**.