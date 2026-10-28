
```python
print("-----+------")
for bit in (0, 1):
    print(f"{bit}  | {op(bit, 0)}      {op(bit, 1)}")
```

The truth table for **and** is:

```python
print_truth_table(lambda x, y: x and y, "and")
```

| and | 0 | 1 |
| --- | --- | --- |
| 0 | 0 | 0 |
| 1 | 0 | 1 |

This table says, for example, that for inputs of 0 and 0, the output of 0 and 0 is 0. Electrical engineers use a standardized set of symbols to represent gates. Figure 9.3 shows the “American” symbols for **and** gates with their two inputs and single outputs:

![](img/4ffeafa3e3f1373f02971e43b0755ea6_364_0.png)

Figure 9.3: 2-bit logical “and” gate

The table for **or** is:

```python
print_truth_table(lambda x, y: x or y, "or")
```

| or | 0 | 1 |
| --- | --- | --- |
| 0 | 0 | 1 |
| 1 | 1 | 1 |

![](img/4ffeafa3e3f1373f02971e43b0755ea6_364_1.png)

Figure 9.4: 2-bit logical “or” gate

We frequently write exclusive-or “^” as **xor**.

```python
print_truth_table(lambda x, y: x ^ y, "xor")
```

| xor | 0 | 1 |
| --- | --- | --- |
| 0 | 0 | 1 |
| 1 | 1 | 0 |

![](img/4ffeafa3e3f1373f02971e43b0755ea6_365_0.png)

Figure 9.5: 2-bit logical "xor" gate

In addition to these three primary 2-bit gates, we define secondary gates by applying not to them. `nand` is `not and`:

```python
print_truth_table(lambda x, y: bit_not(x and y), "nand")
```

| nand | 0 | 1 |
| --- | --- | --- |
| 0 | 1 | 1 |
| 1 | 1 | 0 |

![](img/4ffeafa3e3f1373f02971e43b0755ea6_365_1.png)

Figure 9.6: 2-bit logical "nand" gate

### Exercise 9.3

Verify that the function

```python
def bit_nand(bit_0, bit_1):
    return bit_not(bit_0 and bit_1)
```

has the same truth table as the one above.

`nor` is `not or`:

```python
print_truth_table(lambda x, y: bit_not(x or y), "nor")
```

| nor | 0 | 1 |
| --- | --- | --- |
| 0 | 1 | 0 |
| 1 | 0 | 0 |

![](img/4ffeafa3e3f1373f02971e43b0755ea6_365_2.png)

Figure 9.7: 2-bit logical "nor" gate

### xnor is not xor:

```python
print_truth_table(lambda x, y: bit_not(x ^ y), "xnor")
```

| xnor | 0 | 1 |
| --- | --- | --- |
| 0 | 1 | 0 |
| 1 | 0 | 1 |

![](img/4ffeafa3e3f1373f02971e43b0755ea6_366_0.png)

Figure 9.8: 2-bit logical “xnor” gate

### Exercise 9.4

Define functions `bit_nor` and `bit_xnor` and validate their results.

### Exercise 9.5

Define the function

```python
def mystery_gate(bit_0, bit_1):
    x = bit_nand(bit_0, bit_1)
    y = bit_nand(bit_0, x)
    z = bit_nand(bit_1, x)
    return bit_nand(y, z)
```

Display its truth table and determine which 2-bit gate this is. Note that we built it from four nand gates, which means that the `mystery_gate` is superfluous.

### Exercise 9.6

Define the function

```python
def another_mystery_gate(bit_0, bit_1):
    return abs(bit_0 - bit_1)
```

Display its truth table and determine which 2-bit gate this is.

The 2-bit controlled-not (CNOT) gate has the following characteristics:

- The first bit is the *control bit*, and the second bit is the *target bit*.
- Based on the control bit's value, we either apply **not** to the target bit or leave it alone. The first bit controls whether we apply **not** to the target bit.
- Specifically, if the control bit is 1, we apply **not** to the target bit. If the control bit is 0, we do nothing to the target bit.
- The gate has two output bits, the control bit and whatever we did to the target bit.

**CNOT** is the first 2-bit gate we have seen with two inputs and two outputs.

### Exercise 9.7

Write the function `controlled_not` that implements the **CNOT** gate. It should return a tuple of two bits. What is its truth table if we use the second bit of the returned tuple as the result? Have we seen this gate before?

### Exercise 9.8

What do we get if we use the output bits from `controlled_not` as the arguments to another call to `controlled_not`? That is, what is

```python
controlled_not(*controlled_not(x, y))
```

for any bits x and y?

## 9.2.3 Reversible gates

A *reversible gate* **F** has two essential properties:

- The number of inputs to **F** is the same as the number of outputs.
- There is a gate **G** with the same number of inputs and outputs as **F** such that if you apply **G** to the outputs of **F**, you get the inputs to **F**.

In mathematical notation, **G = F⁻¹** and **G ∘ F = id**. We get **G** if we “run **F** backward.” **G** is the *inverse* of **F**.

### Exercise 9.9

Is **G** reversible? What is its inverse?

For the 1-bit gates, **id** and **not** are reversible and are their own inverses. **0** and **1** are not reversible.

None of the single output 2-bit gates are reversible. **CNOT** is reversible and is its own inverse.

To better show reversible gates, I now introduce a new way of drawing them in diagrams.

![](img/4ffeafa3e3f1373f02971e43b0755ea6_368_0.png)

When the input to the **not** gate is 0, the result is 1. When the input is 1, the result is 0.

The **CNOT** gate has a control bit, shown with a black dot, and a target bit, shown with a plus sign in a circle. We connect them with a line.

![](img/4ffeafa3e3f1373f02971e43b0755ea6_368_1.png)

![](img/4ffeafa3e3f1373f02971e43b0755ea6_368_2.png)

The example illustrates how a 0 or 1 in the control bit affects a 1 in the target bit. We can implement **CNOT** in Python with the function

```python
def bit_cnot(bit_0, bit_1):
    return (bit_0, bit_not(bit_1) if bit_0 else bit_1)
```

The **SWAP** gate interchanges the values of the bits. We draw the gate with two Xs connected by a line. For example,

![](img/4ffeafa3e3f1373f02971e43b0755ea6_368_3.png)

![](img/4ffeafa3e3f1373f02971e43b0755ea6_368_4.png)

In the second example, the bit values are the same, and the swap is not apparent.

### Exercise 9.10

Implement the function `bit_swap` in Python using a simultaneous assignment.

## 9.2.4 Gates on more than two bits

We can extend many of the 2-bit gates to three or more inputs. The 3-bit **and** returns 1 if all its inputs are 1, and 0 otherwise. For example,

![](img/4ffeafa3e3f1373f02971e43b0755ea6_369_0.png)

Though these are useful in electrical engineering, they are less so in coding.

The Toffoli or **CCNOT** gate is a reversible 3-bit gate. The first two bits are control bits, and the third is the target bit. If both the control bits are 1, we apply **not** to the target. Otherwise, we leave the target bit alone. We draw it as shown on the right.

![](img/4ffeafa3e3f1373f02971e43b0755ea6_369_1.png)

### Exercise 9.11

Show that **CCX** is reversible.

![](img/4ffeafa3e3f1373f02971e43b0755ea6_369_2.png)

The Fredkin or **CSWAP** gate is another reversible 3-bit gate. The first bit is the control bit, and its value determines whether we swap the values of the second and third bits. If the control bit is 1, we do the swap. Otherwise, we leave bits 2 and 3 alone. We draw it as shown on the left.

### Exercise 9.12

Show that **CSWAP** is reversible.

If `a` has `n` bits and `b` has `m` bits, the sum will have at most `maximum(n, m) + 1` bits, where the extra bit is for the carry. The carry bit might be 0. Expanding our diagram for `a` and `b` each having 2 bits, we get

![](img/4ffeafa3e3f1373f02971e43b0755ea6_371_0.png)

`a0` and `b0` are the right-most, low-order bits. For example, if `a` is 01, then `a0` is 1. Our task is to understand what is inside that big box with the “+”. Can we break it down into calls to the 1-bit adder? Do we need to modify the 1-bit adder so it can be a component in larger circuits?

Our plan for 2-bit addition is this:

1. Add `a0` and `b0` to get `s0` and a temporary carry bit `c0`.
2. Add `a1`, `b1`, and `c0` to get `s1` and carry bit `c`.

We add single bits in each step, but there is a problem in step 2: we need to add three single bits, not two. We modify the 1-bit adder to have three inputs:

![](img/4ffeafa3e3f1373f02971e43b0755ea6_371_1.png)

Now we can create our addition circuit for 2-bit numbers:

![](img/4ffeafa3e3f1373f02971e43b0755ea6_371_2.png)

I labeled the carry bits.

### Exercise 9.19

Which of these relationships hold for xor, nand, nor, xnor, and their combinations?

These rules are useful in Python conditionals. For example, the statement

```python
if x != y and z != w:
```

is the same as

```python
if not (x == y or z == w):
```

### Exercise 9.20

Replace `x < y` with an expression involving `not`. Do the same for `<=`, `>`, and `>=`.

## 9.5 Universality for bit gates

Do we need all these gates? Is there some subset of the 1- and 2-bit gates that can generate all the others? Let’s begin with a nand gate and see what we can build from there.

The first new construction is that `not x = x nand x`. If we feed `x` to both inputs of nand, we get the same result as not!

![](img/4ffeafa3e3f1373f02971e43b0755ea6_374_0.png)

![](img/4ffeafa3e3f1373f02971e43b0755ea6_374_1.png)

The black dot “●” reuses the bit’s value so we can use it as two inputs.

This circuit shows that if we have nand, we don’t need not as well. Though we may use not in circuits, it is superfluous.

The original set of 1- and 2-bit gates was

`not`, `and`, `or`, `xor`, `nand`, `nor`, `xnor`.

Since `x and y = not (x nand y)`, the gates we still need are

`or`, `xor`, `nand`, `nor`, `xnor`.
