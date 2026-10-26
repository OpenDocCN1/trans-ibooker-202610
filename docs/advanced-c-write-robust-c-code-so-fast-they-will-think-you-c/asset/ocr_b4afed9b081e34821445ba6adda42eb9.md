Let's say that the call looks like this:

```
function(expression);    // deduce T and ParamType from expression
```

Type deduction proceeds differently depending on the form of ParamType used:

+   - **ParamType is a value (T)**: Pass-by-value function call
+   - **ParamType is a reference or pointer (T& or T*)**: Pass-by-reference function call
+   - **ParamType is a rvalue reference (T&&)**: Pass-by-reference function call or something else

# Case 1: ParamType is pass-by-value (T)

```
template<typename T>
void function(T parameter);
```

As a pass-by-value call, this means that the parameter will be a copy of whatever is passed in. Because this is a new instance of the object, the following rules are applied to the expression:

+   - If the expression's type is a reference, then ignore the reference part.
+   - If, after step 1, the remaining type is const and/or volatile, then ignore those as well.

What's left is T. Let's attempt to compile the following file code:

```
template<typename T>
class TypeDisplay;

template<typename T>
void function(T parameter)
{
    TypeDisplay<T> type;
}

void types()
{
    int x = 42;
    function(x);
}
```