# # Storing Data

Many of your programs will ask users to input certain kinds of information. You might allow users to store preferences in a game or provide data for a visualization. Whatever the focus of your program is, you’ll store the information users provide in data structures such as lists and dictionaries. When users close a program, you’ll almost always want to save the information they entered. A simple way to do this involves storing your data using the `json` module.

The `json` module allows you to convert simple Python data structures into JSON-formatted strings, and then load the data from that file the next time the program runs. You can also use `json` to share data between different Python programs. Even better, the JSON data format is not specific to Python, so you can share data you store in the JSON format with people who work in many other programming languages. It’s a useful and portable format, and it’s easy to learn.

> **NOTE**
> 
> *The* JSON (JavaScript Object Notation) *format was originally developed for JavaScript. However, it has since become a common format used by many languages, including Python.*

# ## *Using `json.dumps()` and `json.loads()`*

Let’s write a short program that stores a set of numbers and another program that reads these numbers back into memory. The first program will use `json.dumps()` to store the set of numbers, and the second program will use `json.loads()`.