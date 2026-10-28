The 2-bit controlled-not (CNOT) gate has the following characteristics:

+   - The first bit is the *control bit*, and the second bit is the *target bit*.
+   - Based on the control bit’s value, we either apply **not** to the target bit or leave it alone. The first bit controls whether we apply **not** to the target bit.
+   - Specifically, if the control bit is 1, we apply **not** to the target bit. If the control bit is 0, we do nothing to the target bit.
+   - The gate has two output bits, the control bit and whatever we did to the target bit.

**CNOT** is the first 2-bit gate we have seen with two inputs and two outputs.

# Exercise 9.7

Write the function *controlled_not* that implements the **CNOT** gate. It should return a tuple of two bits. What is its truth table if we use the second bit of the returned tuple as the result? Have we seen this gate before?

# Exercise 9.8

What do we get if we use the output bits from *controlled_not* as the arguments to another call to *controlled_not*? That is, what is

```
controlled_not(*controlled_not(x, y))
```

for any bits x and y?

# 9.2.3 Reversible gates

A *reversible gate* **F** has two essential properties:

+   - The number of inputs to **F** is the same as the number of outputs.
+   - There is a gate **G** with the same number of inputs and outputs as **F** such that if you apply **G** to the outputs of **F**, you get the inputs to **F**.

In mathematical notation, **G = F⁻¹** and **G ∘ F = id**. We get **G** if we “run **F** backward.” **G** is the *inverse* of **F**.

# Exercise 9.9

Is **G** reversible? What is its inverse?