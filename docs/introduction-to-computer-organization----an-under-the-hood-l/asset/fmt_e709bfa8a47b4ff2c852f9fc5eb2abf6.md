
# Boolean Algebra Properties

## OR Distributes over AND

The OR operator applied to quantities AND-ed together can be *distributed* to apply to each of the AND-ed quantities:

```
x ∨ (y ∧ z) = (x ∨ y) ∧ (x ∨ z)
```

Because addition is not distributive over multiplication in elementary algebra, you may miss this way of manipulating Boolean expressions.

First, let's look at elementary algebra. Using addition for OR and multiplication for AND in the previous equation, we have this:

```
x + (y ⋅ z) ≠ (x + y) ⋅ (x + z)
```

We can see this by plugging in the numbers `x` = 1, `y` = 2, and `z` = 3. The left-hand side gives

```
1 + (2 ⋅ 3) = 7
```

and the right-hand side gives

```
(1 + 2) ⋅ (1 + 3) = 12
```

Thus, addition is *not* distributive over multiplication in elementary algebra.

The best way to show that OR is distributive over AND in Boolean algebra is to use a truth table, as shown in [Table 4-5](#c04.xhtml#table4-5).

**Table 4-5: OR Distributes over AND**

| `x` | `y` | `z` | `x ∨ (y ∧ z)` | `(x ∨ y) ∧ (x ∨ z)` |
| --- | --- | --- | --- | --- |
| `0` | `0` | `0` | `0` | `0` |
| `0` | `0` | `1` | `0` | `0` |
| `0` | `1` | `0` | `0` | `0` |
| `0` | `1` | `1` | `1` | `1` |
| `1` | `0` | `0` | `1` | `1` |
| `1` | `0` | `1` | `1` | `1` |
| `1` | `1` | `0` | `1` | `1` |
| `1` | `1` | `1` | `1` | `1` |

Comparing the two right-hand columns, you can see that the variable that is common to the two OR terms, `x`, can be factored out, and thus the distributive property holds.

## OR Annulment Value

An *annulment value* is a value such that operating on a quantity with the annulment value yields the annulment value. There is no annulment value for addition in elementary algebra, but in Boolean algebra, the annulment value for OR is `1`:

```
x ∨ 1 = 1
```

## Complement Value

The *complement value* is the diminished radix complement of the variable. You saw in Chapter 3 that the sum of a quantity and that quantity's diminished radix complement is equal to (*radix* – 1). Since the radix in Boolean algebra is 2, the complement of `0` is `1`, and the complement of `1` is `0`. So, the complement of a Boolean quantity is simply the NOT of that quantity, which gives

```
x ∧ ¬x = 0
x ∨ ¬x = 1
```

The complement value illustrates one of the differences between the AND and OR logical operations and the multiplication and addition arithmetic operations. In elementary algebra:

![c04m0001](image_fi/500099c04/c04m0001.png)

Even if we restrict `x` to be 0 or 1, in elementary algebra `1 ⋅ (–1) = –1`, and `1 + (–1) = 0`.

## Idempotent Property

If an operator is *idempotent*, applying it to two of the same operands results in that operand. In other words:

```
x ∧ x = x
x ∨ x = x
```

This looks different than in elementary algebra, where repeatedly multiplying a number by itself is exponentiation, and repeatedly adding a number to itself is equivalent to multiplication.

## De Morgan's Law

In Boolean algebra, the special relationship between the AND and OR operations is captured by *De Morgan's law*, which states

```
¬(x ∧ y) = ¬x ∨ ¬y
¬(x ∨ y) = ¬x ∧ ¬y
```

The first equation states that the NOT of the AND of two Boolean quantities is equal to the OR of the NOT of the two quantities. Likewise, the second equation states that the NOT of the OR of two Boolean quantities is equal to the AND of the NOT of the two quantities.

This relationship is an example of the *principle of duality*, which in Boolean algebra states that if you replace every `0` with a `1`, every `1` with a `0`, every AND with an OR, and every OR with an AND, the equation is still true. Look back over the rules just given and you'll see that all of them except involution have dual operations. De Morgan's law is one of the best examples of duality. Please, when it's Your Turn, prove De Morgan's law so you can see the principle of duality in play.

## Boolean Functions

The functionality of a computer is based on Boolean logic, which means the various operations of a computer are specified by Boolean functions. A Boolean function looks somewhat like a function in elementary algebra, but the variables can appear in either uncomplemented or complemented form. The variables and constants are connected by Boolean operators. A Boolean function evaluates to either `1` or `0` (true or false).

In "Adding in the Binary Number System" on page 44, you saw that when adding two bits, `x` and `y`, in a binary number, we have to include a possible carry into their bit position in the number. The conditions that cause carry to be `1` are

1. `x` = `1`, `y` = `1`, and there's no carry into the current bit position, or
2. `x` = `0`, `y` = `1`, and there's carry into the current bit position, or
3. `x` = `1`, `y` = `0`, and there's carry into the current bit position, or
4. `x` = `1`, `y` = `1`, and there's carry into the current bit position.

We can express this more concisely with this Boolean function:

```
C[out](c[in], x, y) = (¬c[in] ∧ x ∧ y) ∨ (c[in] ∧ ¬x ∧ y) ∨ (c[in] ∧ x ∧ ¬y) ∨ (c[in] ∧ x ∧ y)
```

where `x` is one bit, `y` is the other bit, `c[in]` is the carry in from the next-lower-order bit position, and `C[out](c[in], x, y)` is the carry resulting from the addition in the current bit position. We'll use this equation throughout this section, but first, let's think about the differences between Boolean and elementary functions.

Like an elementary algebra function, a Boolean algebra function can be manipulated mathematically, but the mathematical operations are different. Operations in elementary algebra are performed on the infinite set of real numbers, but Boolean functions work on only two possible values, `0` or `1`. Elementary algebra functions can evaluate to any real number, but Boolean functions can evaluate only to `0` or `1`.

This difference means we have to think differently about Boolean functions. For example, look at this elementary algebra function:

```
F(x, y) = x ⋅ (–y)
```

You probably read it as, "If I multiply the value of `x` by the negative of the value of `y`, I'll get the value of `F(x, y)`." However, if you look at the Boolean function

```
F(x, y) = x ∧ (¬y)
```

there are only four possibilities. If `x` = `1` and `y` = `0`, then `F(x, y)` = `1`. For the other three possibilities, `F(x, y)` = `0`. Whereas you can plug in any numbers in an elementary algebra function, a Boolean algebra function shows you what the values of the variables are that cause the function to evaluate to `1`. I think of elementary algebra functions as *asking* me to plug in values for the variables for evaluation, while Boolean algebra functions *tell* me what values of the variables cause the function to evaluate to `1`.

There are simpler ways to express the conditions for carry. And those simplifications lead to being able to implement this function in hardware with fewer logic gates, thus lowering the cost and power usage. In this and the following sections, you'll learn how the mathematical nature of Boolean algebra makes function simplification easier and more concise.

### Canonical Sum or Sum of Minterms

A *canonical form* of a Boolean function explicitly shows whether each variable in the problem is complemented or not in each term that defines the function, just as we did with our English statement of the conditions that produce a carry of `1` earlier. This ensures that you have taken all possible combinations into account in the function definition. The truth table, shown in [Table 4-6](#c04.xhtml#table4-6), for the carry equation we saw earlier

```
C[out](c[in], x, y) = (¬c[in] ∧ x ∧ y) ∨ (c[in] ∧ ¬x ∧ y) ∨ (c[in] ∧ x ∧ ¬y) ∨ (c[in] ∧ x ∧ y)
```

should help to clarify this.

**Table 4-6: Conditions That Cause Carry to Be `1`**
