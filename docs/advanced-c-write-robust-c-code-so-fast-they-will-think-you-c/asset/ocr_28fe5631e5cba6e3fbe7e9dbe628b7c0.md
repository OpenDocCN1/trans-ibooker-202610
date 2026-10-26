12. Click on the Run button to rerun the tests – this time, we have added two more tests that fail – AddFractions and AddFractions2:

![](bbox=[0.15, 0.14, 0.92, 0.39])

Figure 2A.42: Additional tests to fail

13. Locate the operator+= function in the Function.cpp file.

14. Make the necessary changes to the function and click on the Run button to rerun the tests until the tests pass. Look at the equations that define its operation given earlier and see how operator*=( ) was implemented.

15. Open Fraction.hpp in the editor and change the line near the top of the file to read like so:

```
#define EXERCISE7_STEP 15
```

16. Click on the Run button to rerun the tests – his time, we have added two more tests that fail – SubtractFractions and SubtractFractions2.

17. Locate the operator-= function in the Function.cpp file.

18. Make the necessary changes to the function and click on the Run button to rerun the tests until the tests pass.

19. Open Fraction.hpp in the editor and change the line near the top of the file to read like so:

```
#define EXERCISE7_STEP 19
```

20. Click on the Run button to rerun the tests – this time, we have added two more tests that fail – DivideFractions and DivideFractions2.