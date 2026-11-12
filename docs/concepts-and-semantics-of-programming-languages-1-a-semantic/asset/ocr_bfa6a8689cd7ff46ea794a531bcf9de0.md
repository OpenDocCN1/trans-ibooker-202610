# Chapter 3. Semantics of Functional Features . . . . . . . . . . . . . . . . . . 35

+   3.1. Syntactic aspects . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 35

+   3.1.1. Syntax of a functional kernel . . . . . . . . . . . . . . . . . . . . 35

+   3.1.2. Abstract syntax tree . . . . . . . . . . . . . . . . . . . . . . . . . 36

+   3.1.3. Reasoning by induction over expressions . . . . . . . . . . . . . . . 39

+   3.1.4. Declaration of variables, bound and free variables . . . . . . . . . . 39

+   3.2. Execution semantics: evaluation functions . . . . . . . . . . . . . . . 42

+   3.2.1. Evaluation errors . . . . . . . . . . . . . . . . . . . . . . . . . . 42

+   3.2.2. Values . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 43

+   3.2.3. Interpretation of operators . . . . . . . . . . . . . . . . . . . . . 45

+   3.2.4. Closures . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 46

+   3.2.5. Evaluation of expressions . . . . . . . . . . . . . . . . . . . . . . 47

+   3.3. Execution semantics: operational semantics . . . . . . . . . . . . . . 54

+   3.3.1. Simple expressions . . . . . . . . . . . . . . . . . . . . . . . . . . 55

+   3.3.2. Call-by-value . . . . . . . . . . . . . . . . . . . . . . . . . . . . 56

+   3.3.3. Recursive and mutually recursive functions . . . . . . . . . . . . . . 60

+   3.3.4. Call-by-name . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 61

+   3.3.5. Call-by-value versus call-by-name . . . . . . . . . . . . . . . . . . 62

+   3.4. Evaluation functions versus evaluation relations . . . . . . . . . . . . 64

+   3.4.1. Status of the evaluation function . . . . . . . . . . . . . . . . . . 64

+   3.4.2. Induction over evaluation trees . . . . . . . . . . . . . . . . . . . 65

+   3.5. Semantic properties . . . . . . . . . . . . . . . . . . . . . . . . . . . 69

+   3.5.1. Equivalent expressions . . . . . . . . . . . . . . . . . . . . . . . . 69

+   3.5.2. Equivalent environments . . . . . . . . . . . . . . . . . . . . . . . 71

+   3.6. Exercises . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 71

# Chapter 4. Semantics of Imperative Features . . . . . . . . . . . . . . . . . 77

+   4.1. Syntax of a kernel of an imperative language . . . . . . . . . . . . . . 77

+   4.2. Evaluation of expressions . . . . . . . . . . . . . . . . . . . . . . . . 81

+   4.3. Evaluation of definitions . . . . . . . . . . . . . . . . . . . . . . . . 86

+   4.4. Operational semantics . . . . . . . . . . . . . . . . . . . . . . . . . . 89

+   4.4.1. Big-step semantics . . . . . . . . . . . . . . . . . . . . . . . . . . 89

+   4.4.2. Small-step semantics . . . . . . . . . . . . . . . . . . . . . . . . . 93

+   4.4.3. Expressiveness of operational semantics . . . . . . . . . . . . . . . 95

+   4.5. Semantic properties . . . . . . . . . . . . . . . . . . . . . . . . . . . 96

+   4.5.1. Equivalent programs . . . . . . . . . . . . . . . . . . . . . . . . . 96

+   4.5.2. Program termination . . . . . . . . . . . . . . . . . . . . . . . . . 98

+   4.5.3. Determinism of program execution . . . . . . . . . . . . . . . . . . 100

+   4.5.4. Big steps versus small steps . . . . . . . . . . . . . . . . . . . . 103

+   4.6. Procedures . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 109

+   4.6.1. Blocks . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 109

+   4.6.2. Procedures . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 112

+   4.7. Other approaches . . . . . . . . . . . . . . . . . . . . . . . . . . . . 118