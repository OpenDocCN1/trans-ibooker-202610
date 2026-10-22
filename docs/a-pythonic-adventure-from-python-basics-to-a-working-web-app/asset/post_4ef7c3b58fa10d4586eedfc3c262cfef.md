
Erik clicked [New] in the editor, then [Save], typed menu_function.py, and was ready to write code.

"Functions in Python start with the word def followed by the name of the function," Simon continued. "Let’s call our function menu. Then you open the parentheses and list your arguments."

Erik wrote def menu( and wasn’t sure what to do next.

Simon helped: "We just talked about your arguments. I see you are thinking how to name them. Remember, naming variables and arguments is one of the most difficult problems in computer science? You are not alone. Let’s name them: choices, title, and prompt. Just type them between the parentheses and put a colon after the closing one."

Erik typed the following:

```
def menu(choices, title, prompt):
```

He noticed that after he pressed ENTER, the cursor moved to the next line, but four spaces to the right. "Should I write here?" he asked Simon.

"Yes, sure!" Simon answered. "You see: the editor is helping you to write your function! Now look at your yesterday code and start copying what you want to put in the function. Look: first we print the title. Let’s do it here too, but instead of the actual string we just print the argument called title. You can even put the line of dashes after it, like you did before."

Erik wrote:

```
def menu(choices, title, prompt):
    print(title)
    print("----------------------------")
```

"Now write the loop," Simon continued. "But instead of drinks or flavors, your list is now called choices. And you can use the variable c in the loop, as the first letter of 'choice'."

Erik copied the loop from his previous script. He got the idea now and added the input() function with the prompt even without asking his brother.

```
def menu(choices, title, prompt):
    print(title)
    print("----------------------------")
    i = 1
    for c in choices:
        print(i, c)
        i = i + 1
    choice = input(prompt)
```

"Great!" Simon said when he saw Erik’s code. "Now we have to return the choice."

"Can't we just print the choice variable in our main program?" Erik asked.

"No! And this is a very important thing about functions," Simon was glad that Erik asked this question. "The variables you have inside your function are visible only within the function. Look here, I'll draw a picture."

![](img/f6295b544c84568800930d6ec95963ae_42_0.png)

Visible means that you can't see what's inside those variables when you are not inside the function. So if we want our main program to see their values we have to return these variables. Usually we have many variables inside a function, but we want to return only one or two as a result. In this case we can return the number that the user entered, which is stored in the variable choice."

Simon paused and thought for a moment.

"But we can do better," he said. "Look at your yesterday code again. What else is repeating?"

Erik looked and said: "Those int() functions and also that we had to add - 1 three times. That

was annoying," he still hasn’t gotten used to the fact that list indices start with zero.

"Okay, let’s add them to the function too," Simon suggested. "We will convert the user’s answer to integer, get the item from the list, and return the item, not its index. That will make our function even more useful. The main program that calls it will get the user’s choice, not just some number. Let me show you," and Simon added the conversion operations and the return statement to the function.

```
def menu(choices, title, prompt):
    print(title)
    print("-----------------------------")
    i = 1
    for c in choices:
        print(i, c)
        i = i + 1
    choice = input(prompt)
    answer = choices[int(choice) - 1]

    return answer
```

"Now let’s test it," Simon said. "In our main program that goes just below the function we will call it and print the answer we get. But first, we need our lists with drinks and everything. Copy them from the top of your yesterday’s program."

Erik added three lists just below the function. This time all the lines were not shifted and began in the first position.

```
drinks = ["chocolate", "coffee", "decaf"]
flavors = ["caramel", "vanilla", "peppermint", "raspberry", "plain"]
toppings = ["chocolate", "cinnamon", "caramel"]
```

"Good," Simon said. "Now you are ready to call your function. Pass the title, the list of drinks, and the input prompt. The result from the function assign to a variable. Call it choice, for example. And then print it."

Erik followed Simon’s instructions and here is what he’s got:

```
def menu(choices, title, prompt):
    print(title)
    print("-----------------------------")
    i = 1
    for c in choices:
        print(i, c)
        i = i + 1
    choice = input(prompt)
    answer = choices[int(choice) - 1]

    return answer

drinks = ["chocolate", "coffee", "decaf"]
flavors = ["caramel", "vanilla", "peppermint", "raspberry", "plain"]
toppings = ["chocolate", "cinnamon", "caramel"]
choice = menu(drinks, "Erik's drinks", "Choose your drink: ")
print(choice)
```

## YOUR TURN Create your own function

Create your own function similar to what Erik just created. Use your menu lists, titles, and prompts. Try to run it (before Erik!).

"Should I run it?" Erik asked.

"Yes, it’s now ready, go ahead!"

Erik clicked [Run] and the program asked him about his drink, exactly the same way it did before. Erik answered and got the result he expected.

```
Erik's drinks
-----------------------------
1 chocolate
2 coffee
3 decaf
Choose your drink: 2
coffee
>>>
```

"It works!" he said. "I’ll add the other menus here," and he started writing. In 10 minutes or less he was ready to test the whole program. Now it looked like this:

```
def menu(choices, title, prompt):
    print(title)
    print("-----------------------------")
    i = 1
    for c in choices:
        print(i, c)
        i = i + 1
    choice = input(prompt)
    answer = choices[int(choice) - 1]
    
    return answer

drinks = ["chocolate", "coffee", "decaf"]
flavors = ["caramel", "vanilla", "peppermint", "raspberry", "plain"]
toppings = ["chocolate", "cinnamon", "caramel"]

drink = menu(drinks, "Erik's drinks", "Choose your drink: ")
flavor = menu(flavors, "Erik's flavors", "Choose your flavor: ")
topping = menu(toppings, "Erik's toppings", "Choose your topping: ")

print("Here is your order: ")
print("Main product: ", drink)
print("Flavor: ", flavor)
print("Topping: ", topping)
print("Thanks for your order!")
```

## YOUR TURN Add other menus

Add other menus to your program. They will use the same function but with different arguments: lists of choices, titles, and prompts. Try to run it and test with your menu choices.

And it worked as expected! Erik ran the script, entered 2, 2, 1 again and got his order:

```
Here is your order:
Main product:  coffee
Flavor:  vanilla
Topping:  chocolate
Thanks for your order!
>>>
```

Simon said: "Notice that your program became shorter. And now if you have to change something, you change it only in one place."

"Why would I want to change it? It works well already," Erik said.

"Oh, there are always ways to improve your code!" Simon answered. "Let’s make its title a little bit better. Did you notice that your line of dashes is now longer than the title?"

"Really? Oh, yes, you are right," Erik said. "It’s because I changed the title to just 'Erik’s drinks'. I can make it shorter, that’s easy."

"Sure, you can make it shorter, but look—in the 'drinks' menu the title is shorter, in the 'toppings' menu it’s longer. You have to print the line of dashes that will be the same length as your title string."

"But how can I do it? Maybe, it’s better to remove that line of dashes?" Erik was confused.

"No, I like your line here; it makes your receipt look more real. I want you to keep it. But we have to calculate the length of our title and make it the same length. We’ll learn one new function and one new operation here. The function we are going to use is called len(). You just put a string argument inside the parentheses and it returns the length of the string. Let me show you. We’ll use another feature of Mu Editor, called REPL. It stands for 'Read-Eval-Print-Loop' and means that you can use Python interactively. I usually use it when I want to test something quickly. Or to show something to somebody, like now," and Simon smiled.

"Click [REPL] [icon]," Simon continued.

Erik clicked and another window opened at the bottom of the Mu Editor window.

![](img/f6295b544c84568800930d6ec95963ae_45_0.png)
