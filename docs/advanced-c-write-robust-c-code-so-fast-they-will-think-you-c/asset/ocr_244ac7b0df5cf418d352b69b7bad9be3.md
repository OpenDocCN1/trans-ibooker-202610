Later, when **displayStackStats** is called, it shows the correct **size** of 3. But when the top command is called, there is the wrong value shown in the head. This is highlighted in red ink. Now, the code for the top command doesn't alter the value of head, so it is clear that an error occurred in the previous statement of execution, that is, at **displayStackStats**.

So, we have narrowed down the code where there may be an issue. We can run the debugger to point **displayStackStats** and move inside **displayStackStats** to find the reasons that caused the values to be altered inside the Stack. The following is a screenshot of the same wherein the user needs to start the debugger from the beginning:

![](bbox=[0.205, 0.31, 0.815, 0.92])

Figure 7.20: Debugger display – IV