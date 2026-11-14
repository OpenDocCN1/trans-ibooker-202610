
###### Figure 1.48: Stepping over the code

8. Seeing the variables change after the execution of each line of code makes the algorithm much clearer to understand. As you press `F6`, note that the following are the values after each execution of the `t += i++;` line:

![Figure 1.49: Variable states through time](image/C14508_01_49.jpg)

###### Figure 1.49: Variable states through time

The preceding output clearly explains how the values are changing and why `6` is printed at the end.

9. Explore other features of the debugger. While the variable view is useful, you can also hover over any variable and browse its value:

![Figure 1.50: View option of the debugger](image/C14508_01_50.jpg)

###### Figure 1.50: View option of the debugger

Furthermore, the **Expression** view helps you calculate things that are otherwise not clear from the values that you browse.

10. Click on **Expression** on the right-hand side and click on the **Add** button:

![Figure 1.51: Adding an expression](image/C14508_01_51.jpg)

###### Figure 1.51: Adding an expression

11. Type `t+i` and hit `Enter`. Now you see the total in the list of expressions:

###### 

![Figure 1.52: Expression view with a new expression](image/C14508_01_52.jpg)

###### Figure 1.52: Expression view with a new expression

You can press the red square in the toolbar or select `Run` | `Terminate` to stop debugging at any time. Another feature is breakpoints, which tell the debugger to pause whenever it goes to a line marked with a breakpoint. So far, we have been stepping through our code line by line, which may be very time-consuming in a large project. Instead, you usually want to continue the execution until it arrives at the code that you are interested in.

12. Now, instead of going line by line, add a breakpoint in the line that does the printing. For this, double-click on the area to the left of the line number of this line. In the following figure, the dot represents a breakpoint:

![Figure 1.53: Working with breakpoints](image/C14508_01_53.jpg)

###### Figure 1.53: Working with breakpoints

13. Now start the debugger. As usual, it will start paused. Now select `Run` | `Resume` or click on the toolbar button. It will run the three executions of the loop and pause at our breakpoint. This way, we saved time by stepping through code that we are not investigating:

###### 

![Figure 1.54: Working with the debugger](image/C14508_01_54.jpg)

###### Figure 1.54: Working with the debugger

14. While we have been dealing with the loop that we added, we ignored the line that creates an `app` object. The **Step Over** command was skipping this line. However, we also have the option to go into the constructor call that is in this line. For that, we will use `Run` | `Step Into` or the corresponding toolbar button.

15. Stop the debugger and start it again. Click on **Step Over** to go to the line where the application is created:

###### 

![Figure 1.55: Working with the debugger – the Step Over option](image/C14508_01_55.jpg)

###### Figure 1.55: Working with the debugger – the Step Over option

16. The highlighted is the next line that would be executed if we step over again. Instead, press the Step Into button. This will take us into the constructor call:

###### 

![Figure 1.56: Working with the debugger – the Step Into option](image/C14508_01_56.jpg)

###### Figure 1.56: Working with the debugger – the Step Into option

This is a handy feature for diving deeper into the function instead of simply stepping over it. Also, notice the call stack in the left debug view. You can always click on the lower entries to go and view the callers' contexts again.

This was a brief introduction to the Eclipse CDT debugger, which uses `GDB` under the hood to give you a visual debugging experience. You may find debugging useful when trying to understand runtime errors better and correcting the mistakes that caused them.

# Writing Readable Code

While visual debuggers are quite useful to identify and eliminate runtime errors or unintended program behavior, it is a better idea to write code that is less likely to have problems to begin with. One way to do that is to strive to write code that is easier to read and to understand. Then, finding problems in code becomes more like identifying contradictions between English sentences and less like solving cryptic puzzles. When you are writing code in a way that is understandable, your mistakes will often be apparent as you are making them and will be easier to spot when you come back to solve problems that slipped through.

After some unenjoyable maintenance experiences, you realize that the primary purpose of the programs that you write is not to make the computer do what you want to, but to tell the reader what the computer will do when the program runs. This usually means that you need to do more typing, which IDEs can help with. This may also mean that you sometimes write code that is not the most optimal in terms of execution time or memory used. If this goes against what you have learned, consider that you may be trading a minuscule amount of efficiency for the risk of being incorrect. With the vast processing power and memory at our disposal, you may be making your code unnecessarily cryptic and possibly buggy in the vain quest for efficiency. In the next sections, we will list some rules of thumb that may help you write code that is more readable.

## Indentation and Formatting

C++ code, as in many other programming languages, is composed of program blocks. A function has a set of statements that form its body as a block. A loop's block statements will execute in iterations. An `if` statement's block executes if the given condition is true and the corresponding `else` statement's block executes otherwise.

Curly braces, or lack thereof for single-statement blocks, inform the computer, whereas indentation in the form of white space informs the human reader about the block structure. The lack of indentation, or misleading indentation, can make it very difficult for the reader to understand the structure of the code. Therefore, we should strive to keep our code well-indented. Consider the following two code blocks:

```
// Block 1
if (result == 2)
    firstFunction();
secondFunction();

// Block 2
if (result == 2)
    firstFunction();
secondFunction();
```

While they are identical in terms of execution, it is much clearer in the second one that `firstFunction()` is executed only if `result` is `2`. Now consider the following code:

```
if (result == 2)
    firstFunction();
    secondFunction();
```

This is simply misleading. If the reader is not careful, they might easily assume that `secondFunction()` is executed only if `result` is `2`. However, this code is identical to the two previous examples in terms of execution.

If you feel like correcting indentation is slowing you down, you can use your editor's formatting facilities to help you. In Eclipse, you can select a block of code and use `Source` | `Correct Indentation` to fix the indentation of that selection, or use `Source` | `Format` to also fix other formatting issues with code.

Beyond indentation, other formatting rules such as placing the curly brace at the correct line, inserting spaces around binary operators, and inserting a space after each comma are also very important formatting rules that you should abide by to keep your code well-formatted and easy to read.
