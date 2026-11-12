
value that cannot be a typing error; otherwise, an error will be raised (this mechanism was first presented in Chapter 3 in relation to the ErrT error). In this case, the semantic rules must include typing rules, and must account for errors resulting from type incompatibilities. The second mechanism consists of carrying out typechecking prior to execution: this is known as static typing. A program can only be executed if its typechecking did not detect type incompatibilities. Typing error detection therefore occurs earlier than in the case of dynamic typing, and the execution is guaranteed to be free of typing errors. The execution semantics only needs to deal with well-typed programs, making it easier to handle.

### 5.1.2. How to verify types?

Once a static or dynamic typing mechanism has been selected, there are two options for verification. Some programming languages ask programmers to declare the types of their variables explicitly. In C, for example, the function to compute n!, presented in example 3.3, is written as:

```c
int fact (int n) {
  if (n <= 1) return 1 ;
  else return n * fact (n - 1) ;
}
```

where the types (int) of the parameter n and of the result are given.

If the code of this function uses a temporary variable, then the type of this variable should be declared:

```c
int fact (int n) {
  if (n <= 1) return 1 ;
  else {
    int rec_result = fact (n - 1) ;
    return n * rec_result ;
  }
}
```

The typechecker uses this explicit type information when applying typing rules to guarantee that this program is well-typed. Some other languages do not require typing annotations in the source code and the type of any expression is elaborated automatically. This mechanism is called type inference or type synthesis. It is also based on typing rules, but requires additional analysis. The types obtained thus are said to be synthesized or inferred.

The design of Exp2 is done with a type inference mechanism. Section 5.3 studies the formal type verification rules used to define the type inference algorithm described in section 5.4. A more detailed study of typing can be found in [PIE 02].

## 5.2. Informal typing of a program Exp<sub>2</sub>

### 5.2.1. A first example

Let us begin by computing, intuitively, the type of the expression [3.3] presented in section 3.1.4:

let x(1) = 25 in let f = fun x(2) ↦ x(3) + 3 in (f 7) + x(4)

- let x(1) = 25 in ...

This first let binds x(1) to the constant value 25. As this literal constant represents an integer, we deduce that x(1) must have an integer type, denoted as int.

- let f = fun x(2) ↦ x(3) + 3 in ...

The second let binds f to a functional expression. Thus, its type must be that of a function taking an argument of a certain type and returning a value of another type. Now, we have to determine these two types. For the moment, the type of the argument x(2) is unknown as no type annotation is provided for it. We introduce an unknown α (also known as a type variable) to denote this unknown type. Let us examine the body of f to deduce the type of the result. This body contains an application of the + operator, the semantics of which is the addition of two integers. This means that the result of f must be of type int; furthermore, the two subexpressions that are arguments of + must also be of type int. This is the case for 3 as it is a literal constant representing an integer. The subexpression on the left, x(3), also needs to be of type int. However, x(3) is an occurrence of the identifier x(2), bound by this function f. This function is well-typed only if x(3) and x(2) (to which we assigned the unknown type α) have the same type. Hence α = int. Finally, the type of f is that of a function for which argument is an integer and which returns an integer. This type is denoted as int → int, where the arrow represents the type of a function, the type of its parameter is on the left of the arrow and the type of the return value on the right.

- (f 7) + x(4)

This expression is an application of the + operator, which takes two arguments of the type int. On the left, the function f is clearly applied to an integer (7 is of type int) and returns an integer. On the right, x(4) is an occurrence of the identifier x(1) bound by let x(1) = 25 in ..., and is also of type int. Thus, the application of + is well-typed and returns a result of type int, which is the type of the whole expression.

In this informal description of the typechecking mechanism, we had to retrieve the types of the identifiers. Just as expressions are evaluated in an execution environment, a typing environment is required for typechecking. As a first approach, we shall consider that this environment binds identifiers to their type; this point will be discussed in more detail later.

### 5.2.2. Typing a conditional expression

Staying with our informal approach, we shall now compute the type of the expression fun x ↦ if x then true else 0. This is a function with a parameter x. As in the previous example, we have no type annotation, so the type of x is an unknown α. Analyzing the body of the function, we see that x acts as a condition, implying that it must be a boolean, denoted as bool. We must therefore have α = bool. The type of the result of this expression is the one of the conditional expression. The type of the then branch is bool, and the type of the else branch is int. Which type to choose? Let us look at the question from another angle. When applying this function to an actual value, the type of the result will depend on the value of the argument a point that prevents the further use of this result in source code. For this reason, a well-typed conditional expression must apply a test to a boolean value and return a value of the same type in both possible cases. In conclusion, the expression we are studying here is ill-typed.

### 5.2.3. Typing without type constraints

In this final part of our presentation of typechecking, we consider the following expression:

let f_(1) = fun x_(1) ↦ x_(2) in if (f_(2) true) then 8 else (f_(3) 7)

- let f_(1) = fun x_(1) ↦ x_(2) in ...

As in our first example, let binds f_(1) to a functional expression. We thus need to determine the types of its argument and of its body. Once again, as there is no type annotation for the occurrence x_(1), its type is unknown and will be denoted by α. The body of f simply returns the argument; this does not provide any further information about the type of x. Thus, there are no constraints on this type, meaning that α can be replaced by any type. Indeed, this function may be applied to any value, since it simply returns this value as a result. Hence, the type of f_(1) is α → α.

- if (f_(2) true), then 8 else (f_(3) 7)

In a conditional expression, the condition expression must be of the type bool. We must therefore ensure that (f_(2) true) is of the type bool. The type of the function f_(1) is α → α, and, as it is applied to the constant true of type bool, we must have α = bool. The type of the condition is bool since α is also the type of the result of f_(1). Now, we must ensure that the expressions found in each of the two branches of this conditional have the same type. As the then branch is of type int, the else branch must also have type int. This last branch is an application of f_(3) to the argument 7 of type int, so the result is also of type int. Both branches of the conditional are therefore of the same type, which is also the type of the full expression. However, there is a subtle consistency issue in our reasoning, to be explained in section 5.2.4.

### 5.2.4. Polymorphism

In the last example in section 5.2.3, in applying \( f_{(2)} \) to \( true \), we used \( \alpha = \text{bool} \). Then, when applying \( f_{(3)} \) to 7, we implicitly used \( \alpha = \text{int} \). By transitivity of the equality, we get \( \text{bool} = \text{int} \), which is certainly wrong. However, as \( f \) is the identity function, both uses of \( f \) appear to be licit. While there are no constraints concerning the type of \( f_{(1)} \), the type expression \( \alpha \to \alpha \) cannot serve directly to establish equality constraints between \( \alpha \) and several different types which may be incompatible. For each occurrence of \( f \), we must use a “copy” of its type expression and replace the type variable \( \alpha \) with a new type variable (known as a *fresh variable*). This prevents interferences between type constraints introduced by different occurrences of \( f \). Thus, the typing environment binds identifiers to the “form” of their types, rather than to the types themselves; this form is called a *type scheme*. This scheme may contain special type variables called *generalized* type variables. When searching for the type of an identifier in a typing environment, a “fresh” copy of the type scheme is produced, in which the generalized type variables are replaced by new variables. This copy is known as an *instance* of the scheme, and these new variables are not generalized, as they denote some yet unknown types. A type scheme, noted \( \sigma \), is thus made up of the list of generalized type variables (to be copied) and the shape of the type written with these variables. The type scheme of the function \( f \) described above is thus denoted as \( \forall \alpha. \alpha \to \alpha \).

An expression for which the type includes type variables at the end of the typechecking process is said to be *polymorphic*. As we shall see in section 5.6, at the end of the typechecking process, the types of some expressions may contain unknown variables that cannot be generalized.

## 5.3. Typing rules in Exp<sub>2</sub>

We turn now to the formal definitions of the rules of typechecking. These rules do not yet allow us to synthesize types in a program, but they make it possible to check that a program is well-typed. These rules will be used in section 5.4 to develop an inference algorithm for automatic type computation.

### 5.3.1. Types, type schemes and typing environments

First, the language used in expressions denoting types, known as the *type algebra*, must be defined. Each type \( \tau \) corresponds to a set of values. Exp2 includes integer, boolean and functional values. As we have seen, type variables are also required. Types are defined inductively using the grammar shown in Table 5.1.

In what follows, \( \alpha_1 \to \alpha_2 \to \cdots \to \alpha_{n+1} \) will be used to denote the type:

\(\alpha_1 \to (\alpha_2 \to (\cdots (\alpha_n \to \alpha_{n+1})))\)
