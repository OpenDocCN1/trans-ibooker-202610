# The Loop

In this case, the required number of repetitions is achieved by counting the loop turns performed so far. For that purpose, you use the count variable.

At the beginning (initializer), the variable is set to zero.
After completing every loop turn (iterator), the variable is incremented by one.
The loop body (the display of a line of text) is repeated as long as (loopCondition) the number of lines in the output has not reached ten. As soon as the count variable becomes ten, the condition (count < 10 or 10 < 10) will no longer be fulfilled, the loop will terminate, and the computer will continue executing the statements following the loop.

# Explore It Yourself

You should take the time to explore the inner workings of loops to grasp them thoroughly. Use debugging tools you already know: stepping and inspecting the count variable.

# Tip

Visual Studio can help you write a for loop without mistakes. Just enter for, press the Tab key twice, and edit the generated loop header.

# Choosing the Number of Repetitions

The for loop allows you to solve cases when you do not know the number of repetitions in advance (at the time of code writing).

# Task

You will modify the previous exercise to let the user specify the number of the sentence repetitions (see Figure 20-3).