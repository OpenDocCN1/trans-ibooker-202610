"You see now?" Simon said. "You forgot to indent the lines to the right. That’s why it says that it expects an indented block here. Move all the lines that are part of the loop to the right."

Erik changed his function and checked the code again.

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
        if choice == '1' or choice == '2' or choice == '3':
            answer = choices[int(choice) - 1]
            break
        else:
            print("Enter number 1, 2, or 3!")
            answer = ''

    return answer
```

YOUR TURN   Add the while-loop to your function

Add the loop like Erik just did. Try to use the Check button to find if there are errors in your code. Try to make errors and see if the editor can find them.

Now the Check button showed a green thumbs-up for a moment so Erik knew it was okay. He clicked Run to test the program. At the first menu he entered '42' like Simon did last time. The program reported the he should choose a number 1, 2, or 3 and returned to the prompt again! It didn’t crash, it worked!

"What was the other wrong thing Simon did with my program?" Erik tried to remember. "Right, he tried to enter 'coffee'! Let’s try that too."

He entered 'coffee' and his program responded as he expected again! Finally Erik typed '2' and got the next menu. Here is what he saw in the editor window (we marked Erik’s input with bold):

```
Erik's Menu
------------
1 chocolate
2 coffee
3 decaf
Choose your item: 42
Enter number 1, 2, or 3!
Choose your item: coffee
Enter number 1, 2, or 3!
Choose your item: 2
Erik's flavors
------------
```

At the next menu he typed 4 for Raspberry and got the same message: