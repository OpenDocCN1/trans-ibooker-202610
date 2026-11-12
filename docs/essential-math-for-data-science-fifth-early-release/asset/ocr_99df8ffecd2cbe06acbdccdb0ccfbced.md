```
my_value = 2 * ((3 + 2)**2 / 5) - 4
print(my_value) # prints 6.0
```

While both examples are technically correct, the latter one is more clear to us easily confused humans. If you or someone else makes changes to your code, the parentheses provide an easy reference of operation order as you make changes. This provides a line of defense against code changes to prevent bugs as well.

# Variables

If you have done some scripting with Python or another programming language, you have an idea what a variable is. In mathematics, a **variable** is a named placeholder for an unspecified or unknown number.

You may have a variable x representing any real number, and you can multiply that variable without declaring what it is. In Example 1-3 we take a variable input x from a user, and multiply it by 3.

# _Example 1-3. A variable in Python that is then multiplied_

```
x = int(input("Please input a number\\n"))
product = 3 * x
print(product)
```

There are some standard variable names for certain variable types. If these variable names and concepts are unfamiliar, no worries! But the rest of you readers might recognize we use theta θ to denote angles and beta β for a parameter in a linear regression. Greek symbols make awkward variable names in Python, so we would likely name these variables theta and beta in Python as shown in Example 1-4.

# _Example 1-4. Greek variable names in Python_

```
beta = 1.75
theta = 30.0
```