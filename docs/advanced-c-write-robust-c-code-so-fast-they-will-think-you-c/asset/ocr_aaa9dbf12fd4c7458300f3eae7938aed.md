This is a handy feature for diving deeper into the function instead of simply stepping over it. Also, notice the call stack in the left debug view. You can always click on the lower entries to go and view the callers' contexts again.

This was a brief introduction to the Eclipse CDT debugger, which uses GDB under the hood to give you a visual debugging experience. You may find debugging useful when trying to understand runtime errors better and correcting the mistakes that caused them.

# Writing Readable Code

While visual debuggers are quite useful to identify and eliminate runtime errors or unintended program behavior, it is a better idea to write code that is less likely to have problems to begin with. One way to do that is to strive to write code that is easier to read and to understand. Then, finding problems in code becomes more like identifying contradictions between English sentences and less like solving cryptic puzzles. When you are writing code in a way that is understandable, your mistakes will often be apparent as you are making them and will be easier to spot when you come back to solve problems that slipped through.

After some unenjoyable maintenance experiences, you realize that the primary purpose of the programs that you write is not to make the computer do what you want to, but to tell the reader what the computer will do when the program runs. This usually means that you need to do more typing, which IDEs can help with. This may also mean that you sometimes write code that is not the most optimal in terms of execution time or memory used. If this goes against what you have learned, consider that you may be trading a minuscule amount of efficiency for the risk of being incorrect. With the vast processing power and memory at our disposal, you may be making your code unnecessarily cryptic and possibly buggy in the vain quest for efficiency. In the next sections, we will list some rules of thumb that may help you write code that is more readable.

# Indentation and Formatting

C++ code, as in many other programming languages, is composed of program blocks. A function has a set of statements that form its body as a block. A loop's block statements will execute in iterations. An **if** statement's block executes if the given condition is true and the corresponding **else** statement's block executes otherwise.