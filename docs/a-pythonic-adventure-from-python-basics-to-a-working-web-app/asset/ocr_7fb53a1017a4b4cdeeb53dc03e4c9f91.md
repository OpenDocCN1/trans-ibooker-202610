"What is cur?" Emily asked.

"It’s called a cursor in the database world. Similar to your mouse cursor, it can point to a specific place in a database. We are not going to go too much into details. In this case I’m following the examples from the official Python documentation. The documentation says we need this cursor to execute SQL commands in the database. Look at the next line with cur.execute(). This is our first SQL instruction. It creates a table called orders with columns named name, drink, flavor, topping. In databases, tables are just normal tables with rows and columns. Each new order is a new row in the table. In each row we have four columns with names that I mentioned already. Easy, huh?" and he drew a simple table.

![](bbox=[0.15, 0.29, 0.88, 0.48])

"Now let’s rewrite the save_orders() function. First, we rename it to save_order() because we are going to save just one order. Then we repeat the first two lines from those I just wrote: create a connection and create a cursor. After that we can execute the SQL command. In this case we use the INSERT command and pass the values from the order[] dictionary. Finally, we call the commit() function to save the new order in the database."

# Listing 12.7 app.py: Add orders to the table

```
. . . .
from flask import Flask, render_template, request

def save_order(order):
    con = sqlite3.connect("orders.db")
    cur = con.cursor()
    cur.execute(
        "INSERT INTO orders(name,drink,flavor,topping) VALUES(?,?,?,?);",
        (order["name"], order["drink"], order["flavor"], order["topping"]),
    )
    con.commit()
    return
. . . .
```

YOUR TURN Create a save_order() function

Copy the save_order() function to your program. Pay attention to quotes, square brackets, commas, parentheses.