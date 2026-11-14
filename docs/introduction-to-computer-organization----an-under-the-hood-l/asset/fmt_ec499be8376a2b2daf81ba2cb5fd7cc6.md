
The first sequential logic circuit we’ll look at is a `latch`, a one-bit storage device that can be in one of two states, depending on its input. A latch can be constructed by connecting two or more logic gates such that the output from one gate is fed back into the input of another gate; this keeps the output of both gates in the same state as long as power is applied. The state of a latch does not depend on time. (The term `latch` is also used for a multiple-bit storage device that behaves like the one-bit device described here.)

### SR Latch Using NOR Gates

The most basic latch is the `Set-Reset` (`SR`). It has two inputs (`S` and `R`) and two states, called `Set` and `Reset`. The state is used as the primary output, `Q`. It’s common to also provide the complemented output, `¬Q`. The `SR` latch is said to be in the `Set` state when the outputs `Q = 1` and `¬Q = 0`. It’s in a `Reset` state when `Q = 0` and `¬Q = 1`.

[Figure 7-1](#c07.xhtml#figure7-1) shows a simple implementation of an `SR` latch using `NOR` gates. The output of each `NOR` gate is fed into the input of the other. As we describe the behavior of the circuit in this chapter, you’ll see that this `feedback` is what keeps the latch in one state.

![Figure 7-1: NOR gate implementation of an SR latch](image_fi/500099c07/f07001.png)

There are four possible input combinations for an `SR`. Let’s go through them here:

#### `S = 0`, `R = 0`: Keep current state

If the latch is in the `Set` state (`Q = 1` and `¬Q = 0`), an input of `S = 0` and `R = 0` will cause `¬Q`, the output of the upper `NOR` gate, to yield `¬(0 ∨ 1) = 0`, and `Q`, the output of the lower `NOR` gate, to yield `¬(0 ∨ 0) = 1`. Conversely, if the latch is in a `Reset` state (`Q = 0` and `¬Q = 1`), then the output of the upper `NOR` gate yields `¬(0 ∨ 0) = 1`, and the lower `NOR` gate yields `¬(1 ∨ 0) = 0`. Thus, the cross feedback between the two `NOR` gates maintains the current state of the latch.

#### `S = 1`, `R = 0`: Set (`Q = 1`)

If the latch is in the `Reset` state, these inputs cause the output of the upper `NOR` gate to be `¬(1 ∨ 0) = 0`, thus changing `¬Q` to `0`. This is fed back to the input of the lower `NOR` gate to yield `¬(0 ∨ 0) = 1`. The feedback from the output of the lower `NOR` gate to the input of the upper keeps the output of the upper `NOR` gate at `¬(1 ∨ 1) = 0`. The latch has then moved into the `Set` state (`Q = 1` and `¬Q = 0`).

If the latch is in the `Set` state, the upper `NOR` gate yields `¬(1 ∨ 1) = 0`, and the output of the lower `NOR` gate is `¬(0 ∨ 0) = 1`. The latch thus remains in the `Set` state.

#### `S = 0`, `R = 1`: Reset (`Q = 0`)

If the latch is in the `Set` state, the lower `NOR` gate yields `¬(0 ∨ 1) = 0`, thus changing `Q` to be `0`. This is fed back to the input of the upper `NOR` gate to yield `¬(0 ∨ 0) = 1`. The feedback from the output of the upper `NOR` gate to the input of the lower keeps the output of the lower `NOR` gate at `¬(1 ∨ 1) = 0`. The latch has then moved into the `Reset` state (`Q = 0` and `¬Q = 1`).

If the latch is already in the `Reset` state, the lower `NOR` gate yields `¬(1 ∨ 1) = 0`, and the output of the upper `NOR` gate is `¬(0 ∨ 0) = 1`, so the latch remains in the `Reset` state.

#### `S = 1`, `R = 1`: Not allowed

If `Q = 0` and `¬Q = 1`, the upper `NOR` gate yields `¬(1 ∨ 0) = 0`. This is fed back to the input of the lower `NOR` gate to yield `¬(0 ∨ 1) = 0`. This would give `Q = ¬Q`, which is inconsistent with the laws of Boolean algebra.

If `Q = 1` and `¬Q = 0`, the lower `NOR` gate yields `¬(0 ∨ 1) = 0`. This is fed back to the input of the upper `NOR` gate to yield `¬(1 ∨ 0) = 0`. This would give `Q = ¬Q`, which is inconsistent.

Circuits must be designed to prevent this input combination.

To simplify things, we can represent this logic visually. [Figure 7-2](#c07.xhtml#figure7-2) introduces a graphic way to show the behavior of a `NOR` gate `SR` latch: the `state diagram`. In this `state diagram`, the current state is shown in the bubbles, with the corresponding primary output below the state. The lines with arrows show the possible transitions between the states and are labeled with the inputs that cause the transition to the next state.

![Figure 7-2: State diagram for NOR-gate SR latch](image_fi/500099c07/f07002.png)

The two circles in [Figure 7-2](#c07.xhtml#figure7-2) show the two possible states of the `SR` latch—`Set` or `Reset`. The labels on the lines show the combination of inputs, `SR`, that causes each state transition. For example, when the latch is in the `Reset` state, there are two possible inputs, `SR = 00` and `SR = 01`, that cause it to remain in that state. The input `SR = 10` causes it to transition to the `Set` state. Since the output is dependent only on the state, and not on the input, a latch is a `Moore state machine`.

Those familiar with graph theory will recognize that a `state diagram` is a directed graph: the states are the vertices, and the inputs that cause transitions are the edges. Although they are beyond the scope of this book, tools from graph theory can be useful in the design process.

As in graph theory, we can also show the same behavior in a tabular form with a `state transition table`, as in [Table 7-1](#c07.xhtml#table7-1). Here `S` and `R` are the inputs, `Q` is the output in the current state, and `Q[next]` shows the output in the state that results from the corresponding input. The `X` in the bottom two rows indicates an impossible condition.

**Table 7-1: NOR Gate SR Latch**

| **S** | **R** | **Q** | **Q[next]** |
|-------|-------|-------|-------------|
| `0`   | `0`   | `0`   | `0`         |
| `0`   | `0`   | `1`   | `1`         |
| `0`   | `1`   | `0`   | `0`         |
| `0`   | `1`   | `1`   | `0`         |
| `1`   | `0`   | `0`   | `1`         |
| `1`   | `0`   | `1`   | `1`         |
| `1`   | `1`   | `0`   | `X`         |
| `1`   | `1`   | `1`   | `X`         |

Both inputs to a `NOR gate SR latch` are normally held at `0`, which maintains the current state, giving the output `Q`. Momentarily changing only `R` to `1` causes the state to go to `Reset`, which changes the output to `Q = 0`, as shown in the `Q[next]` column of the `state transition table`. And momentarily changing only `S` to `1` causes the state to go to `Set`, giving the output `Q = 1`.

As described earlier, the input combination `S = R = 1` is not allowed because that would cause an inconsistent state for the `SR` latch. We show this in the `state transition table` by placing an `X` in the `Q[next]` column in the rows that are prohibited.

### SR Latch Using NAND Gates

The physics of their construction tends to make `NAND` gates faster than `NOR` gates. Recalling that `NAND` and `NOR` have complementary properties, it probably doesn’t surprise you that it’s possible to build an `SR` latch from `NAND` gates. Since a `NAND` gate is the logical complement of a `NOR` gate, we’ll use `¬S` and `¬R` as the inputs, as shown in [Figure 7-3](#c07.xhtml#figure7-3). To emphasize the logical duality of the two designs, `NAND` and `NOR`, I have drawn the circuit with the output `Q` at the top and `¬Q` on the bottom.

![Figure 7-3: NAND gate implementation of an SR latch](image_fi/500099c07/f07003.png)

Like the `NOR gate SR latch`, the `NAND gate SR latch` is said to be in the `Set` state when the outputs are `Q = 1` and `¬Q = 0`, and in a `Reset` state when `Q = 0` and `¬Q = 1`. There are four possible input combinations:

#### `¬S = 1`, `¬R = 1`: Keep current state

If the latch is in the `Set` state (`Q = 1` and `¬Q = 0`), the upper `NAND` gate yields `¬(1 ∧ 0) = 1`, and the lower `NAND` gate `¬(1 ∧ 1) = 0`. If `Q = 0` and `¬Q = 1`, the latch is in the `Reset` state, the upper `NAND` gate yields `¬(1 ∧ 1) = 0`, and the lower `NAND` gate `¬(0 ∧ 1) = 1`. Thus, the cross feedback between the two `NAND` gates maintains the state of the latch.

#### `¬S = 0`, `¬R = 1`: Set (`Q = 1`)
