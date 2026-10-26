+   10. Open **Exercise4.cpp** in the editor and change the line near the top of the file to read like so:

```
#define EXERCISE4_STEP 12
```

+   11. Click on the **Run** button to compile and run the program. The following output is generated:

![](bbox=[0.084, 0.225, 0.857, 0.594])

Figure 3.32: Annotated Step 12 output for Exercise 4

+   12. Compare the output with the code in the **testSensors()** method. We'll find that we can easily assign to an empty **unique_ptr** (**light**) and that we can assign from one **shared_ptr** to another (**light3 = light2**) without the need for **std::move()** in either case.

+   13. Open **Exercise4.cpp** in the editor and change the line near the top of the file to read like so:

```
#define EXERCISE4_STEP 15
```