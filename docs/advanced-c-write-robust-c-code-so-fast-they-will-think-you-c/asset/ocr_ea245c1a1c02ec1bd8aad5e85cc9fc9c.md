We can write the following test cases for it:

+   - With only extra characters ("  "), returns empty string ().
+   - With only empty characters in beginning("  AA") return string with end characters ("AA")
+   - With only empty characters at end("AA  "), should return string with begin characters("AA")
+   - With characters in middle ("  AA  ") return string with characters ("AA")
+   - With empty space in middle ("AA  BB"), return the same string ("AA BB")
+   - All steps 3,4,5 with single character. Should return string with single character.

To create test cases , please check file **ParseFileTestCases.cpp**, test cases for `trim` function are written in test suite `trim`. Now write **Util.cpp** file (All miscellaneous functions are written in **Util.cpp**). Write `trim` function with signature shown above in file. Execute test cases of `trim` function and check if it passes. It doesn't change the function appropriately and test it again. Repeat till all test cases pass.

Now we are confident to use `trim` function in the project. Repeat similar steps for the rest of the common functions (`isDigit`, `isNumeric`, `parseHeader` and so on). Please refer the **Util.cpp** file and **ParseFiletestCases.cpp** for the same and test all the common functions.

After completing common functions, we can write parse each file function separately. Main thing to understand and learn here is how to break down module to small functions. Find small repetitive tasks and create small functions for each so it is refactored. Understand the detail functionality of these small functions and create appropriate unit test cases.

Complete single function and thoroughly test it, if it fails, then update the function till it passes all test cases. Similarly, complete other functions. Then write and execute test cases for bigger functions, that should be relatively easy since we are calling small functions tested above in these bigger functions.