"Good idea," Simon said. "The save_order() function does nothing except printing 'Saving order...' That's okay for now, we'll write it later. Now we are ready to call the main_menu() function and test our algorithm. Go ahead and add the call for the main_menu() at the end and run it. You main program will consist of only this function call."

Erik added the function call so the whole program now looked like this:

# Listing 6.1 main_menu.py

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

def get_order():
    return {}

def print_order(order):
    print(order)
    return

def save_order(order):
    print("Saving order...")
    return

main_menu()
```

YOUR TURN   Create main menu
Write a main menu function similar to the one Erik just created. Feel free to change the dialogue messages.

He clicked Run and saw the output:

```
Check your order:
{}
Confirm? Press Y to confirm, N to cancel:
```

He typed "y" and got this:

```
Saving order...
Thanks for your order:
{}
Check your order:
{}
Confirm? Press Y to confirm, N to cancel:
```