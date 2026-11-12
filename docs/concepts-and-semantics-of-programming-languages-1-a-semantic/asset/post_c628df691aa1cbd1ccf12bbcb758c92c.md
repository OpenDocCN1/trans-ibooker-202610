

function calls that led to accel, until a dedicated handler is found. There is a handler (the try ... with construct) in the immediate calling function, f, which catches the exception and executes the prescribed treatment. If there had been no handler for NullMass in f, the exception would have propagated to the function that called f, and so on as long as no handler is found, possibly as far as the root function of the program, interrupting it as well, thus killing the execution of the global program. The handler in function f catches a possible exception, which is then bound to the variable x. If the caught exception is not the expected one, the program explicitly raises it again to ensure its detection. As we shall see, many languages automatically handle the raising of an exception if it is not one of the cases expected by the handler. We have chosen here a simplified construction of the handlers, without any loss of generality, in order to ease the presentation.

Exceptions provide a simple and flexible way of handling errors in software. If an exception is not explicitly handled by the call which raised it, it is automatically propagated upstream in the functions calls graph until a function which “knows” how to handle the exception is found. If there is no handler for this exception, then the execution of the program is interrupted, highlighting the existence of a configuration not foreseen in the algorithm. We can see that this mechanism contrasts radically with the one previously used to report errors in the traditional style, where the “impossible values” returned by functions must be encoded and propagated manually.

Exceptions can be “entities” of different kinds, depending on the language. Nevertheless, the same two functions – raising and catching exceptions – are always present.

## 8.2. A simple formalization: ML-style exceptions

In this section, we shall give a formal presentation of the syntax, the execution semantics and the exceptions typechecking for our functional language Exp2. This presentation is based on the model used by the ML family of languages. We deliberately consider only constant exceptions in order to simplify the presentation. We will briefly discuss the approaches used in certain other languages in section 8.3, highlighting the main differences between these approaches and the model presented here. These differences do not affect the fundamental principle of exceptions.

### 8.2.1. Abstract syntax

Let us extend the abstract syntax of Exp2 given in section 3.1.1. Table 8.1 presents three additional rules, added to those in Table 3.1, to define constructs relating to exceptions:

- 1) declaration of a constant exception by its name Exc;
- 2) raising an exception expression \( e \);
- 3) catching an exception, binding the identified exception to the variable \( x \).

| e ::= ... previous expressions | |
| --- | --- |
| exception Exc in e | Declaration of an exception |
| raise e | Raising an exception |
| try e1 with x -> e2 | Catching an exception |

**Table 8.1.** *Extension of the syntax of* **Exp**<sub>2</sub> *to include exceptions*

When executing a try/with construct, if the evaluation of \( e_1 \) raises an exception \( Exc \), then the result of the execution is the evaluation of \( e_2 \) in which \( x \) is bound to \( Exc \).

### 8.2.2. Values

In **Exp**<sub>2</sub>, an exception is a constant value just like an integer in \( \mathbb{Z} \), a boolean in \( \mathbb{B} \) or a closure in \( \mathbb{F} \). The fact that exceptions are values means that they can be results of evaluating arbitrary expressions, before – possibly – being raised. We must therefore differentiate between a result being an exception value and one being an explicitly raised exception. In the first case, no error is reported; the result is simply a value allowing a later error notification. In the second case, the result is an actual and characterized error.

In section 3.2.2, “error results” (denoted by **E**) were “highlighted” errors and not values. In addition to the existing values, we must introduce values for non-raised exceptions. Let **E** be the set of exception values and \(\hat{\mathbb{E}}\) the set of raised exceptions. Thus, the set of values in our language is now defined by \(\mathbf{V} = \mathbb{Z} \cup \mathbb{B} \cup \mathbb{F} \cup \mathbb{E}\). The set containing all possible results of the evaluation of an expression, that is either a value or a raised exception, is now defined as \(\mathcal{R} = \mathbf{V} \cup \hat{\mathbb{E}}\).

### 8.2.3. Type algebra

Since exceptions are values, they must have a type. We have chosen to name this type **exn**. It extends the type algebra presented in Table 5.1.

### 8.2.4. Operational semantics

Now, let us extend the rules of the big-step operational semantics using call by value presented in Box 3.3. Intuitively, evaluation results can now be represented by a sum type as follows:

```
type ’a result = Raised of exn | Value of ’a
```

To verify whether a result belongs to the “error” or “value” sets, it suffices to look at the result constructor.

When an exception is declared, its existence is recorded in the environment by binding its name to a value. For simplicity’s sake, we shall consider that this value is simply the name of the exception.

In the languages of the ML family, the exceptions are only the constructors of a particular sum type that can be extended throughout the program. For this reason, exceptions can be parameterized using arguments, just like any other sum type constructor. For example, an exception declared as

```
exception SyntaxError of (string * int * int)
```

may indicate the presence of a syntax error by specifying the name of the file, the line number and the number of the character in the line where the propagated error was detected.

```
(E_dexc)   (Exc, EC) ⊕ Env ⊢ e ~ v
          -------------------------------
          Env ⊢ exception Exc in e ~ v

(E_raise)  Env ⊢ e ~ v
          ------------------------------- (v ∈ IE)
          Env ⊢ raise e ~ v̂

(E_try)    Env ⊢ e₁ ~ v₁
          -------------------------------
          Env ⊢ try e₁ with x → e₂ ~ v₁

(E_catch)  Env ⊢ e₁ ~ v̂₁   (x, v₁) ⊕ Env ⊢ e₂ ~ v₂
          -----------------------------------------------------------
          Env ⊢ try e₁ with x → e₂ ~ v₂
```

Box 8.1. Operational semantics of exceptions

The rule E_dexc is used to declare an exception by binding its name to a new value of the same name in the environment. The rule E_raise describes the evaluation of an error raising expression. The result of this evaluation is not a value, but a raised expression belonging to ÎE. Rules E_try and E_catch are used for error catching. Rule E_try makes the hypothesis that the evaluation of the expression e_1 in the environment Env does not raise an exception, so the exception handler is not used and the returned value is the one of e_1. Rule E_catch makes the hypotheses that the evaluation of the expression e_1 in the environment Env raises an exception (v̂₁) and that this exception v₁ is caught by the handler. Thus, the handler expression e₂ is executed in Env extended by a binding of x to the intercepted exception value.

The environment in which the exception v̂₁ is raised for the first time, following the evaluation of an expression e required in order to evaluate e₁, is not recorded in this rule. As mentioned in section 8.1.4, if an exception is not explicitly handled by the call which raised it, then the exception mechanism is in charge to propagate it as a result to calls, figuring into the graph of function calls, until finding a function which handles its treatment. Note that this transmission of \(\widehat{v_1}\) is described by the sub-tree of which the conclusion is the left premise of this rule \(E_{catch}\); this sub-tree is built with the evaluation rules of function applications or operations seen in Chapter 3.

### 8.2.5. Typing

The typing rules presented in Box 5.1 can now be extended by three new rules, as shown in Box 8.2.

\((T_{catch}) \frac{(Exc, \forall \emptyset.\mathbf{exn}) \oplus \text{Env} \vdash e : \tau}{\text{Env} \vdash \mathbf{exception}\ Exc\ \mathbf{in}\ e : \tau} \qquad (T_{raise}) \frac{\alpha\ \text{fresh}}{\text{Env} \vdash \mathbf{raise}\ e : \alpha}\)

\((T_{try}) \frac{\text{Env} \vdash e_1 : \tau \quad (x, \forall \emptyset.\mathbf{exn}) \oplus \text{Env} \vdash e_2 : \tau}{\text{Env} \vdash \mathbf{try}\ e_1\ \mathbf{with}\ x \rightarrow e_2 : \tau}\)

Box 8.2. Typing rules for exceptions in \(\mathbf{Exp}_2\)

The rule \(T_{catch}\) simply extends the environment with the name of the exception (which is also its value) bound to the type exn of all the exceptions. The rule \(T_{raise}\) is used to type the raising of an exception. Since this construct raises an error instead of returning a value, it does not have, strictly speaking, a true type. The rule assigns it a polymorphic type denoted by \(\alpha\), which is a fresh type variable. This choice keeps the properties of the typing algorithm and allows an exception to be raised in any type context without type constraints. Thus, if ... then 1 else raise Exc is well-typed, as is if ... then "ok" else raise Exc. Finally, the rule \(T_{try}\) requires the type of the result of the expression to be the same, whether or not the exception handler is used. In other words, whether or not the evaluation of \(e_1\) ends in an error, the result of the expression must be of the same type.

Researchers have extended the type system of an ML language (OCaml) to include static detection of uncaught exceptions [PES 99]. The type algebra in this case is more complex, as it is annotated using exception expressions, and requires the implementation of cyclic types. To obtain more precise analytical results, the type inference requires the identifiers of recursive functions to be generalized in order to type their bodies (see section 5.3.3). A heuristic on the size of type terms allows the decidability of the inference process to be maintained.

## 8.3. Exceptions in other languages

In this section, we shall provide a brief overview of the way in which exceptions are handled in different languages. In C++, Python and Java, for example, exceptions are closely linked to the object mechanisms of the language. An exception is defined by a class inheriting from a base “exception” class. Objects in this class may be raised
