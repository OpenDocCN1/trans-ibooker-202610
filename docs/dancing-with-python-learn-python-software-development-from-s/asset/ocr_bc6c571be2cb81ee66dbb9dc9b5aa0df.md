To fix terminology, we can call this a function application circuit. When we draw it like this, we call **maximum**, **round**, and “+” functions, operations, or gates.

The **maximum** and “+” gates each take two inputs and have one output. The **round** gate has one input and one output. In general, **round** is not reversible: you cannot go from the output answer it produced to its input.

Going to their lowest level, classical computer processors manipulate bits using logic operators. These are also called logic gates.

The simplest logic gate is **not**. It turns **0** into **1** and **1** into **0**. If you think of Booleans instead of bits, **not** interchanges false and true. **not** is a reversible gate. **not**(x) means “**not** applied to the Boolean x.”

# Exercise 1.13

What is the value of **not(not(x))** when x equals each of **0** and **1**?

The **and** gate takes two bits and returns **1** if they are both the same and **0** otherwise.

# Exercise 1.14

What are the values for each of the following?

**and(0, 0)**
**and(0, 1)**
**and(1, 0)**
**and(1, 1)**

The **or** gate takes two bits and returns **1** if either is **1** and **0** otherwise. The **xor** gate takes two bits and returns **1** if one and only one bit is **1** and **0** otherwise. **xor** is short for “exclusive-or.”

# Exercise 1.15

What are the values for each of the following?

**xor(0, 0)**
**xor(0, 1)**
**xor(1, 0)**
**xor(1, 1)**