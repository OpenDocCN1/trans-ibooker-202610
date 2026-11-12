
### 15.4 Summary

In this chapter, we have examined different types of algorithms that have been proposed for quantum-resistant solutions. We have examined hash functions, code-based cryptography, and supersingular isogeny key exchange. Some of the math in this chapter might have been a bit rigorous for some readers. Keep in mind that unless you intend to be working in the field of quantum-resistant cryptography, it is not critical that you have every nuance of every algorithm clear in your mind. As long as you have a general understanding, that will be sufficient.

### Test Your Skills

### REVIEW QUESTIONS

- 1. At the core of many cryptographic hash functions is a(n) ________.
- a. Merkle-Damgaard construction
- b. pseudo-random-number generator
- c. isogeny
- d. error correction code

- 2. Which of the following algorithms has a private key of (SPA)?
- a. SWIFFT
- b. SIDH
- c. Lamport
- d. McEliece

- 3. The ________ cryptosystem originally used parameters n = 1024, k = 524, and t = 50.
- a. SWIFFT
- b. SIDH
- c. Lamport
- d. McEliece

- 4. The ________ cryptosystem usually uses the binary Goppa code.
- a. Niederreiter
- b. McEliece
- c. Lamport
- d. SWIFFT

- 5. A finite field, also called a ______, is a field with a finite number of elements.
- a. torsion group
- b. Galois field
- c. abelian group
- d. torsion field

- 6. When curves are _____, they will have the same j-invariant over a closed field.
- a. isomorphic
- b. isometric
- c. surjective
- d. bijective

This page intentionally left blank

## Working with Q#

### Chapter Objectives

After reading this chapter and completing the review questions, you will be able to do the following:

- Understand the fundamentals of Q#
- Write Q# programs
- Create quantum simulations with Q#

Microsoft developed Q# as a relatively easy-to-use quantum programming language. Microsoft first announced Q# in September 2018, and by December 2018 it was released. Q# is based on C# for syntax structure but includes the ability to create quantum logic gates as well as to simulate entanglement.

If you have no experience in programming, do not be overly concerned. In the section “Getting Started with Q#,” we will approach a basic Q# program assuming you have no experience programming at all. There is also a section titled “Basic Programming Concepts,” which is where we’ll begin.

### 16.1 Basic Programming Concepts

This section is for those readers who have very limited programming experience. If you have experience in programming, particularly in C#, then you may wish to skim this section to at least get some of the Q#-specific items.

### 16.1.1 Variables and Statements

In general, all programs handle data of some sort. Regardless of the purpose of the program, it must process data. Data must be temporarily stored in the program. This is accomplished via variables. A variable is simply a place in memory set aside to hold data of a particular type. This is called a variable because its value or content can change or vary. When you create a variable, you are actually setting aside a small piece of memory for storage purposes. The name you give the variable is actually a label for that address in memory. For example, you declare a variable in this manner:

```
int j;
```

In this case, you have just allocated 4 bytes of memory (the amount used by integers), and you are using the variable j to refer to those 4 bytes of memory. Now whenever you reference j in your code, you are actually referencing a specific address in memory. Table 16.1 lists and describes the basic data types available in Q#.

### TABLE 16.1 Q# Data Types

| Data Type | Description |
| :--- | :--- |
| Unit | Represents a singleton type whose only value is ( ). |
| Int | Represents a 64-bit (4 byte) signed integer. Values range from –9,223,372,036,854,775,808 to 9,223,372,036,854,775,807. |
| BigInt | Represents signed integer values of any size. |
| Double | Represents a double-precision 64-bit floating point number. Values range from –1.79769313486232e308 to 1.79769313486232e308. |
| Bool | Represents Boolean values (true/false). |
| String | Represents text as values that consist of a sequence of UTF-16 (2 byte) code units. |
| Qubit | Represents a qubit. Values of type Qubit are instantiated via allocation. |
| Result | Represents the result of a projective measurement onto the eigenspaces of a quantum operator with eigenvalues ±1. Possible values are 0 and 1. |
| Pauli | Represents a single-qubit Pauli matrix. Possible values are PauliI, PauliX, PauliY, and PauliZ. |
| Range | Represents an ordered sequence of equally spaced Int values, in ascending or descending order. |
| Array | Represents values that each contain a sequence of values of the same type. |
| Tuple | Represents values that each contain a fixed number of items of different types. Tuples containing a single element are equivalent to the element they contain. |

Once you have variables, the next building block is a statement. A statement is simply a single line of code that performs some action. That action might be to declare a variable, add two numbers, compare two values, or just about anything at all. All statements/expressions end with a semicolon. This is true in many programming languages, including Java, C, and C#. A few example statements will help clarify this for you:

```
int acctnum;
acctnum = 55555;
acctnum = acctnum + 5;
```

Each statement performs a different action, but it does perform some action, and it ends with a semi-colon. In many programming languages, the terms _statement_ and _expression_ are used interchangeably. In Q#, an expression is a special type of statement. Q# has some very specific statements. Table 16.2 shows the possible statement types.

### TABLE 16.2 Q# Statement Types

| Statement Type | Description |
|---|---|
| Variable declaration | Defines one or more local variables that will be valid for the remainder of the current scope, and it binds them to the specified values. There are also variable reassignment statements that change a variable's value. |
| Expression statement | An expression statement consists of an operation or function call returning a Unit. The invoked callable needs to satisfy the requirements imposed by the current context. |
| Return statement | A return statement terminates the execution within the current callable context and returns control to the caller. |
| Fail statement | A fail statement aborts the execution of the entire program, collecting information about the current program state before terminating in an error. |
| Iteration | An iteration is a loop-like statement that during each iteration assigns the declared loop variables to the next item in a sequence (a value of Array or Range type) and executes a specified block of statements. |
| Repeat statement | Quantum-specific loop that breaks based on a condition. The statement consists of an initial block of statements that is executed before a specified condition is evaluated. If the condition evaluates to false, an optional subsequent fixup-block is executed before entering the next iteration of the loop. |
| Conjugation | A conjugation is a special quantum-specific statement, where a block of statements that applies a unitary transformation to the quantum state is executed, followed by another statement block, before the transformation applied by the first block is reverted again. In mathematical notation, conjugations describe transformations of the form U†VU to the quantum state. |
| Qubit allocation | Instantiates and initializes qubits and/or arrays of qubits and then binds them to the declared variables. Executes a block of statements. The instantiated qubits are available for the duration of the block and will be automatically released when the statement terminates. |

There is another special type in Q# called _immutable_. As the name suggests, it cannot change. It is like a constant in other programming languages. If you use the `let` statement, then you are making the type an immutable. Therefore, if you say

```
let myvar = 3
```

then `myvar` has the value 3 and cannot be changed.

Now that you have seen variable declaration and statements, it is time to examine the basic structure of a Q# program. All programs, regardless of the programming language, have components. In Q#, as
