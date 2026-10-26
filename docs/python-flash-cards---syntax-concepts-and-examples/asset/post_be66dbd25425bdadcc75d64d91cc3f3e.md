
## DATA STRUCTURES AND TYPES

- What are data structures?
- What are data types?
- Why are data structures important?
- What are the most common kinds of data structures?

**Data structures** define how you store data in a programming language. Every programming language has a core set of data structures. *Data types* refer to the data structures you’re using or the kind of information stored in those data structures.

Much of programming focuses on handling data. How you represent data through code impacts what you can do with that data: if you model your data well, your program is easier to work with.

Choosing a specific approach to data modeling informs how you think about data, so knowing the available data structures in a programming language will help you best represent the information in your project.

The most common data structures include variables, sequences, mappings, classes, and objects. Python also has text-based data types, such as strings, and numerical data types, such as integers and floats.

## VARIABLES

- What is a variable?
- Why are variables important?
- What is a statically typed language?
- What is a dynamically typed language?
- What makes Python a dynamic language?

**In Python, a _variable_ is a name attached to a piece of data. You first define a variable, then use that name when you refer to that piece of data.**

In _statically typed languages_, you must declare the kind of data a variable represents when you define it. Python is a _dynamically typed language_, meaning you don’t have to declare the kind of data a variable will represent. In Python, the interpreter examines the data associated with the variable throughout the life of the program and manages type issues for you.

Dynamic languages prioritize a programmer’s time over efficient use of the processor, but they can still be fast and efficient when used properly. Static languages prioritize processor efficiency over a programmer’s time. Programs are longer and more verbose in static languages, but can be highly optimized.

A string is a value made up of text. A classic example of a string is the code "Hello, world!"

A string is one of the simplest data types in any language. Much of the information that’s passed within and between programs is strings.

Strings are treated as a collection of characters, so the string "123" is different from the numerical value 123. A substring is part of a string.

You can perform many actions with strings:

- Joining strings together, known as concatenation
- Inserting the value of a variable into a string, known as interpolation
- Changing a string’s case
- Stripping extra whitespace from a string
- Searching for a substring within a string
- Replacing some characters in a string

In Python, strings are enclosed using single or double quotes.

## NUMERICAL DATA TYPES

- What is an int?
- What is a float?
- What can you do with numerical data?
- What other kinds of numerical data types are available?

## SEQUENCES

- What is a sequence?
- Why are sequences important?
- What kinds of things can you do with a sequence?
- What kind of sequences are available in Python?
- What is a mutable sequence?

A sequence is a data structure that stores a collection of items in a specific order. In some languages, a sequence can only contain items of one type; in other languages, including Python, a sequence can have items of different types.

Sequences are important when you need to store items in a specific order. For example, a list of website users might be ordered according to when each user registered. With a sequence, you can do the following:

- Add and remove items
- Work with individual items or groups of items
- Determine whether a value is in the sequence
- Look for duplicate or unique items
- Loop through the sequence and do something with each item
- Work with items that match certain conditions.

In Python, the main sequence type is the list. Lists are mutable, meaning you can modify them after creating them. Tuples are immutable sequences, meaning they can’t be changed after they’re defined. Strings are a special type of sequence: each element in the sequence is a character.

## MAPPINGS

- What is a mapping?
- Why are mappings important?
- What can you do with a mapping?
- What is the main mapping type in Python?

A mapping is a data structure that connects two or more pieces of information, commonly called a key-value pair. When you ask for a key, you get its associated value. One mapping structure can store many key-value pairs.

Mappings don’t just store data. They allow you to establish relationships between the data. For example, you could pair each username with a password, each country with its capital, and each book with its author.

Mappings are not limited to single pieces of data: you could use a single piece of data as a key and use a sequence as the value. For example, you could pair a username with a sequence of other users to define a relationship between one user and other users. This is the core of how many social networks are implemented.

In Python, the dictionary is the main mapping type. Make sure you understand dictionaries, because many advanced Python data structures are represented by dictionaries.

## CLASSES

- What is a class?
- What are attributes and methods?
- What is an instance?
- What can you do with a class?

A class is a structure that allows you to combine the data and functionality associated with a particular task. The variables of a class are attributes. The functions of a class are methods.

Classes can represent real-world objects or abstract ideas. After defining a class, you use it by making an instance, or object, of the class. You can make as many instances as you want from one class.

As an example, you might use a class to represent a website user. The class would have attributes associated with the user’s username, password, registration date, and more. Methods would define the actions the user could take, such as registering, authenticating, logging in, and logging out. You could then make one instance for each user who registers on the site.

Many external libraries are written as classes, so learning to work with classes makes it easier to work with many existing projects.

## INHERITANCE

- What is inheritance?
- What is a parent class?
- What is a child class?

*Inheritance* is the idea of basing one class on another class so the new class can use the existing methods and attributes. When your classes are modeled on similar things, use inheritance rather than starting a new class.

Imagine you want to model a book, so you write a class called Book. The class attributes might be author, title, and subject, and methods might be describe_book(), show_sample_page(), and review_book(). An ebook is a kind of book, so you could make EBook inherit from Book so it has all the attributes and methods of Book. Then you only need to define any additional attributes and methods specific to ebooks, such as a file_size attribute and a download_book() method.

In this example, Book is a parent class and EBook is a child class. A child class inherits from the parent class. When you write a child class, you can add any attributes or methods not in the parent class, as well as customize the behaviors inherited from the parent class.

A Boolean value represents either true or false. In Python, these values are written as True and False.

Boolean values can represent the state of a program or a certain condition. For example, you can use variables such as game_active, can_edit, and polling_open, which take either a True or False value. When these values are True, certain code sections are enabled as the program runs.

A null value represents nothing. You would assign a null value to a variable that doesn’t have a defined value. In Python, this is written as None. In logical comparisons, None evaluates to False.

The null value is useful in many situations. For example, you can use it to make some parameters for a function optional. If a value is provided, the parameter is assigned the given value. If no value is provided, the parameter doesn’t have any effect on the program. A function can also return None when it can’t do its work, instead of generating an error that might cause the program to crash.
