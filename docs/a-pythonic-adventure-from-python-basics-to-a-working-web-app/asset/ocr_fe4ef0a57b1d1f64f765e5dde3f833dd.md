"It’s my second function and it works!" he proudly said to his brother.

"Yes, you are building your own function library already, great!" Simon said. "Now let’s copy your new function to the main program. Don’t copy the print() lines—we used them just for testing. I think that file is called menu_function.py, right?"

"Right," Erik said. "But where should I put my function in that file? In the beginning or in the end?"

"The rule in Python is that you should define your function before you start using it. Because of that usually all functions are placed in the beginning of the file, before the main program. You can place it right after your first function."

"Okay," Erik said and started working. Here is what he’s got.

# Listing 5.5 menu_files.py

```
def menu(choices, title="Erik's Menu", prompt="Choose your item: "):
    ...
    # this function didn't change
    ...
    return answer

def read_menu(filename):
    f = open(filename)
    temp = f.readlines()
    result = []
    for item in temp:
        new_item = item.strip()
        result.append(new_item)

    return result

drinks = read_menu("drinks.txt")
flavors = read_menu("flavors.txt")
toppings = read_menu("toppings.txt")

drink = menu(drinks)
flavor = menu(flavors, "Erik's flavors", "Choose your flavor: ")
topping = menu(toppings, "Erik's toppings", "Choose your topping: ")

print("Here is your order: ")
print("Main product: ", drink)
print("Flavor: ", flavor)
print("Topping: ", topping)
print("Thanks for your order")
```

**YOUR TURN**   **Copy your function to your main program**
**Copy the new read_menu() function to your main program and try it.**

He tested the program and it worked exactly as before!

"This is good," Simon said. "Now try to add something to the toppings file, for example. And see