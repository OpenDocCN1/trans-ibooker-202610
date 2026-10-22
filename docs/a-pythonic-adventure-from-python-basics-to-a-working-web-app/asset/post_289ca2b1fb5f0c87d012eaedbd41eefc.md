
len(choices)+1

"And finally in this loop (label 3), we convert each number from the sequence to a string and add it to the end of the list of allowed answers. This function is called append()." Simon finished his explanation and added: "Yes, it’s a bit complicated for the first time, but try to read the Python code here yourself and you’ll understand it as if it’s plain English."

"Now we have to change our list with 1, 2, 3 to the list of allowed answers that we just built," Simon made that change, looked at the code, and slapped his forehead: "Oh, I just noticed!"

"What?" Erik thought they have finished already. But it looked like there was something else.

"We also have to change our message," Simon said. "Because our function now can accept menu lists of any length, we should tell the user something like: 'Enter a number from 1 to 6' or 'from 1 to 12,' depending on the length of our menu. Remember how we can get the length of a list?"

"With the len() function?" asked Erik.

"Of course!" Simon said, and made the final change in the function. It now looked like this:

```
def menu(choices, title="Erik's Menu", prompt="Choose your item: "):
    print(title)
    print(len(title) * "-")
    i = 1
    for c in choices:
        print(i, c)
        i = i + 1
    while True:
        choice = input(prompt)
        allowed_answers = []
        for a in range(1, len(choices)+1):
            allowed_answers.append(str(a))

        if choice in allowed_answers:
            answer = choices[int(choice) - 1]
            break
        else:
            print("Enter number from 1 to ", len(choices))
            answer = ''

    return answer
```

## YOUR TURN  Add the allowed_answers list

Add the list of allowed answers to your function. Test whether it allows you to use menu lists of different length.

"Now we are protected from user errors!" Simon said. "Try it and see if it works!"

Erik ran the program again and entered '42', 'coffee', 'weryiuryt587456' but the program didn’t crash like before. Every time, it gave him a reminder that he should use a number and it should be between 1 and 3 or 5, depending on the menu.

"This is cool! It works and it doesn’t crash!" Erik was really happy that he created such a robust program.

"One more thing," Simon said. "This one will be really-really final for today, I promise!"

"Okay," said Erik. He started to like this programming thing. He liked that his program now looked like a real one, and it worked! Even if it wasn’t an online or mobile application yet, it worked like a chat with a shop. Erik imagined that he was texting with his favorite coffee shop, ordering his drinks, and then coming to pick them up.

Simon said, "Your program now doesn’t let me enter anything except the numbers from 1 to 3 or 5. But what if I want to skip something? Like I don’t want any toppings on my drink?"

"In my flavors menu I have 'plain,' which means 'no flavor.' I can add the same to toppings," Erik answered.

"That works too," Simon said. "But in general with every menu you should give your user an option to exit the menu. Usually people use something like 'Click X to exit from this menu.' I think we should add this to our menu function too."

"How do we do it?" Erik asked. He thought a little and said, "I know! We will add 'X' to the list of allowed answers! Am I right?"

"Absolutely!" Simon was very glad to see his brother making such a good progress. "Remember, we used the function called append() to add items to the list? We can use it here, right after we finished adding numbers to the allowed_answers."

"Let me try," Erik said and started typing.

"Sure, go ahead," Simon encouraged his brother. "Just make sure you add it after that for-loop. Better to add an empty line after it; that way you will be able to see that it’s not part of the loop."

Here is Erik’s new version of the menu function:

```
def menu(choices, title="Erik's Menu", prompt="Choose your item: "):
    print(title)
    print(len(title) * "_")
    i = 1
    for c in choices:
        print(i, c)
        i = i + 1
    while True:
        choice = input(prompt)
        allowed_answers = []
        for a in range(1, len(choices)+1):
            allowed_answers.append(str(a))

        allowed_answers.append('X')

        if choice in allowed_answers:
            answer = choices[int(choice) - 1]
            break
        else:
            print("Enter number from 1 to ", len(choices))
            answer = ''

    return answer
```

"Nice," Simon said. "I would also add the lower-case letter 'x', because that’s what most people would type. Now what should we do if the user types 'x'?"

"Exit the menu loop?" Erik said.

"Right! But what are we going to return to the main program? Normally we return the user’s choice from the menu: coffee or chocolate or whatever. What if the user types 'x'? What should we return?"

"Nothing?" Erik suggested.

"Yes, we just return an empty string," Simon said. "If the user types 'x' you just assign to answer an empty string like this: " and return the answer the same way you return it if the answer is in the menu."

Simon continued, "Important thing: you should do that check before you try to convert it into a number, but after you check whether it’s in the allowed_answers list. Do you see where to put this check?"

"Yes, right after this line: if choice in allowed_answers."

"Great! Go ahead an add it! In this case you will have a nested if statement: one if inside of another if. This is pretty common, sometimes you see three levels of nested 'ifs' or even more. Just make sure your indentations are correct. This is how Python tells the computer what to do if the condition is true or false."

Erik worked more on his code and finally got this:

```
def menu(choices, title="Erik's Menu", prompt="Choose your item: "):
    print(title)
    print(len(title) * "-")
    i = 1
    for c in choices:
        print(i, c)
        i = i + 1
    while True:
        choice = input(prompt)
        allowed_answers = []
        for a in range(1, len(choices)+1):
            allowed_answers.append(str(a))

        allowed_answers.append('X')
        allowed_answers.append('x')

        if choice in allowed_answers:
            if choice == 'X' or choice == 'x':
                answer = ''
                break
            else:
                answer = choices[int(choice) - 1]
                break
        else:
            print("Enter number from 1 to ", len(choices))
            answer = ''

    return answer
```

## YOUR TURN   Add the exit option

Add the 'X' option to the list of allowed answers. Add the nested 'if' to check it. Test whether it works. What if you enter 'x' in all menus? What order will you get in that case?

He tested the program, entering 'x' in all three menus and he got what he expected:

```
Erik's Menu
------------
1 chocolate
2 coffee
3 decaf
Choose your item: x
Erik's flavors
-------------
1 caramel
2 vanilla
3 peppermint
4 raspberry
5 plain
Choose your flavor: x
Erik's toppings
-------------
1 chocolate
2 cinnamon
3 caramel
Choose your topping: x
Here is your order:
Main product:
Flavor:
Topping:
Thanks for your order!
```

"An empty order!" he said.

"Right, exactly as it should be," Simon confirmed. "I like your program," he continued. "It works—that’s the first and the most important part. It’s user-friendly, and it gives the user instructions on what to do—that’s the second part. It checks the input and it doesn’t let the user enter wrong values—that’s the third part."

"Let’s quickly recap what you learned today," Simon said. "What was the first thing?"

"First, you broke my program again!" Erik answered. He wasn’t very angry this time because he knew that together with Simon they have fixed the program. "And you told me that I should always think how my users can use program in a bad way."

"Right, dealing with stubborn users who don’t want to follow your instructions is a part of programmer’s job," Simon smiled.

"Then we wrote a menu loop where we checked what the user entered and didn’t allow them to use answers that are not allowed. Then you told me about that `append()` function to add something to a list."

Erik continued, "Then I wrote *nested* if checks and now my code looks like real programs they show in movies."

Simon smiled, "Trust me, what they show in movies are very rarely real programs. But you are right, your program is getting more complex, it uses different Python operators, all these loops, and ifs, and lists."

"And then we added the 'x' option in the menu and now any user can get an empty order!" Erik giggled.

"Yes, why not," Simon said. "You shouldn’t *force* your users to always order something. You should give them an option to cancel their order or to exit from the menu."

Simon continued, "The program is really good now. Tomorrow I’ll ask you stop being a programmer and become the coffee shop manager."

"Am I not a manager already?" Erik asked.

"Yes, you are," Simon smiled. "Now imagine you, the coffee shop manager, just received a new flavor component for your coffee drinks. And you want to add that flavor to the menu. Oh, and also a couple of new toppings. What would you do?"

"I would add those toppings to the list of toppings, not a big deal," Erik answered.

"Yes, but what if you are *just* a manager and not a programmer? You know nothing about this
