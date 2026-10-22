"Now," Simon said, "create a sample order."

"What is a sample order?"

"Your order is a dictionary, right?" Simon started to explain. "In your main program you created an empty dictionary and then started to fill it with the values you were getting from the customer. Here we want to skip that step and imagine that our order dictionary is already filled with the customer’s choices. Let me start it for you," Simon said and wrote in Erik’s editor:

```
order = {
    "name": "Erik",
```

"You can continue now," he said. "Don’t forget to close the curly braces."

Erik finished the order dictionary and closed the curly braces. Now it looked like this:

```
order = {
    "name": "Erik",
    "drink": "coffee",
    "flavor": "caramel",
    "topping": "chocolate"
}
```

"I noticed that you indented the lines in this dictionary," he asked his brother. "Is it also a rule for dictionaries in Python?"

"No," Simon answered, "in this case I did it just to make it look better. And to be more readable. I could put all items together in one line, or start from the beginning of the line, but I think this way it looks better."

"Now," he continued, "we have a dictionary. And we want to save it in a file. I guess I should remind you about files operations with dictionaries."

"Yes," Erik said, "it was so-o-o long ago, I don’t remember much."

"Sure," Simon said. "Also you’ll learn a couple of new things about files. First, we have to open a file. To open a file we should call a function named open() --of course!--and pass the file name as an argument. You know everything about functions and their arguments, right? That function returns a file handle. It’s a special object that our program can now use to work with this file."

Simon wrote one more line below the dictionary that Erik created.

```
order = {
    "name": "Erik",
    "drink": "coffee",
    "flavor": "caramel",
    "topping": "chocolate"
}

f = open("orders.json", "w")
```