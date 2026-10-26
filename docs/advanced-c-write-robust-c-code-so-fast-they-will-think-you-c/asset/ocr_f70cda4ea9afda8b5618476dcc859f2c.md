In the preceding screenshot, the assertion throws an error as the **ip** length that was passed is greater than **16**.

+   8. Now, to satisfy the **assert** condition so that the binary runs fine, we need to update the value of **ip** inside the **main** function. Write the following code to do this:

```
ip = "111.111.111.111";
```

Again, open the compiler, compile the **.cpp** file, and run the binary file that was generated. The following output is displayed in the compiler:

![](bbox=[0.065, 0.28, 0.865, 0.455])

Figure 7.3: Running the Assertion binary on the Command Prompt

# Note

As we're just dealing with **assert** here, we haven't added any extra functionality to our **checkValidIP()** function. However, we'll use the same example in the *Exception Handling* and *Unit Testing* sections wherein we'll add more functionality to our function.

+   9. If we don't want the executable to abort due to assert in production or the release environment, remove the **assert** macro call from the code. First, we'll update the value of **ip** whose length is greater than **16**. Add the following code to the file:

```
ip = "111.111.111.11111";
```

+   10. Now, during compilation, pass the **-DNDEBUG** macro. This will make sure that the assert is not called in the binary. Write the following command to compile our **.cpp** file in the Terminal:

```
g++ -DNDEBUG AssertSample.cpp
```