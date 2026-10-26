The definitions of each are as follows:

+   - An expression that determines the identity of an object is a **glvalue**.
+   - An expression whose evaluation initializes an object or the operand of an operator is a **prvalue**. Examples include a literal (except string literal) such as 3.1415, true or nullptr, the this pointer, post increment, and post decrement expressions.
+   - A glvalue object that has resources and can be reused (because its life is ending) is an **xvalue**. Examples include a function call whose return type is an rvalue reference to an object, such as `std::move()`.
+   - A glvalue that is not an xvalue is an **lvalue**. Examples include the name of a variable, a function or a data member, or a string literal.
+   - A prvalue or xvalue is an **rvalue**.

It does not matter if you do not fully understand these for the explanation that follows – just know that an expression that is considered to be an lvalue can have its address taken (using the address of operator, that is, "&"). The type deduction rules for the following require that you know what an lvalue is, as well as what it isn't:

```
template<typename T>

void function(T&& parameter)
{
    TypeDisplay<T> type;
}
```

The type deduction rules for this ParamType form are as follows:

+   - If the expression is an lvalue reference, then both T and ParamType are deduced to be an lvalue reference. This is the only scenario where the type is deduced to be a reference.
+   - If the expression is an rvalue reference, then the rules for Case 2 apply.