
Using this notation to write Boolean equations as a canonical sum and using the ∏ symbol to denote multiplication, we can restate the function for the complement of carry as follows:

![c04m0003](image_fi/500099c04/c04m0003.png)

If you look back at [Table 4-7](#c04.xhtml#table4-7), you’ll see that these are the conditions that cause the complement of carry to be `0` and hence carry to be `1`. This shows that using either minterms or maxterms is equivalent. The one you use can depend on factors such as what hardware components you have available to implement the function and personal preference.

## Comparison of Canonical Boolean Forms

[Table 4-8](#c04.xhtml#table4-8) shows all the minterms and maxterms for a three-variable problem. If you compare corresponding minterms and maxterms, you can see the duality of minterms and maxterms: one can be formed from the other using De Morgan’s law by complementing each variable and interchanging OR and AND.

Table 4-8: Canonical Terms for a Three-Variable Problem

| Minterm | Expression | x | y | z | Maxterm | Expression |
|---------|------------|---|---|---|---------|------------|
| `m[0]` | `¬x ∧ ¬y ∧ ¬z` | `0` | `0` | `0` | `M[0]` | `x ∨ y ∨ z` |
| `m[1]` | `¬x ∧ ¬y ∧ z` | `0` | `0` | `1` | `M[1]` | `x ∨ y ∨ ¬z` |
| `m[2]` | `¬x ∧ y ∧ ¬z` | `0` | `1` | `0` | `M[2]` | `x ∨ ¬y ∨ z` |
| `m[3]` | `¬x ∧ y ∧ z` | `0` | `1` | `1` | `M[3]` | `x ∨ ¬y ∨ ¬z` |
| `m[4]` | `x ∧ ¬y ∧ ¬z` | `1` | `0` | `0` | `M[4]` | `¬x ∨ y ∨ z` |
| `m[5]` | `x ∧ ¬y ∧ z` | `1` | `0` | `1` | `M[5]` | `¬x ∨ y ∨ ¬z` |
| `m[6]` | `x ∧ y ∧ ¬z` | `1` | `1` | `0` | `M[6]` | `¬x ∨ ¬y ∨ z` |
| `m[7]` | `x ∧ y ∧ z` | `1` | `1` | `1` | `M[7]` | `¬x ∨ ¬y ∨ ¬z` |

The canonical forms give you a complete, and unique, statement of the function because they take all possible combinations of the values of the variables into account. However, there often are simpler solutions to the problem. The remainder of this chapter is devoted to methods of simplifying Boolean functions.

## Boolean Expression Minimization

When implementing a Boolean function in hardware, each ∧ operator becomes an AND gate, each ∨ operator an OR gate, and each ¬ operator a NOT gate. In general, the complexity of the hardware is related to the number of AND and OR gates used (NOT gates are simple and tend not to contribute significantly to the complexity). Simpler hardware uses fewer components, thus saving cost and space, and uses less power. Cost, space, and power savings are especially important with handheld and wearable devices. In this section, you’ll learn how you can manipulate Boolean expressions to reduce the number of ANDs and ORs, thus simplifying their hardware implementation.

### Minimal Expressions

When simplifying a function, start with one of the canonical forms to ensure that you have taken all possible cases into account. To translate a problem into a canonical form, create a truth table that lists all possible combinations of the variables in the problem. From the truth table, it will be easy to list the minterms or maxterms that define the function.

Armed with a canonical statement, the next step is to look for a functionally equivalent *minimal expression*, an expression that does the same thing as the canonical one, but with a minimum number of literals and Boolean operators. To minimize an expression, we apply the rules of Boolean algebra to reduce the number of terms and the number of literals in each term, without changing the logical meaning of the expression.

There are two types of minimal expressions, depending on whether you use minterms or maxterms:

**Minimal Sum of Products**

1. When starting with a minterms description of the problem, the minimal expression is called a *minimal sum of products*, which is a sum of products expression where all other mathematically equivalent sum of products expressions have at least as many product terms, and those with the same number of product terms have at least as many literals.

As an example of a minimal sum of products, consider these equations:

![c04m0004](image_fi/500099c04/c04m0004.png)

`S` is in canonical form as each of the product terms explicitly shows the contribution of all three variables. The other three functions are simplifications of `S`. Although all three have the same number of product terms, `S3` is a minimal sum of products for `S` because it has fewer literals in its product terms than `S1` and `S2`.

**Minimal Product of Sums**

1. When starting with a maxterms description of the problem, the minimal expression is called a *minimal product of sums*, which is a product of sums expression where all other mathematically equivalent product of sums expressions have at least as many sum terms, and those with the same number of sum terms have at least as many literals.

For an example of a minimal product of sums, consider these equations:

![c04m0005](image_fi/500099c04/c04m0005.png)

`P` is in canonical form, and the other three functions are simplifications of `P`. Although all three have the same number of sum terms as `P`, `P3` is a minimal product of sums for `P` because it has fewer literals in its product terms than `P1` and `P2`.

A problem may have more than one minimal solution. Good hardware design typically involves finding several minimal solutions and then assessing each one within the context of the available hardware. This means more than using fewer gates. For example, as you’ll learn when we discuss the actual hardware implementations, adding judiciously placed NOT gates can actually reduce hardware complexity.

In the following two sections, you’ll see two ways to find minimal expressions.

### Minimization Using Algebraic Manipulations

To illustrate the importance of reducing the complexity of a Boolean function, let’s return to the function for carry:

```
C[out](c[in], x, y) = (¬c[in] ∧ x ∧ y) ∨ (c[in] ∧ ¬x ∧ y) ∨ (c[in] ∧ x ∧ ¬y) ∨ (c[in] ∧ x ∧ y)
```

The expression on the right-hand side of the equation is a sum of minterms. [Figure 4-4](#c04.xhtml#figure4-4) shows the circuit to implement this function. It requires four AND gates and one OR gate. The small circles at the inputs to the AND gates indicate a NOT gate at that input.

![Figure 4-4: Hardware implementation of a function to generate the value of carry when adding two numbers](image_fi/500099c04/f04004.png)

Figure 4-4: Hardware implementation of a function to generate the value of carry when adding two numbers

Now let’s try to simplify the Boolean expression implemented in [Figure 4-4](#c04.xhtml#figure4-4) to see whether we can reduce the hardware requirements. Note that there may not be a single path to a solution, and there may be more than one correct solution. I’m presenting only one way here.

First, we’ll do something that might look strange. We’ll use the idempotency rule to duplicate the fourth term twice:

```
C[out](c[in], x, y) = (¬c[in] ∧ x ∧ y) ∨ (c[in] ∧ ¬x ∧ y) ∨ (c[in] ∧ x ∧ ¬y) ∨ (c[in] ∧ x ∧ y) ∨ (c[in] ∧ x ∧ y) ∨ (c[in] ∧ x ∧ y)
```

Next, rearrange the product terms slightly to OR each of the three original terms with (c[in] ∧ x ∧ y):

```
C[out](c[in], x, y) = ((¬c[in] ∧ x ∧ y) ∨ (c[in] ∧ x ∧ y)) ∨ ((c[in] ∧ x ∧ ¬y) ∨ (c[in] ∧ x ∧ y)) ∨ ((c[in] ∧ ¬x ∧ y) ∨ (c[in] ∧ x ∧ y))
```

Now we can use the rule for distribution of AND over OR to factor out terms that OR to `1`:

![c04m0006](image_fi/500099c04/c04m0006.png)

[Figure 4-5](#c04.xhtml#figure4-5) shows the circuit for this function. Not only have we eliminated an AND gate, but also all the AND gates and the OR gate have one fewer inputs.

![Figure 4-5: Simplified hardware implementation generating carry when adding two numbers](image_fi/500099c04/f04005.png)

Figure 4-5: Simplified hardware implementation generating carry when adding two numbers
