
To further emphasize the duality of minterm and maxterm, compare (a) and (b) in [Figure 4-16](#c04.xhtml#figure4-16).

![Figure 4-16: Comparison of (a) one minterm and (b) one maxterm](image_fi/500099c04/f04016.png)

Figure 4-16: Comparison of (a) one minterm and (b) one maxterm

[Figure 4-16](#c04.xhtml#figure4-16)(a) shows the following function:

`F(x, y, z) = ¬x ∧ ¬y ∧ ¬z`

Although it’s not necessary and usually not done, we have placed a `0` in each of the cells representing minterms not included in this function.

Similarly, in [Figure 4-16](#c04.xhtml#figure4-16)(b), we have placed a `0` for the maxterm and a `1` in each of the cells representing the maxterms that are not included in the function:

`¬F(x, y, z) = x ∨ y ∨ z`

This comparison graphically shows how a minterm specifies the minimum number of `1`s in a Karnaugh map, while a maxterm specifies the maximum number of `1`s.

### Larger Groupings on a Karnaugh Map

Thus far, we have grouped only two cells together on our Karnaugh maps. Let’s look at an example of larger groups. Consider a function that outputs `1` when a three-bit number is even. [Table 4-11](#c04.xhtml#table4-11) shows the truth table. It uses `1` to indicate that the number is even and uses `0` to indicate odd.

Table 4-11: Even Values of an Eight-Bit Number

| Minterm | `x` | `y` | `z` | Number | `Even(x, y, z)` |
| --- | --- | --- | --- | --- | --- |
| `m[0]` | `0` | `0` | `0` | `0` | `1` |
| `m[1]` | `0` | `0` | `1` | `1` | `0` |
| `m[2]` | `0` | `1` | `0` | `2` | `1` |
| `m[3]` | `0` | `1` | `1` | `3` | `0` |
| `m[4]` | `1` | `0` | `0` | `4` | `1` |
| `m[5]` | `1` | `0` | `1` | `5` | `0` |
| `m[6]` | `1` | `1` | `0` | `6` | `1` |
| `m[7]` | `1` | `1` | `1` | `7` | `0` |

The canonical sum of products for this function is

`Even(x, y, z) = ∑[]^()(0,2,4,6)`

[Figure 4-17](#c04.xhtml#figure4-17) shows these minterms on a Karnaugh map with these four terms grouped together. You can group all four together because they all have adjacent edges.

From the Karnaugh map in [Figure 4-17](#c04.xhtml#figure4-17), we can write the equation for showing when a three-bit number is even:

`Even(x, y, z) = ¬z`

![Figure 4-17: Karnaugh map showing even values of a three-bit number](image_fi/500099c04/f04017.png)

Figure 4-17: Karnaugh map showing even values of a three-bit number

The Karnaugh map shows that it does not matter what the values of `x` and `y` are, only that `z` = `0`.

### Adding More Variables to a Karnaugh Map

Each time you add another variable to a Karnaugh map, you need to double the number of cells. The only requirement for the Karnaugh map to work is that you arrange the minterms, or maxterms, according to the adjacency rule. [Figure 4-18](#c04.xhtml#figure4-18) shows a four-variable Karnaugh map for minterms. The `y` and `z` variables are on the horizontal axis, and `w` and `x` are on the vertical.

![Figure 4-18: Mapping of four-variable minterms on a Karnaugh map](image_fi/500099c04/f04018.png)

Figure 4-18: Mapping of four-variable minterms on a Karnaugh map

So far we have assumed that every minterm (or maxterm) is accounted for in our functions. But design does not take place in a vacuum. We might have knowledge about other components of the overall design telling us that some combinations of variable values can never occur. Next, we’ll see how to take this knowledge into account in your function simplification process. The Karnaugh map provides an especially clear way to visualize the situation.

### Don’t Care Cells

Sometimes, you have information about the values that the variables can have. If you know which combinations of values will never occur, the minterms (or maxterms) that represent those combination are irrelevant. For example, you may want a function that indicates whether one of two possible events has occurred, but you know that the two events cannot occur simultaneously. Let’s name the events `x` and `y`, and let `0` indicate that the event has not occurred and `1` indicate that it has. [Table 4-12](#c04.xhtml#table4-12) shows the truth table for our function, `F(x, y)`.

Table 4-12: Truth Table for `x` or `y` Occurring, but Not Both

| `x` | `y` | `F(x, y)` |
| --- | --- | --- |
| `0` | `0` | `0` |
| `0` | `1` | `1` |
| `1` | `0` | `1` |
| `1` | `1` | `X` |

We can show that both events cannot occur simultaneously by placing an `X` in that row. We can draw a Karnaugh map with an `X` for the minterm that can’t exist in the system, as shown in [Figure 4-19](#c04.xhtml#figure4-19). The `X` represents a *don’t care* cell—we don’t care whether this cell is grouped with other cells or not.

![Figure 4-19: Karnaugh map for F(x, y), showing a “don’t care” cell](image_fi/500099c04/f04019.png)

Figure 4-19: Karnaugh map for `F(x, y)`, showing a “don’t care” cell

Since the cell that represents the minterm `x ∧ y` is a “don’t care” cell, we can include it, or not, in our minimization groupings, leading to the two groupings shown. The Karnaugh map in [Figure 4-19](#c04.xhtml#figure4-19) leads us to the solution:

`F(x, y) = x ∨ y`

which is a simple OR gate. You probably guessed this solution without having to use a Karnaugh map. You’ll see a more interesting use of “don’t care” cells when you learn about the design of two digital logic circuits at the end of Chapter 7.

## Combining Basic Boolean Operators

As mentioned earlier in this chapter, we can combine basic Boolean operators to implement more-complex Boolean operators. Now that you know how to work with Boolean functions, we’ll design one of the more common operators, the *exclusive or*, often called `XOR`, using the three basic operators, `AND`, `OR`, and `NOT`. It’s so commonly used that it has its own circuit symbol.

### XOR

The `XOR` is a binary operator. The result is `1` if one, and only one, of the two operands is `1`; otherwise, the result is `0`. We’ll use ⊻ to designate the `XOR` operation. It’s also common to use the ⊕ symbol. [Figure 4-20](#c04.xhtml#figure4-20) shows the `XOR` gate operation with inputs `x` and `y`.

| `x` | `y` | `x ⊻ y` |
| --- | --- | --- |
| `0` | `0` | `0` |
| `0` | `1` | `1` |
| `1` | `0` | `1` |
| `1` | `1` | `0` |

![Figure 4-20: The XOR gate acting on two variables, x and y](image_fi/500099c04/f04020.png)

Figure 4-20: The `XOR` gate acting on two variables, `x` and `y`

The minterm implementation of this operation is

`x ⊻ y = (x ∧ ¬y) ∨ (¬x ∧ y)`

The `XOR` operator can be implemented with two `AND` gates, two `NOT` gates, and one `OR` gate, as shown in [Figure 4-21](#c04.xhtml#figure4-21).

![Figure 4-21: XOR gate made from AND, OR, and NOT gates](image_fi/500099c04/f04021.png)

Figure 4-21: `XOR` gate made from `AND`, `OR`, and `NOT` gates

We can, of course, design many more Boolean operators. But we’re going to move on in the next few chapters and see how these operators can be implemented in hardware. It’s all done with simple on/off switches.

## What You’ve Learned

1.  **Boolean operators** The basic Boolean operators are `AND`, `OR`, and `NOT`.
2.  **Rules of Boolean algebra** Boolean algebra provides a mathematical way to work with the rules of logic. `AND` works like multiplication, and `OR` is similar to addition in elementary algebra.
3.  **Simplifying Boolean algebra expressions** Boolean functions specify the functionality of a computer. Simplifying these functions leads to a simpler hardware implementation.
4.  **Karnaugh maps** These provide a graphical way to simplify Boolean expressions.
5.  **Gray code** This shows how to order the cells in a Karnaugh map.
6.  **Combining basic Boolean operators** `XOR` can be created from `AND`, `OR`, and `NOT`.
