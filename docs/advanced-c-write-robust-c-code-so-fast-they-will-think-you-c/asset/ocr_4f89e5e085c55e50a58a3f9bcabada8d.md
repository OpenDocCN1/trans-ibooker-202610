```
PrintVector("After transform", values);\n\nstd::cout << "Complete.\\n";\nreturn 0;\n}
```

When this program runs the output shows that the vector has been scaled by 5:

![](bbox=[0.325, 0.254, 0.704, 0.338])

Figure 4.24: Transform using lambda for scaling

The lambda in this program is `[] (int n) {return 5*n;}` and has an empty capture clause `[]`. The empty capture clause means that the lambda function does not access any variables from the surrounding scope. If there are no parameters passed to the lambda, then the parameters clause `()` is optional.

# Capturing data into Lambdas

The **capture clause**, or the **lambda introducer** (from the C++ specification), allows the anonymous function to capture data from the surrounding scope for later use. This combination of a function with a scope that resolves the functions variable is known as a **closure**. The scope in this case is a set of variable bindings specified in the capture clause. In the previous section, we said that lambdas are anonymous functions. With the addition of variable capture through closures, lambdas are more correctly identified as anonymous function objects. The compiler creates an anonymous class with an inline constructor to capture the variables and a function call operator **operator()**.

A capture clause is a comma-separated list of zero or more captured variables. There is also the concept of default captures – either by reference or by value. So, the basic syntax for a capture is:

+   - `[&]` – captures all automatic storage duration variables in scope by reference
+   - `[=]` – captures all automatic storage duration variables in scope by value (make a copy)
+   - `[&x, y]` – captures x by reference and y by value