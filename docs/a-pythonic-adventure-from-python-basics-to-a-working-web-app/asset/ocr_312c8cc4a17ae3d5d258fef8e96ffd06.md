```
drinks = ["chocolate", "coffee", "decaf"]
flavors = ["caramel", "vanilla", "peppermint", "raspberry", "plain"]
toppings = ["chocolate", "cinnamon", "caramel"]

print("Erik's Coffee Shop drinks")
print("--------------------------")
i = 1
for d in drinks:
    print(i, d)
    i = i + 1
drink = input("Choose your drink: ")

print("Erik's Coffee Shop flavors")
print("--------------------------")
i = 1
for f in flavors:
    print(i, f)
    i = i + 1
flavor = input("Choose your flavor: ")

print("Erik's Coffee Shop toppings")
print("--------------------------")
i = 1
for t in toppings:
    print(i, t)
    i = i + 1
topping = input("Choose your topping: ")

print("Here is your order: ")
print("Main product: ", drinks[int(drink) - 1])
print("Flavor: ", flavors[int(flavor) - 1])
print("Topping: ", toppings[int(topping) - 1])
print("Thanks for your order!")
```

# YOUR TURN   Modify your program to print the order

Modify your program similar to what Erik just did and print the order. Don’t forget to convert the input strings to numbers. Don’t forget to subtract one (1) from each number—list indexes start with zero, remember?

He ran the program, entered 2, 1, 1 and finally got what he wanted:

```
Here is your order:
Main product:  chocolate
Flavor:  caramel
Topping:  chocolate
Thanks for your order!
>>>
```

"Cool! It works!" Erik was definitely happy. "I like my coffee shop program! Are we done with it?"

"Almost," Simon answered. "Look, you wrote almost exactly the same code three times."

"What’s wrong with that?"

"Imagine you want to change something in you code. For example, change the way you print the menu items. You’ll have to change it in all three places. Or in even more places if you decide to