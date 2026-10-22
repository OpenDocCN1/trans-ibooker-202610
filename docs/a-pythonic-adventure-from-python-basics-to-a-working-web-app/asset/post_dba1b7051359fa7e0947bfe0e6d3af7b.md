
print("Welcome to Erik's Coffee Shop!")

answer = input("Please enter your name: ")
print("Here is your order, ", answer)
```

Simon noticed that Erik was looking for the [Run] button and explained: "Before clicking [Run] again, you have to stop your previous Python session. See these three angle brackets here? They mean that Python is running and waiting for your input. We will use it later, but for now, just click [Stop] and then click [Run] again."

Now the program asked for the order and answered exactly as Erik programmed it.

```
Welcome to Erik's Coffee Shop!
Please enter your name: Erik
Here is your order, Erik
>>>
```

YOUR TURN   Make your program more user-friendly
Add the prompt and the output string to your first program. Create a different prompt asking the client their name, like "Glad to see you! What's your name? "

"Looks much more user-friendly, doesn't it?" Simon said. "Always think about your users and ask yourself: Is it clear enough what I expect from the user? Can they possibly make a mistake here?"

"Now," Simon continued, "we have to ask your client about their order. You said you have coffee and chocolate. And also you said something about flavors and toppings?"

"Yes," Erik said, "I want to ask them which topping and flavor they want."

"Well, go ahead and ask them. You can just repeat the same code—but don't forget to change the prompts. And I think you should print the whole order at the end, not after each question. Try it."

Erik wrote this code and stopped at the last line.

```
print("Welcome to Erik's Coffee Shop!")

answer = input("Please enter your name: ")
answer = input("Please enter your drink: ")
answer = input("Please enter your flavor: ")
answer = input("Please enter your topping: ")
print("Here is your order: ", answer)
```

"You told me to put the answers in the answer variable. But how do I know now which is the flavor and the topping?" Erik was confused.

"Yes, I told you to put the answers in a variable, for example called 'answer'," Simon answered. Here we come to one of the most difficult problems in computer science: naming variables", he smiled. "Of course, you don’t store all the answers in the variable called answer. Let’s use different variables for different answers and give them meaningful names. For the client’s name we’ll use a variable called name--that’s easy. If you ask about a main drink, put the answer in the variable drink or product. For the flavor and topping answers use the variables flavor and topping."

"At the end," Simon continued, "print each variable on a separate line, using several print() functions. Go ahead. I will help you if necessary."

Erik worked on his code and finally produced this:

## Listing 1.1 coffeeshop.py

```
print("Welcome to Erik's Coffee Shop!")

name = input("Please enter your name: ")
drink = input("Please enter your drink: ")
flavor = input("Please enter your flavor: ")
topping = input("Please enter your topping: ")
print("Here is your order, ", name)
print("Main product: ", drink)
print("Flavor: ", flavor)
print("Topping: ", topping)
print("Thanks for your order!")
```

Erik clicked Run, and his program started a dialogue. Erik answered all the questions and got a nice output:

```
Welcome to Erik's Coffee Shop!
Please enter your name: Erik
Please enter your drink: coffee
Please enter your flavor: caramel
Please enter your topping: chocolate
Here is your order, Erik
Main product: coffee
Flavor: caramel
Topping: chocolate
Thanks for your order!
>>>
```

Simon noticed the last line and praised Erik for his initiative: "It’s always good to thank your customers."

"Yes, I saw that on several receipts in coffee shops," Erik said. He was glad he had done something on his own besides what his older brother told him.

YOUR TURN  Add more options to the dialogue. Use variables.
Edit your previous program and add the other lines to the dialogue. Again, feel free to change the prompts and strings you print to something more suitable to your project.
Change your printed output. Look at the receipts from the places you visit (coffee shops, restaurants, groceries, other shops). Try to make your printed output look similar. Use text symbols like |, _, =, +, and others to make your output look interesting.

Simon decided it was time to wrap up for today.

"I think it was a good start today," he said. "Let’s see what we have done today. First, we installed your programming environment."

"Yes," Erik said. "I like this Mu Editor. It uses colors to show me different parts of the program. And also it shows my string in red until I put the quotes at the end. And it has a dark mode! I know that *real programmers* always use the dark mode! I think I will continue using it."

"Second," Simon said, "we used a *function* for the first time. What was it?"

"It was the `print()` function," Erik said. "I told it what to print and it printed it."

"Right. You *called* the function and *passed and argument* to it."

"Third," Simon continued, "you used another function to get information from the user."

"It was `input()`," said Erik. "And I saved the answers in *variables*."

"Great!" Simon was really proud of his brother. "You are making good progress."

YOUR TURN  Explain it yourself
Try to explain it in your own words.

- What is a function? Give some examples.
- What are function arguments? Give some examples.
- How do you call a function?
- What is a variable? Why do we need them?
- How should we name our variables? Why?

## What is a program?

"Finally, let’s add a bit more theory," Simon said. "We just built a very simple program. But it has all the main components of any other program. We have asked the user for some *input*. After we received data from the user, we did something with that data. We usually call it *processing*. In our case, we just stored that data, but we could do something else with it, right?"

"For example?" Erik asked.

"For example, you’ve entered 'coffee' in lower case, but we may want to start all products and flavors from a capital letter. There is a special function in Python for that. So we can *process* the data after we received it."

"Great idea, I want to add it!" Erik said.

"Sure, we’ll do it. And finally, after we processed the data, we printed it out. In other words, we produced some *output*. Look here."

![](img/f6295b544c84568800930d6ec95963ae_20_0.png)

"Input doesn’t always come from a user," Simon continued. "Sometimes there is no interaction with a user, and the program takes data from somewhere else. For example, from the Internet, like recent sports results. Sometimes from sensors, like in robotics. Or from documents and images."

"Very often the output is not just a print output. In my robotics team we get inputs from sensors, we process them in the microcontroller, and our output is the signals to motors like: 'turn left, move forward.' But the structure is still the same: input → processing → output."

"Enough theory," Simon said. "Tomorrow we’ll work on improving your program."

"Improving?" Erik was surprised. "But it works fine already, doesn’t it?"

"What if your user enters something you don’t have in your shop?" Simon asked. "Like 'maple syrup'? What will you do? You should tell your user what you have in your coffee shop and what they can order. So, tomorrow we’ll work on *menus*. And also, we’ll see what we can do in case of errors."

## New terms we have learned today

+   - **Variable**
+     a place (a box) where we can store some values. For example, we can store numbers, letters, strings. A variable can hold only one thing at a time.
+   - **Function**
+     a piece of code that does something and that we want to be able to do it again.
+   - **Function arguments**
+     information that the function needs to do its job.
+   - **To call a function**
+     to write the name of the function with parenthesis and arguments between them.

## Code for this chapter

You can find the code for this chapter here:
https://github.com/pavelanni/pythonicadventure-code/tree/main/ch01

## More ideas for your projects

Find more ideas for your applications at the book companion site:
https://pythonicadventure.com/projects/. Don’t worry about Flask, SQLite, and web application (that are mentioned there) for now. We’ll get there later.
