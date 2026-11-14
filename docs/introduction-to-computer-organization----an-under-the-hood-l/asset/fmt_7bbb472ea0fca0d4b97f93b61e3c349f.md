
The Primary SR latch in [Figure 7-14](#c07.xhtml#figure7-14) is in its write mode when `CK = 0`. We want it to toggle the output, `Q`, when `J = K = 1`. Given these conditions, [Table 7-8](#c07.xhtml#table7-8) shows how `¬S` and `¬R` depend on `Ja` and `Ka` for the two possible values of `Q`. We saw how these values of `¬S` and `¬R` affect `Q` in [Table 7-2](#c07.xhtml#table7-2), which are copied here into [Table 7-8](#c07.xhtml#table7-8).

**Table 7-8: Additional Inputs to Add Toggle Function to SR Flip-Flop**

| **Ja** | **Ka** | **¬S** | **¬R** | **Q** | **Q[next]** |
| --- | --- | --- | --- | --- | --- |
| `0` | `0` | `1` | `1` | `0` | `0` |
| `0` | `1` | `1` | `0` | `0` | `0` |
| `1` | `0` | `0` | `1` | `0` | `1` |
| `1` | `1` | `0` | `0` | `0` | X |
| `0` | `0` | `1` | `1` | `1` | `1` |
| `0` | `1` | `1` | `0` | `1` | `0` |
| `1` | `0` | `0` | `1` | `1` | `1` |
| `1` | `1` | `0` | `0` | `1` | X |

The third and sixth rows of [Table 7-8](#c07.xhtml#table7-8) show that `JaKa = 10` toggles the state, `Q`, from `0` to `1`, and `JaKa = 01` toggles the state from `1` to `0`. This leads us to the design in [Figure 7-15](#c07.xhtml#figure7-15), with `Ja = ¬Q` and `Ka = Q`.

![Figure 7-15: JK flip-flop](image_fi/500099c07/f07015.png)

**Figure 7-15: JK flip-flop**

We should check that the feedback connections made in this circuit preserve the behavior of the other functions of the JK flip-flop. [Table 7-9](#c07.xhtml#table7-9) shows the state transition table for the JK flip-flop in [Figure 7-15](#c07.xhtml#figure7-15) with `¬S`, `¬R`, and `¬Q` added. Refer to [Table 7-2](#c07.xhtml#table7-2) for the relationship between `¬S`, `¬R`, and `Q[next]`.

**Table 7-9: JK Flip-Flop State Table for the Circuit in [Figure 7-15](#c07.xhtml#figure7-15)**

| | **J** | **K** | **Q** | **¬Q** | **¬S** | **¬R** | **Q[next]** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Keep | `0` | `0` | `0` | `1` | `1` | `1` | `0` |
| Keep | `0` | `0` | `1` | `0` | `1` | `1` | `1` |
| Reset | `0` | `1` | `0` | `1` | `1` | `1` | `0` |
| Reset | `0` | `1` | `1` | `0` | `1` | `0` | `0` |
| Set | `1` | `0` | `0` | `1` | `0` | `1` | `1` |
| Set | `1` | `0` | `1` | `0` | `1` | `1` | `1` |
| Toggle | `1` | `1` | `0` | `1` | `0` | `1` | `1` |
| Toggle | `1` | `1` | `1` | `0` | `1` | `0` | `0` |

Using three-input NAND gates at the input to this JK flip-flop does add some complexity to the circuit. The additional complexity is about the same as adding an XOR gate to a D flip-flop to get a T flip-flop (see [Figure 7-12](#c07.xhtml#figure7-12)). Although an SR flip-flop is a little less complex than a JK flip-flop if the toggle function is not needed, there are manufacturing cost advantages to having only one design. The JK flip-flop, by providing all four functions, also allows more flexibility, and hence cost savings, in design.

## Designing Sequential Logic Circuits

Now we'll consider a more general set of steps for designing sequential logic circuits. Design in any field is usually iterative, as you have no doubt learned from your programming experience.

You start with a design, analyze it, and then refine the design to make it faster, less expensive, and so forth. After gaining some experience, the design process usually requires fewer iterations. The following steps are a good method for building a first working design:

1.  From the word description of the problem, create a state transition table and state diagram showing what the circuit must do. These form the basic technical specifications for the circuit you will be designing.
2.  Choose a binary code for the states and create a binary-coded version of the state table and/or state diagram. For *N* states, the code will need *log*₂*N* bits. Any code will work, but some codes may lead to simpler combinational logic in the circuit.
3.  Choose a type of flip-flop. This choice is often dictated by the components you have on hand.
4.  Add columns to the state table that show the input required to each flip-flop to cause each of the required transitions.
5.  Simplify the inputs to each flip-flop. Karnaugh maps or algebraic methods are good tools for the simplification process.
6.  Draw the circuit.

The simplification step may cause you to rethink your choice of type of flip-flop. These three steps—flip-flop choice, determining inputs, simplification—may need to be repeated several times to get a good design. The following two examples illustrate this process.

### Designing a Counter

Rather than asking you to do all the work at this point, I'll go through two examples. If you have access to a digital circuit simulator, or the required hardware, I suggest that you use those resources to follow along. This is like a guided "Your Turn."

In this example, we want to design a counter that has an `Enable` input. When `Enable = 1`, it increments through the sequence 0, 1, 2, 3, 0, 1, ..., incrementing each clock tick. `Enable = 0` causes the counter to remain in its current state. The output is the sequence number in two-bit binary.

#### Step 1: Create a State Transition Table and State Diagram

At each clock tick, the counter increments by 1 if `Enable = 1`. If `Enable = 0`, it remains in the current state. [Figure 7-16](#c07.xhtml#figure7-16) shows the four states—0, 1, 2, 3—and the corresponding two-bit output for each state.

![Figure 7-16: State diagram for a counter that cycles through 0, 1, 2, 3, 0, …](image_fi/500099c07/f07016.png)

**Figure 7-16: State diagram for a counter that cycles through 0, 1, 2, 3, 0, …**

[Table 7-10](#c07.xhtml#table7-10) shows the state transition table.

**Table 7-10: State Transition Table for a Counter that Cycles Through 0, 1, 2, 3, 0, …**

| | **Enable = 0** | **Enable = 1** |
| --- | --- | --- |
| **Current *n*** | **Next *n*** | **Next *n*** |
| 0 | 0 | 1 |
| 1 | 1 | 2 |
| 2 | 2 | 3 |
| 3 | 3 | 0 |

When `Enable = 0`, the counter is essentially turned off, and when `Enable = 1`, the counter automatically increments by 1, wrapping around to 0 when it reaches its limit of 3.

#### Step 2: Create a Binary-Coded Version of the State Table and/or State Diagram

With four states, we need two bits. We will let *n* be the state, which we represent with the two-bit binary number `n[1]n[0]`. The behavior of the counter is shown in the state transition table, [Table 7-11](#c07.xhtml#table7-11).

**Table 7-11: State Transition Table for Two-Bit Counter**

| **Enable** | **Current *n*[1]** | **Current *n*[0]** | **Next *n*[1]** | **Next *n*[0]** |
| --- | --- | --- | --- | --- |
| `0` | `0` | `0` | `0` | `0` |
| `0` | `0` | `1` | `0` | `1` |
| `0` | `1` | `0` | `1` | `0` |
| `0` | `1` | `1` | `1` | `1` |
| `1` | `0` | `0` | `0` | `1` |
| `1` | `0` | `1` | `1` | `0` |
| `1` | `1` | `0` | `1` | `1` |
| `1` | `1` | `1` | `0` | `0` |

#### Step 3: Select a Flip-Flop

JK flip-flops are a good place to start because they provide all the functions. After going through the design, we may learn that a simpler flip-flop would work. We could then come back to this step and go through the remaining steps again. An experienced designer may have some insight into the problem that would suggest starting with another type of flip-flop. Often, any potential savings in cost or power consumption do not justify changing to another type of flip-flop.

#### Step 4: Add Columns to the State Transition Table Showing the Required Inputs
