means now we have to learn how to keep several orders and store them in a file. We know already how Python stores several items that come in order—you used it for you menus.

"A list!" Erik said.

"Correct! A list in Python can contain different things: strings, numbers, even dictionaries. In this case we’ll have a list of dictionaries. Each dictionary will contain an order we’ll add them one by one to the list. A new customer, a new order, a new dictionary in the list. Let me draw a diagram."

![](bbox=[0.125, 0.26, 0.875, 0.515])

"Let’s create a list of orders," Simon continued. "Copy the existing order in the code and call it order1, for example.

Then change the order’s content: the name, the drink, and others."

Erik worked on his code for a while and finally got this additional order, just below the first one.

```
order1 = {
    "name": "Alex",
    "drink": "choco",
    "flavor": "vanilla",
    "topping": "caramel"
}
```

"Good," Simon said, "now create an empty list called orders. Note that it’s plural—'orders'. It’s very similar to creating an empty dictionary—you have done that already. Just instead of curly braces use square brackets."

Erik added the following line below the second order:

```
orders = []
```

"And now we will add both orders to the list," Simon said. "Believe it or not, but the list orders