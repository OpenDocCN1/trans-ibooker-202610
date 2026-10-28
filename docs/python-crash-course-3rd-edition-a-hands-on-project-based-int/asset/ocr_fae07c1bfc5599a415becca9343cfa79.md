Most of this code is unchanged. The second `elif` block now checks to make sure a person is less than age 65 before assigning them the full admission rate of $40. Notice that the value assigned in the `else` block needs to be changed to $20, because the only ages that make it to this block are for people 65 or older.

# Omitting the else Block

Python does not require an `else` block at the end of an `if-elif` chain. Sometimes, an `else` block is useful. Other times, it’s clearer to use an additional `elif` statement that catches the specific condition of interest:

```
```python
age = 12

if age < 4:
    price = 0
elif age < 18:
    price = 25
elif age < 65:
    price = 40
elif age >= 65:
    price = 20

print(f"Your admission cost is ${price}.")
```
```

The final `elif` block assigns a price of $20 when the person is 65 or older, which is a little clearer than the general `else` block. With this change, every block of code must pass a specific test in order to be executed.

The `else` block is a catchall statement. It matches any condition that wasn’t matched by a specific `if` or `elif` test, and that can sometimes include invalid or even malicious data. If you have a specific final condition you’re testing for, consider using a final `elif` block and omit the `else` block. As