
# Complete the menu: A real program

# In this chapter

- Erik and Simon create load and save functions in the main program
- Simon adds the exit function to the main menu and `get_order()` function
- Simon explains why he thinks Erik just created a real program
- The brothers discuss future plans

"Now let’s get real," Simon said. "Yesterday we played with sample orders and simple programs. Today it’s time to use what we learned in our real program."

"Yes, let’s do it!" Erik said.

"Open your file `main_menu.py` where we wrote the main menu. We should add a couple of functions to work with the JSON file. Let’s recall what we have to do first," and Simon pulled his diagram from yesterday.

![](img/f6295b544c84568800930d6ec95963ae_120_0.png)

"So we have to write a function that will load the list of orders from a JSON file. But first it has to check if the file exists. If it doesn’t, we create an empty list and return it from this function. If it exists, we read from it, convert JSON to a Python list and return that list."

"Let me help you," Simon felt that it’s a bit confusing for Erik. "Usually, in functions that work with files we pass the file name as an argument," and Simon started the function at the bottom of the file, right before the last line with main_menu():

```
def load_orders(filename):
```

"Now we have to check if the file exists. There is a special function for that and we can find it in the os module."

"What is 'os'?" Erik asked.

"OS stands for 'operating system.' Operating system in the computer manages all files and programs. It works with your screen, your keyboard, your speakers, and video camera. On a typical computer, the OS can be Windows, macOS, or Linux. In our case we are going to ask the operating system if a file with such a name exists on this computer," and Simon added a line:

```
def load_orders(filename):
    if os.path.exists(filename):
```

"Look, we used the os module here. That means we have to import it the same way we imported the json module in our sample program yesterday. In this program we haven’t imported the json or os modules yet, so let’s import them both."

Simon moved the cursor to the very beginning of the file and added two lines:

```
import os
import json
```

He returned the cursor to the load_orders() function at the end of the file and continued his explanation. "If the file exists, we open it for reading, use the json.load() function to read from the file to the list orders and return the list." He added three lines to the function.

```
def load_orders(filename):
    if os.path.exists(filename):
        f = open(filename, "r")
        orders = json.load(f)
        return orders
```

"What if it doesn’t exist? We just create an empty list and return it."

```
def load_orders(filename):
    if os.path.exists(filename):
        f = open(filename, "r")
        orders = json.load(f)
        return orders
    else:
        orders = []
        return orders
```

"Now load function is ready!" he said and looked at Erik.

"I don’t think I could write it myself," Erik said.

"Of course, it looks complicated when you do it first time. But look, you can read it as if it was plain English, can’t you?"

Erik looked at the function again and tried to read it. "If the file with 'filename' exists, then open the file. Save it in the object called 'f'. Then load the previous orders from that 'f' file into 'orders'. Hmmm… yes, I can read it." He was surprised. He could read Python now and understand it!

"The next function is easier," Simon continued. "I think you can write it yourself if you look at our file where we practiced with sample orders. Look, right after the two append() operations there are three lines that we need here. We already have a function called save_order() that does nothing except printing 'Saving order…'. Let’s replace it with a real one. I think it should be called save_orders()—plural, because now we know how to save a list of orders in a JSON file, right?"

Simon wrote the beginning of the function:

```
def save_orders(orders, filename):
```

He explained: "We pass the list of orders as a first argument. Then we pass the name of the file where we want to store it. Now you can add those three lines from our yesterday’s program."

Erik looked at the 'dict_json.py' file and copied three lines from it. Now the function looked like this:

```
def save_orders(orders, filename):
    f = open(filename, "w")
    json.dump(orders, f, indent=4)
    return
```

"Can we test it now?" he asked Simon.

"We are almost ready," Simon answered. "Look at the bottom of our file. Now we just call the main_menu() function. Remember my second diagram from yesterday?" and he pulled the drawing.

![](img/f6295b544c84568800930d6ec95963ae_123_0.png)

"This is what we do in the main_menu() function. We just have to edit it a little bit to serve several customers and save their orders in the list 'orders.' To do that we have to pass that list into the main_menu() function there is a place to save the orders as they are entered."

He moved the cursor to the beginning of the file and added the orders argument to the main_menu() definition.

```
def main_menu(orders):
```

"Now," he continued, "each time the customer enters a new order it will be appended to the 'orders' list. Before we added the list as an argument, main_menu() didn’t know where to add the new order. Now we can use the append() method and add it to 'orders.' Right after the customer confirms the order, we add it to the list of orders. We won’t use the save_order() function here. We’ll save all orders when you close the program." And Simon changed the main_menu() function to this:

```
def main_menu(orders):
    while True:
        order = get_order()
        print("Check your order:")
        print_order(order)
        confirm = input("Confirm? Press Y to confirm, N to cancel: ")
        if confirm == "Y" or confirm == "y":
            orders.append(order)
            print("Thanks for your order:")
            print_order(order)
        else:
            continue
```

"And we also change the main program to three steps: load the previous orders, main menu (get the new orders), and save all the orders." And he added those three lines to the bottom of the file. Now it looks like this:

```
orders = load_orders("orders.json")
main_menu(orders)
save_orders(orders, "orders.json")
```

"Can I try it now?" Erik asked.

"Sure, go ahead!" Simon said.

Erik ran the program, entered his name at the first prompt, then selected his drink components. When the program asked to confirm the order, he typed "Y." The program got back to the "What’s your name: " prompt.

"Okay," Erik said, "it works. But how can I check my orders? Are they saved in the file?"

"Let’s check," Simon said.

The brothers opened the 'orders.json' file and were surprised to see that it still contained the old orders from their yesterday’s experiments. Even Simon was confused.

"Let’s see," he said. "We open the file, we read from it, we get the order… But we never write to the file because we are still in the main menu taking orders! We never reach that save_orders() function! Hmmm… let me think how to fix it."

Erik smiled. His know-it-all brother didn’t know what to do.

That lasted only a moment.

Simon said: "I see. We didn’t give the user a way to exit the main menu. We keep asking the user their name, but what if we want to finish the program?"

"I saw that you pressed CONTROL C when you wanted to stop my program," Erik said.

"Yes, I did it, but that’s a not normal way of finishing programs. When I did that, the program was interrupted. Usually Python gives you an error message when you do that. When a program is interrupted that means it doesn’t do anything else: it just stops wherever it is. It doesn’t write our orders to the file, it doesn’t close the file. Pressing CONTROL C is a bad way to finish a program."

Simon paused for bit and continued: "We should give the user a normal way of finishing our program."

"Like Exit in the menu in Word?"

"Yes, like that. Let’s tell the user that if they want to exit they should enter 'X' and only 'X' when asked about their name. The probability that we’ll have a customer with a real full name 'X' is very low. Almost zero, actually. So let’s do this: if in the get_order() function the customer
