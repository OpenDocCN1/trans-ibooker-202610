
![](img/f6295b544c84568800930d6ec95963ae_98_0.png)

"Let's start with get_name. What do we do here?" Simon asked.

"We just ask 'what is your name?'," Erik answered.

"Right, and then?"

"And then we save it in a variable, like name."

"Almost right," Simon said. "Remember, we decided that order will be a dictionary. And we will save everything related to that order in this dictionary. For example, to save the customer's name instead of name = 'Erik' we should write order['name'] = 'Erik'. Only instead of 'Erik' we will use the function input() like you did in your first program."

"Let me try," Erik said and wrote this function:

```
def get_order():
    order['name'] = input("What's your name: ")
    return {}
```

"Now try it," Simon said.

Erik clicked Run, the program asked his name and he entered 'Erik'. But then he's got several
lines of error messages.

```
What's your name: Erik
Traceback (most recent call last):
  File "/home/erik/mu_code/main_menu.py", line 41, in <module>
    main_menu()
  File "/home/erik/mu_code/main_menu.py", line 3, in main_menu
    order = get_order()
  File "/home/erik/mu_code/main_menu.py", line 27, in get_order
    order['name'] = input("What's your name: ")
NameError: name 'order' is not defined
>>>
```

"What’s that???" he asked Simon.

"Look, Python tells you where your problem is occurring. Read the last line."

"Name 'order' is not defined," Erik read it.

"It’s very simple," Simon explained. "You tried to put something in the dictionary but you haven’t created it yet. It’s easy to fix. Let’s create an empty dictionary. Remember, we used curly braces for that? Just write order = {} before the line with input ()."

Erik changed his function to this:

```
def get_order():
    order = {}
    order['name'] = input("What's your name: ")
    return {}
```

and ran it again. This time it didn’t give him any errors.

```
What's your name: Erik
Check your order:
{}
Confirm? Press Y to confirm, N to cancel: y
Saving order...
Thanks for your order:
{}
What's your name:
```

"It’s better now," he said.

"Yes, better, but look: it still prints an empty order. You created an order and even entered your name into it, but your function returns an empty dictionary. See this line: return {}?"

"But this is how you wrote it!" Erik was sure it’s not his fault.

"Yes, I wrote it this way to test the main menu function. But now we have to return the actual order dictionary. Change it to return order and let’s see if it prints your name."

Erik changed the function to this:

```
def get_order():
    order = {}
    order['name'] = input("What's your name: ")
    return order
```

and ran it again. This time he saw:

```
What's your name: Erik
Check your order:
{'name': 'Erik'}
Confirm? Press Y to confirm, N to cancel:
```

"Yes, it prints my name now!"

"Congratulations!" Simon said. "Now you know how to work with dictionaries!"

## YOUR TURN   Create your get_order() function
Start writing your own get_order() function in the main_menu.py file. Add the first input to get the customer's name. Test it by running the main menu program.

### What are your choices?

"Let’s move on," Simon continued. "We have to add your menu() function now to fill drinks and flavors. But we also need the read_menu() function to read your menus from files. Copy both of them (menu() and read_menu() from the menu_files.py file and paste them here in the main_menu.py. Paste them right before the def get_order() : line."

"What if I paste it after that line?" Erik wanted to know why his older brother gave him such strict orders.

"Then it won’t work," Simon gave him a simple answer and smiled. "Okay, if you really want to know: we are going to use these two functions in the get_order() function. First we have to read the menu contents from files: your drinks, your flavors, your toppings. Then we call the menu() function three times to get the customer choices. And before we can use these functions we should define them. In other words, we should tell Python that such functions exist and what they do. That’s why they need to be pasted before the def get_order() : line." By the way, this is why we use the word def to start a function—we define it."

"Okay," Erik said and started copying the functions. In a couple of moments his main_menu.py file looked like this:

# Listing 7.1 main_menu.py

```
def main_menu():
    while True:
        order = get_order()
        print("Check your order:")
        print_order(order)
        confirm = input("Confirm? Press Y to confirm, N to cancel: ")
        if confirm == "Y" or confirm == "y":
            save_order(order)
            print("Thanks for your order:")
            print_order(order)
        else:
            continue

def menu(choices, title="Erik's Menu", prompt="Choose your item: "):
    print(title)
    print(len(title) * "-_")
    i = 1
    for c in choices:
        print(i, c)
        i = i + 1
    while True:
        choice = input(prompt)
        allowed_answers = []
        for a in range(1, len(choices) + 1):
            allowed_answers.append(str(a))

        allowed_answers.append("X")
        allowed_answers.append("x")

        if choice in allowed_answers:
            if choice == "X" or choice == "x":
                answer = ""
                break
            else:
                answer = choices[int(choice) - 1]
                break
        else:
            print("Enter number from 1 to ", len(choices))
            answer = ""
    return answer

def read_menu(filename):
    f = open(filename)
    temp = f.readlines()
    result = []
    for item in temp:
        new_item = item.strip()
        result.append(new_item)
    return result
# . . .
# No changes in get_order() and save_order() functions
# . . .
main_menu()
```

"Correct!" Simon said. "Now you know the rule: define something before using it. You just saw the problem when you got errors with the order dictionary, and now you see it with the menu() and read_menu() functions here."

"Now we are ready to use these functions in get_order()," he continued. "Look at your menu_files.py program. What did we do first?"

"We read the menus from files," Erik answered.

"Good, let’s do it here, but inside the function."

Erik added three lines to the get_order() function:

```
def get_order():
    order = {}
    order["name"] = input("What's your name: ")
    drinks = read_menu("drinks.txt")
    flavors = read_menu("flavors.txt")
    toppings = read_menu("toppings.txt")
    return order
```

He had to adjust the lines by adding four spaces before each line so they were all indented at the same level.

"And now the same with three menu() functions?" he asked Simon.

"Sure, go ahead!"

Erik changed his function to this:

```
def get_order():
    order = {}
    order["name"] = input("What's your name: ")
    drinks = read_menu("drinks.txt")
    flavors = read_menu("flavors.txt")
    toppings = read_menu("toppings.txt")
    drink = menu(drinks, "Erik's drinks", "Choose your drink: ")
    flavor = menu(flavors, "Erik's flavors", "Choose your flavor: ")
    topping = menu(toppings, "Erik's toppings", "Choose your topping: ")
    return order
```

He was proud of his work and looked at Simon.

"Almost right," Simon said. "You copied it right, but you have to change the code a bit to store the answers in the order dictionary. It should be an easy change, you know how to do it."

"Ah, I see," Erik said and changed the function. Now the function looked like this:

# Listing 7.2 main_menu.py

```
def get_order():
    order = {}
    order["name"] = input("What's your name: ")
    drinks = read_menu("drinks.txt")
    flavors = read_menu("flavors.txt")
    toppings = read_menu("toppings.txt")
    order["drink"] = menu(drinks, "Erik's drinks",
        "Choose your drink: ")
    order["flavor"] = menu(flavors, "Erik's flavors",
        "Choose your flavor: ")
    order["topping"] = menu(toppings, "Erik's toppings",
        "Choose your topping: ")
    return order
```
