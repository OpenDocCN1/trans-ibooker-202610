# Simplifying the code by removing the explicit delegate instantiation

We can simplify the code by deleting the explicit instantiation of the Func<string, bool> delegate because the C# compiler can instantiate the delegate for us:

+   1. To help you learn by seeing progressively improved code, copy and paste the query
+   2. Comment out the first example, as shown in the following code:
+   3. Modify the copy to remove the explicit instantiation of the delegate, as shown in the following code:
+   4. Run the code and note that it has the same behavior.

```
// var query = names.Where(
//   new Func<string, bool>(NameLongerThanFour));
```

```
var query = names.Where(NameLongerThanFour);
```

# Targeting a lambda expression

We can simplify our code even further using a lambda expression in place of a named method.

Although it can look complicated at first, a lambda expression is simply a nameless function. It uses the => (read as "goes to") symbol to indicate the return value:

+   1. Copy and paste the query, comment the second example, and modify the query, as shown in the following code:

```
var query = names.Where(name => name.Length > 4);
```

Note that the syntax for a lambda expression includes all the important parts of the NameLongerThanFour method, but nothing more. A lambda expression only needs to define the following:

+   - The names of input parameters: name
+   - A return value expression: name.Length > 4

The type of the name input parameter is inferred from the fact that the sequence contains string values, and the return type must be a bool value as defined by the delegate for Where to work, so the expression after the => symbol must return a bool value.

The compiler does most of the work for us, so our code can be as concise as possible.

+   2. Run the code and note that it has the same behavior.