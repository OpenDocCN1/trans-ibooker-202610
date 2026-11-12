a variable because its value or content can change or vary. When you create a variable, you are actually setting aside a small piece of memory for storage purposes. The name you give the variable is actually a label for that address in memory. For example, you declare a variable in this manner:

```
int j;
```

In this case, you have just allocated 4 bytes of memory (the amount used by integers), and you are using the variable j to refer to those 4 bytes of memory. Now whenever you reference j in your code, you are actually referencing a specific address in memory. Table 16.1 lists and describes the basic data types available in Q#.

# TABLE 16.1 Q# Data Types

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