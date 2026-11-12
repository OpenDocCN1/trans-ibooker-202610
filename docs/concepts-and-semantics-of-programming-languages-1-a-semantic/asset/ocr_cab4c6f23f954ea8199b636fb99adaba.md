Constructs used to define functions and procedures will be introduced later. However, the following theorem characterizes the expressiveness of the language Lang3 defined at this stage.

THEOREM 4.1.— Böhm and Jacopini [BOE 66]: Any algorithm can be expressed as a composition of three control structures: sequence, conditional instruction and iteration.

Defining a semantics for the language Lang3 allows us to have a precise description of the memory modifications resulting from assignments and control instructions. Throughout this chapter, we consider that all expressions and programs over which a semantics is defined are syntactically correct and well-typed (see Chapter 5): evaluation of an expression or execution of a program never generates a typing error.

# 4.2. Evaluation of expressions

As in Chapter 2, an expression in Exp3 is evaluated in a state made up of an execution environment Env E and a memory Mem M. However, the language of expressions Exp3 contains an operator ^, which allocates memory. stores the value of its argument in this new location and returns its reference. Thus, while evaluating an expression never modifies the execution environment, it may trigger a modification of the memory state (known as a side effect of the evaluation). Evaluating an expression returns a value that may be an integer, a boolean or a reference, hence V = Z U B U R. Since we are only considering well-typed expressions (i.e. expressions for which evaluation does not result in a typing error), the only possible errors are definition errors ErrU (access to an identifier which is not defined in the environment) and execution errors ErrE (for example in the case of a division by 0). Hence, IE = {ErrU, ErrE} and V = V U IE is the set of all possible results of expression evaluation.

We thus define an evaluation function:

```
[[ ]]_- : (E x M x Exp3) → (V x M)
```

such that [[e]]Env^Mem = (v, Mem') means that v is the value obtained by evaluating the expression e in the state (Env, Mem) and Mem' is the memory issued from Mem when computing v. This function is defined in Table 4.4.

We suppose that each operator op has a semantics defining how to interpret it by an operator opV over the domain of values. We also suppose that during allocation (operator ^) an external mechanism provides locations in the memory and returns their references. For expressions of the form e1 op e2, we have chosen to evaluate