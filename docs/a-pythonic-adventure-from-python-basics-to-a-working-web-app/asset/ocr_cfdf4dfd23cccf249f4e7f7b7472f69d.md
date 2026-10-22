```
drinks = ["chocolate", "coffee", "decaf"]
flavors = ["caramel", "vanilla", "peppermint", "raspberry", "plain"]
toppings = ["chocolate", "cinnamon", "caramel"]

print("Erik's Coffee Shop drinks")
print("----------------------------")
i = 1
for d in drinks:
    print(i, d)
    i = i + 1
drink = input("Choose your drink: ")

print("Erik's Coffee Shop flavors")
print("----------------------------")
i = 1
for f in flavors:
    print(i, f)
    i = i + 1
flavor = input("Choose your flavor: ")

print("Erik's Coffee Shop toppings")
print("----------------------------")
i = 1
for t in toppings:
    print(i, t)
    i = i + 1
topping = input("Choose your topping: ")

print("Here is your order: ")
print("Main product: ", drinks[int(drink)])
print("Flavor: ", flavors[int(flavor)])
print("Topping: ", toppings[int(topping)])
print("Thanks for your order!")
```

Erik ran the program, entered his choices (coffee, caramel, chocolate) and got this output:

```
Here is your order:
Main product:  decaf
Flavor:  vanilla
Topping:  cinnamon
Thanks for your order!
>>>
```

"What?? It’s all wrong!" Erik exclaimed. "This is not what I chose!"

"I didn’t tell you one important thing about Python lists. Their indices start with zero, not one. So if you want to get the first item from the list, you should put zero in the square brackets. If you want the second item, you use one as the index."

"But why??" Erik was shocked by such a strange thing.

"It’s a long story," Simon answered. "It comes from the way computers store lists in memory. The index you use is the number of items you should skip from the beginning of the list to get the item you want. If you want the first item of the list you don’t have to skip any items. You just take it from the beginning of the list. So the number of items you should skip is zero, right? That’s why the first element’s index is zero. Look here," and Simon draw another picture.