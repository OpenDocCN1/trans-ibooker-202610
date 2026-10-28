
passed or not. The last `if` statement checks whether extra cheese was requested, regardless of the results from the first two tests. These three independent tests are executed every time this program is run.

Because every condition in this example is evaluated, both mushrooms and extra cheese are added to the pizza:

```
Adding mushrooms.
Adding extra cheese.

Finished making your pizza!
```

This code would not work properly if we used an `if-elif-else` block, because the code would stop running after only one test passes. Here’s what that would look like:

```python
requested_toppings = ['mushrooms', 'extra cheese']

if 'mushrooms' in requested_toppings:
    print("Adding mushrooms.")
elif 'pepperoni' in requested_toppings:
    print("Adding pepperoni.")
elif 'extra cheese' in requested_toppings:
    print("Adding extra cheese.")

print("\nFinished making your pizza!")
```

The test for `'mushrooms'` is the first test to pass, so mushrooms are added to the pizza. However, the values `'extra cheese'` and `'pepperoni'` are never checked, because Python doesn’t run any tests beyond the first test that passes in an `if-elif-else` chain. The customer’s first topping will be added, but all of their other toppings will be missed:

```
Adding mushrooms.

Finished making your pizza!
```

In summary, if you want only one block of code to run, use an `if-elif-else` chain. If more than one block of code needs to run, use a series of independent `if` statements.

## Try It Yourself

- **5-3. Alien Colors #1:** Imagine an alien was just shot down in a game. Create a variable called `alien_color` and assign it a value of `'green'`, `'yellow'`, or `'red'`.

  Write an `if` statement to test whether the alien's color is green. If it is, print a message that the player just earned 5 points.

  Write one version of this program that passes the `if` test and another that fails. (The version that fails will have no output.)

- **5-4. Alien Colors #2:** Choose a color for an alien as you did in Exercise 5-3, and write an `if-else` chain.

  If the alien's color is green, print a statement that the player just earned 5 points for shooting the alien.

  If the alien's color isn't green, print a statement that the player just earned 10 points.

  Write one version of this program that runs the `if` block and another that runs the `else` block.

- **5-5. Alien Colors #3:** Turn your `if-else` chain from Exercise 5-4 into an `if-elif-else` chain.

  If the alien is green, print a message that the player earned 5 points.

  If the alien is yellow, print a message that the player earned 10 points.

  If the alien is red, print a message that the player earned 15 points.

  Write three versions of this program, making sure each message is printed for the appropriate color alien.

- **5-6. Stages of Life:** Write an `if-elif-else` chain that determines a person's stage of life. Set a value for the variable `age`, and then:

  - If the person is less than 2 years old, print a message that the person is a baby.
  - If the person is at least 2 years old but less than 4, print a message that the person is a toddler.
  - If the person is at least 4 years old but less than 13, print a message that the person is a kid.
  - If the person is at least 13 years old but less than 20, print a message that the person is a teenager.
  - If the person is at least 20 years old but less than 65, print a message that the person is an adult.
  - If the person is age 65 or older, print a message that the person is an elder.

- **5-7. Favorite Fruit:** Make a list of your favorite fruits, and then write a series of independent `if` statements that check for certain fruits in your list.

  Make a list of your three favorite fruits and call it `favorite_fruits`.

  Write five `if` statements. Each should check whether a certain kind of fruit is in your list. If the fruit is in your list, the `if` block should print a statement, such as *You really like bananas!*

## Using if Statements with Lists

You can do some interesting work when you combine lists and `if` statements. You can watch for special values that need to be treated differently than other values in the list. You can efficiently manage changing conditions, such as the availability of certain items in a restaurant throughout a shift. You can also begin to prove that your code works as you expect it to in all possible situations.

### Checking for Special Items

This chapter began with a simple example that showed how to handle a special value like `'bmw'`, which needed to be printed in a different format than other values in the list. Now that you have a basic understanding of conditional tests and `if` statements, let’s take a closer look at how you can watch for special values in a list and handle those values appropriately.

Let’s continue with the pizzeria example. The pizzeria displays a message whenever a topping is added to your pizza, as it’s being made. The code for this action can be written very efficiently by making a list of toppings the customer has requested and using a loop to announce each topping as it’s added to the pizza:

**toppings.py**

```python
requested_toppings = ['mushrooms', 'green peppers', 'extra cheese']

for requested_topping in requested_toppings:
    print(f"Adding {requested_topping}.")

print("\nFinished making your pizza!")
```

The output is straightforward because this code is just a simple for loop:

```
Adding mushrooms.
Adding green peppers.
Adding extra cheese.

Finished making your pizza!
```

But what if the pizzeria runs out of green peppers? An `if` statement inside the `for` loop can handle this situation appropriately:

```python
requested_toppings = ['mushrooms', 'green peppers', 'extra cheese']

for requested_topping in requested_toppings:
    if requested_topping == 'green peppers':
        print("Sorry, we are out of green peppers right now.")
    else:
        print(f"Adding {requested_topping}.")

print("\nFinished making your pizza!")
```

This time, we check each requested item before adding it to the pizza. The `if` statement checks to see if the person requested green peppers. If so, we display a message informing them why they can’t have green peppers. The `else` block ensures that all other toppings will be added to the pizza.

The output shows that each requested topping is handled appropriately.

```
Adding mushrooms.
Sorry, we are out of green peppers right now.
Adding extra cheese.

Finished making your pizza!
```

### Checking That a List Is Not Empty

We’ve made a simple assumption about every list we’ve worked with so far: we’ve assumed that each list has at least one item in it. Soon we’ll let users provide the information that’s stored in a list, so we won’t be able to assume that a list has any items in it each time a loop is run. In this situation, it’s useful to check whether a list is empty before running a `for` loop.

As an example, let’s check whether the list of requested toppings is empty before building the pizza. If the list is empty, we’ll prompt the user and make sure they want a plain pizza. If the list is not empty, we’ll build the pizza just as we did in the previous examples:

```python
requested_toppings = []

if requested_toppings:
    for requested_topping in requested_toppings:
        print(f"Adding {requested_topping}.")
    print("\nFinished making your pizza!")
else:
    print("Are you sure you want a plain pizza?")
```

This time we start out with an empty list of requested toppings. Instead of jumping right into a `for` loop, we do a quick check first. When the name of a list is used in an `if` statement, Python returns `True` if the list contains at least one item; an empty list evaluates to `False`. If `requested_toppings` passes the conditional test, we run the same `for` loop we used in the previous example. If the conditional test fails, we print a message asking the customer if they really want a plain pizza with no toppings.

The list is empty in this case, so the output asks if the user really wants a plain pizza:

```
Are you sure you want a plain pizza?
```

If the list is not empty, the output will show each requested topping being added to the pizza.

### Using Multiple Lists

People will ask for just about anything, especially when it comes to pizza toppings. What if a customer actually wants french fries on their pizza? You can use lists and `if` statements to make sure your input makes sense before you act on it.

Let’s watch out for unusual topping requests before we build a pizza. The following example defines two lists. The first is a list of available toppings at the pizzeria, and the second is the list of toppings that the user has requested. This time, each item in `requested_toppings` is checked against the list of available toppings before it’s added to the pizza:

```python
available_toppings = ['mushrooms', 'olives', 'green peppers', 'pepperoni', 'pineapple', 'extra cheese']

# ❶
requested_toppings = ['mushrooms', 'french fries', 'extra cheese']

for requested_topping in requested_toppings:
    # ❷
    if requested_topping in available_toppings:
        print(f"Adding {requested_topping}.")
    # ❸
    else:
        print(f"Sorry, we don't have {requested_topping}.")

print("\nFinished making your pizza!")
```
