
```
(¬x_i ∧ ¬y_i) ∨ (x_i ∧ y_i) = (x_i ∧ ¬x_i) ∨ (¬x_i ∧ ¬y_i) ∨ (x_i ∧ y_i) ∨ (y_i ∧ ¬y_i) = x_i ∧ (¬x_i ∨ y_i) ∨ ¬y_i ∧ (¬x_i ∨ y_i) = (x_i ∨ ¬y_i) ∧ (¬x_i ∨ y_i) = x_i ⊻ y_i
```

![c06m0002](image_fi/500099c06/c06m0002.png)

```
Sum_i(Carry_i, x_i, y_i) = Carry_i ∧ (x_i ⊻ y_i) ∨ Carry_i ∧ ¬(x_i ⊻ y_i) = Carry_i ∨ (x_i ⊻ y_i)
```

We’ll do something to develop a Boolean function for `Carry[i+1]` that will probably seem counterintuitive. Let’s start with the Karnaugh map for carry when adding three bits, [Figure 4-13](#c04.xhtml#figure4-13) from Chapter 4, but remove two of the groupings, as shown by the dotted lines in [Figure 6-4](#c06.xhtml#figure6-4).

![Figure 6-4: The Karnaugh map for carry from Figure 4-13, redrawn without two overlapping groupings (dotted lines)](image_fi/500099c06/f06004.png)

This will give us the following equation:

![c06m0003](image_fi/500099c06/c06m0003.png)
