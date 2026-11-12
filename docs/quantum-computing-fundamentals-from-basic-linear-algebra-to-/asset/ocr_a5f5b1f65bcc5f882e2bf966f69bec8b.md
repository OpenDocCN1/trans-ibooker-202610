As you can see, the `BooleanFunction`, in turn, calls the `UnitaryOperation` function. If you carefully studied the previous code samples and reviewed the Deutsch-Jozsa algorithm, this code example should help you to better understand both the algorithm and Q# programming.

# ## 16.5 Bit Flipping

Let us try one more example. This code simply demonstrates bit flipping. This example is quite similar to one found on Microsoft's website at https://docs.microsoft.com/en-us/quantum/tutorials/explore-entanglement. The code on the Microsoft site goes further and simulates entanglement. You should consider working through that example as well.

First, let's look at the code. In this case, all the code is shown in Figure 16.20.

![](bbox=[0.07, 0.37, 0.56, 0.76])

FIGURE 16.20 Entanglement

First notice that this code depends a great deal on gates and measurements. You see the X operation and the M operation that were discussed previously in this chapter. Recall that X flips the state, and M measures it. The entry point is the operation `TestState`, which tests the state of some qubit.

The function simply takes in a number of iterations to go through and an initial value. Then it will go through flipping bits and outputting the result. This is a rather simple program, but it will help you get more comfortable with Q#.