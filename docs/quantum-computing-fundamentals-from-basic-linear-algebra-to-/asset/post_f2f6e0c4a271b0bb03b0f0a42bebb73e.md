
This function simply calls the function IsConstant, passing it another function, BooleanFunction, along with the parameters to BooleanFunction (iQubits, mElements) and the remaining parameter to IsConstant.

Next, let us look at the function IsConstant. This will return if the value is constant or not (i.e., not balanced). The function is shown in Figure 16.18.

![](img/49061be7fe11d9f7f3e56114ab15acdc_327_0.png)

FIGURE 16.18 The IsConstant function

Much of this you have seen earlier in this chapter. We have the two gates, X and H. We are also using the ApplyToEach operation. Note that this function takes in another function, Uf. In this case, that function is one we name BooleanFunction. That requires the two parameters for BooleanFunction, and those are shown as well.

There are two other functions in our code: BooleanFunction and a function named UnitaryOperation. Those are both shown in Figure 16.19.

![](img/49061be7fe11d9f7f3e56114ab15acdc_327_1.png)

FIGURE 16.19 Remaining functions for Deutsch-Jozsa

As you can see, the `BooleanFunction`, in turn, calls the `UnitaryOperation` function. If you carefully studied the previous code samples and reviewed the Deutsch-Jozsa algorithm, this code example should help you to better understand both the algorithm and Q# programming.

## 16.5 Bit Flipping

Let us try one more example. This code simply demonstrates bit flipping. This example is quite similar to one found on Microsoft's website at https://docs.microsoft.com/en-us/quantum/tutorials/explore-entanglement. The code on the Microsoft site goes further and simulates entanglement. You should consider working through that example as well.

First, let's look at the code. In this case, all the code is shown in Figure 16.20.

![](img/49061be7fe11d9f7f3e56114ab15acdc_328_0.png)

FIGURE 16.20 Entanglement

First notice that this code depends a great deal on gates and measurements. You see the X operation and the M operation that were discussed previously in this chapter. Recall that X flips the state, and M measures it. The entry point is the operation `TestState`, which tests the state of some qubit.

The function simply takes in a number of iterations to go through and an initial value. Then it will go through flipping bits and outputting the result. This is a rather simple program, but it will help you get more comfortable with Q#.

## 16.6 Summary

In this chapter, you finally applied the knowledge you have learned to actual programming tasks. If you worked through the projects in this chapter, you should now have a basic working knowledge of Q#. Furthermore, actually programming some of these algorithms should aid you in understanding those algorithms better. There are also numerous sources on the Internet you can consult:

- Microsoft’s Quantum Computing Foundations: https://docs.microsoft.com/en-us/learn/paths/quantum-computing-fundamentals/
- Microsoft Quantum Katas: https://docs.microsoft.com/en-us/quantum/tutorials/intro-to-katas
- Microsoft Q# User Guide: https://docs.microsoft.com/en-us/quantum/user-guide/

## Test Your Skills

### REVIEW QUESTIONS

- 1. In Q#, what data type represents the result of a projective measurement onto the eigenspaces of a quantum operator with eigenvalues ±1?
  - a. EigenReturn
  - b. Return
  - c. Result
  - d. EigenResult

- 2. A(n) ______ is a loop-like statement that during each iteration assigns the declared loop variables to the next item in a sequence (a value of Array or Range type) and executes a specified block of statements.
  - a. iteration
  - b. repeat
  - c. for-next
  - d. while

- 3. ______ is the act of taking the data, and the functions that work on that data, and putting them together in a single class.
  - a. Abstraction
  - b. Encapsulation
  - c. Inheritance
  - d. Polymorphism

- 4. What namespace are Pauli gates found in?
  - a. Microsoft.Quantum.Intrinsic
  - b. Microsoft.Quantum.Canon
  - c. Microsoft.Quantum.Gates
  - d. Microsoft.Quantum.Measurement

- 5. What does the Quantum Development Kit's R operation do?
  - a. Rotate about the x-, y-, or z-axis.
  - b. Reset a given qubit.
  - c. Reset a given gate.
  - d. Rotate about a given Pauli axis.

This page intentionally left blank

## Working with QASM

### Chapter Objectives

After reading this chapter and completing the review questions, you will be able to do the following:

- Understand the fundamentals of QASM
- Write QASM programs
- Create quantum simulations with QASM

Quantum Assembly Language (QASM) is a programming language specifically for quantum programs. It is used to simulate quantum gates and quantum algorithms. You use an online editor (https://www.quantum-inspire.com/projects/6923062) to write QASM code. That editor is shown in Figure 17.1.

![](img/49061be7fe11d9f7f3e56114ab15acdc_332_0.png)

FIGURE 17.1 Quantum Inspire Editor

This programming language is meant to allow you to experiment with various quantum gates and circuits. It is an ideal tool to translate your theoretical knowledge of quantum computing into practice. In fact, it is probably easier to set up quantum simulations in QASM than in Q# (see Chapter 16, “Working with Q#”).

If you have no experience in programming, do not be overly concerned. We will approach a basic QASM program assuming you have no experience programming at all. There is also a section titled “Basic Programming Concepts,” which is where we’ll start.

### 17.1 Basic Programming Concepts

QASM does not have many of the structures found in other programming languages. This section introduces the programming concepts associated with QASM.

### 17.1.1 Instructions

QASM is based on instructions, and the various instructions are related specifically to quantum operations and structures. The fact that so much of what you need for quantum computing is built into the QASM language makes it an excellent choice for learning quantum computing. For example, the command `qubits n` initializes a qubit register of size n. By default, all qubits are initialized in the |0⟩ state.

There are three `prep` statements. With the `prep` instruction, qubits will be explicitly initialized in the |0⟩ state of a specific basis:

- `prep_z`
- `prep_y`
- `prep_x`

The Pauli gates are also prearranged for you in QASM. There are three:

- Pauli-X
- Pauli-Y
- Pauli-Z

Several other gates and measurements are available to you. Table 17.1 summarizes the most important ones.

TABLE 17.1 QASM Instructions

| Instruction | Meaning |
| --- | --- |
| Pauli-X | To use the Pauli-X gate on the first qubit, you use this syntax:
`X q[0]` |
| Pauli-Y | The Y gate is similar:
`Y q[0]` |
| Pauli-Z | And here's the Z gate:
`Z q[0]` |
| Hadamard | To execute the Hadamard gate against the first qubit is much like the Pauli gates:
`H q[0]`
Here's how to execute the Hadamard (or any other gate) against the first two qubits:
`H q[0, 1]` |
| Identity | `I` is the identity matrix/gate. |
| Rx | This is the rotation operator; in this case, rotation about the x-axis. Then use this on the first qubit:
`Rx q[0]` |
| Ry | This is the rotation operator; in this case, rotation about the y-axis. Then use this on the first qubit:
`Ry q[0]` |
| Rz | This is the rotation operator; in this case, rotation about the z-axis. Then use this on the first qubit:
`Rz q[0]` |
| T gate | Here's how to execute the T gate on the first qubit:
`T q[0]` |
| CNOT gate | The CNOT gate is a two-qubit operation, where the first qubit is usually referred to as the control qubit and the second qubit as the target qubit. This can be used to entangle two qubits, as follows:
`CNOT q[0], q[1]` |
| SWAP gate | The SWAP gate is a two-qubit operation. Expressed in basis states, the SWAP gate swaps the state of the two qubits involved in the operation:
`SWAP q[0], q[1]` |
| Toffoli gate | Recall that this gate is sometimes called the Controlled-Controlled NOT gate. Here's an example:
`Toffoli q[1], q[2], q[3]` |
| measure_x | All measurements measure in some basis. The `measure_x` command will measure the qubit in the x-basis. |
| measure_y | All measurements measure in some basis. The `measure_y` command will measure the qubit in the y-basis. |
| measure_z | All measurements measure in some basis. The `measure_z` command will measure the qubit in the z-basis. |
| measure_all | This command will measure all the qubits in parallel using the z-basis. |
