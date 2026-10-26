Now the code is much clearer. While it's very easy to understand what each block is doing, we also added comments to make it even more clear. In this section, we gained a better understanding of how our code is converted to executables. Then, we discussed ways of identifying and resolving possible errors with our code. We finalized this with a discussion about how to write readable code that is less likely to have problems. In the next section, we will solve an activity wherein we will be making code more readable.

# Activity 3: Making Code More Readable

You may have code that is unreadable and contains bugs, either because you wrote it in a hurry, or you received it from someone else. You want to change the code to eliminate its bugs and to make it more readable. We have a piece of code that needs to be improved. Improve it step by step and resolve the issues using a debugger. Perform the following steps to implement this activity:

+   1. Below you will find the source for **SpeedCalculator.cpp** and **SpeedCalculator.h**. They contain the **SpeedCalculator** class. Add these two files to your project.
+   2. Create an instance of this class in your **main()** function and call its **run()** method.
+   3. Fix the style and naming problems in the code.
+   4. Simplify the code to make it more understandable.
+   5. Run the code and observe the problem at runtime.
+   6. Use the debugger to fix the problem.

Here's the code for **SpeedCalculator.cpp** and **SpeedCalculator.h** that you will add to your project. You will modify them as a part of this activity:

```
// SpeedCalculator.h
#ifndef SRC_SPEEDCALCULATOR_H_
#define SRC_SPEEDCALCULATOR_H_

class SpeedCalculator {
private:
    int numEntries;
    double *positions;
    double *timesInSeconds;
    double *speeds;
public:
    void initializeData(int numEntries);
    void calculateAndPrintSpeedData();
};
```