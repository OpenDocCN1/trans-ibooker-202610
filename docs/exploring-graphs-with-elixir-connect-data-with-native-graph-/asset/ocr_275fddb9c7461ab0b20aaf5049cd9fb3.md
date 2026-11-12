First, since statements don’t return anything, they have to cause side effects and mutate memory to fulfill their purpose. Expressions, on the other hand, can be designed to favor referential transparency, giving us the benefits we discussed previously.

The other benefit is that, unlike statements, expressions can be composed. This can help us use a powerful pattern in the functional style of programming—function chaining. We can create a chain of functions so the results of computations flow smoothly from one function into the next. The code begins to read like the problem statement, making it easier to follow.

We saw a benefit of this in ...To the Functional Style, where we sent a list of stock-ticker symbols through a chain of functions to determine the highest-priced stock and its price. This pattern can also help us create fluent interfaces, as we saw in Creating Fluent Interfaces Using Lambda Expressions.

# Design with Higher-Order Functions

In Java, one of the biggest changes we have to make is to design with higher-order functions. We’re used to passing objects to methods, but now we also have the ability to pass functions as arguments. This gives us more concise code: anywhere we passed anonymous inner classes to single-method interfaces, we can now pass lambda expressions or method references.

For example, to register a simple event handler for a Swing button, we had to jump through hoops before, as in the next example.

```
button.addActionListener(new ActionListener() {
public void actionPerformed(ActionEvent event) {
JOptionPane.showMessageDialog(frame, "you clicked!");
}
}
```