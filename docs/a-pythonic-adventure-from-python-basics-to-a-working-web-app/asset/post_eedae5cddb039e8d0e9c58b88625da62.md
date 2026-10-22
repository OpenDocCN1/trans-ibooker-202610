
"Here is the first new thing. See this 'w' letter? It means that we are going to write into the file."

```
f = open("orders.json", "w")
```

"But when we opened the menu files we didn’t use any letters," Erik remembered.

"You are right! Good thing you remembered it," Simon said. "Yes, we didn’t use any letters—they are called modes, by the way—because by default, when I don’t use any letters, Python opens files for reading. This time we want to write to this file so we tell Python about it."

"And I see that you named the file 'orders.json'. Is it because you want to use that JSON format you were talking about?"

"Yes, exactly," Simon answered. "It’s not mandatory, but it’s a convention to add the '.json' extension to JSON files. Another difference is that when we use the 'write' mode, Python will create the file with this name if it doesn’t exist."

"What now?" Erik asked. "How do we write to this JSON file? Last time we used 'methods'; is this what they are called?"

"Yes, you remembered it correctly," Simon said. "But this time we’ll do it differently. It’s all because we are going to write structured data, not just plain text. We are going to use a Python module called json."

"What is a module?" Erik asked immediately.

"I’m going to explain it right now," Simon smiled. "Remember, you wrote several Python functions recently? For example, to read menu items from a file and return a list. Imagine one of your friends wants to write their own program for a coffee shop or something similar."

"Yes," Erik said, "I spoke with Emily recently and she said she wanted to create a program for an ice cream shop."

"Great!" Simon said. "Now you may want to help her and share the functions that you wrote. It will save her some time so her program will be ready earlier. It’s very common among programmers to share their work to help each other. In Python you can group your functions that you want to share in a file and give it to Emily. She can copy that file to her computer and then import it into her program. Because she imported it she can use your functions in her application. Now your file with functions is called a module."

"What if I don’t want to share my functions?" Erik said. "I spent several days writing them!"

"Yes, you did," Simon said. "And you did a great job. But remember, a lot of people spent many days writing other functions in Python, and even Python itself. And they shared their work with other programmers so you can use Python and other functions completely free. That way we help each other to work on our projects. It would be much slower if you and I had to write everything ourselves from scratch. That’s why people use somebody else’s code and also share their code with others. It’s usually called the *Open Source community.*

"Back to JSON," Simon continued. "We are going to use the module called `json`, written by other people. That module can read Python dictionaries and convert them to JSON files. Go to the beginning of your file and add a line: `import json`. It should be the very first line of the file."

Erik did. Here is his updated file.

```
import json

order = {
    "name": "Erik",
    "drink": "coffee",
    "flavor": "caramel",
    "topping": "chocolate"
}

f = open("orders.json", "w")
```

"Now we have to convert your example dictionary to JSON and write it to the file we just opened," Simon said. "In the `json` module, this function is called `dump`. We call it in your program, but we have to tell Python that it should look for this function in the module `json`. So we call it like this: `json.dump()`. You just have to pass two arguments: the dictionary and the file object. Add this function to the end of the file. You dictionary is `order`, your file object is `f`."

Erik added this line to the end of the program:

```
json.dump(order, f)
```

Simon continued: "Now here is another thing that we didn’t do with files before. We should *close* the file. This is important, so let’s draw another diagram."

"Look, here are three main component of a computer: the processor, the memory, and the drive. Your Python program is running on the processor. Your file is stored on the drive in a file system. A file system is what you see in Finder on a Mac, and in Explorer on Windows: folders and files. When you want to work with a file in Python you *open* it, like you just did. That creates a file object in your program. When you write to the file, you write to the computer memory. Then when you want your file to be *really* written to the file system on the drive, you *close it.*"

![](img/f6295b544c84568800930d6ec95963ae_111_0.png)

"This is so complicated!" Erik was confused. "Why don’t we write straight to the drive?"

"Computers are complicated, you are right!" Simon agreed. "The reason for that is that computer engineers try to make computers work faster. Writing to drive is slow—much slower than writing to memory. Imagine you are writing your program in a text editor. If it saved every letter you type immediately to drive it would be very slow. You don’t like working with slow computers, do you?"

"I hate slow computers!" Erik answered emotionally.

"To make computers work faster," Simon continued, "engineers decided to store data in memory and save it to the drive only when necessary. There are a lot of tricks they use to make computers run faster, and, of course, I don’t know all of them. Maybe I’ll learn more about them in college."

"Let’s get back to our program. Remember: file objects use methods. In this case we call f.close(). That will make sure our order is written to the file. Now add it after the last line in your program."

That was easy—after such a long explanation! Erik added it quickly and got this.

```
import json

order = {
    "name": "Erik",
    "drink": "coffee",
    "flavor": "caramel",
    "topping": "chocolate"
}

f = open("orders.json", "w")
json.dump(order, f)
f.close()
```

"Now run it," Simon said.

Erik clicked Run and saw the familiar >>> at the bottom of the window. "Now what?" he asked Simon.

"Nothing happened?" Simon smiled, feeling Erik’s confusion. "Of course, because you didn’t tell Python to print anything. But still, something happened behind the scenes. Python opened a file called orders.json, wrote your dictionary into it, and closed it. Now we have to open it to check if it did it right. Use a plain text editor to open the file. You are on a Mac so it will be TextEdit from your Applications folder. On Windows it’s Notepad, on Linux it’s gedit or Kate. Start the editor and open the file. It’s in your home folder, under mu_code and it’s called orders.json."

Erik started TextEdit, found the file, and opened it. Indeed, he saw his order:

![](img/f6295b544c84568800930d6ec95963ae_112_0.png)

## YOUR TURN  Save your example order in a JSON file
Write the program Erik just wrote. Try to use a slightly different order. Run the program and check the resulting JSON file with a text editor on your platform. Try to change the order and run the program again. Did your JSON file change? (You may have to reload the file in your text editor.)

"You see," Simon said, "it’s your sample order, stored in a file. Let me add something and you will see why JSON files are ideal for storing Python dictionaries."

Simon took the keyboard and changed the `json.dump()` call to this:

```
json.dump(order, f, indent=4)
```

He ran the script again and re-opened the `orders.json` file. Now it looked like this:

![](img/f6295b544c84568800930d6ec95963ae_113_0.png)

## YOUR TURN  Make it beautiful
Add the `indent=4` argument to your previous program and check if your JSON file has changed.

"It looks exactly like my dictionary!" Erik said.

"I told you!" Simon exclaimed. "We’ll be using JSON to store your orders. I said 'orders,' which means now we have to learn how to keep several orders and store them in a file. We know already how Python stores several items that come in order—you used it for you menus.

"A list!" Erik said.

"Correct! A list in Python can contain different things: strings, numbers, even dictionaries. In this case we’ll have a list of dictionaries. Each dictionary will contain an order we’ll add them one by one to the list. A new customer, a new order, a new dictionary in the list. Let me draw a diagram."

![](img/f6295b544c84568800930d6ec95963ae_114_0.png)

"Let’s create a list of orders," Simon continued. "Copy the existing order in the code and call it order1, for example.

Then change the order’s content: the name, the drink, and others."

Erik worked on his code for a while and finally got this additional order, just below the first one.

```
order1 = {
    "name": "Alex",
    "drink": "choco",
    "flavor": "vanilla",
    "topping": "caramel"
}
```

"Good," Simon said, "now create an empty list called orders. Note that it’s plural—'orders'. It’s very similar to creating an empty dictionary—you have done that already. Just instead of curly braces use square brackets."

Erik added the following line below the second order:

```
orders = []
```

"And now we will add both orders to the list," Simon said. "Believe it or not, but the list orders
