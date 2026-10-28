
another `for` loop to run through the list of languages associated with each person:

# ***favorite_languages.py***

```
favorite_languages = {
    'jen': ['python', 'rust'],
    'sarah': ['c'],
    'edward': ['rust', 'go'],
    'phil': ['python', 'haskell'],
}
```

```
for name, languages in favorite_languages.items():
    print(f"\n{name.title()}'s favorite languages are:")
    for language in languages:
        print(f"\t{language.title()}")
```

The value associated with each name in `favorite_languages` is now a list. Note that some people have one favorite language and others have multiple favorites. When we loop through the dictionary, we use the variable name `languages` to hold each value from the dictionary, because we know that each value will be a list. Inside the main dictionary loop, we use another `for` loop to run through each person’s list of favorite languages. Now each person can list as many favorite languages as they like:

```
Jen's favorite languages are:
    Python
    Rust

Sarah's favorite languages are:
    C

Edward's favorite languages are:
    Rust
    Go

Phil's favorite languages are:
    Python
    Haskell
```

To refine this program even further, you could include an `if` statement at the beginning of the dictionary’s `for` loop to see whether each person has more than one favorite language by examining the value of `len(languages)`. If a person has more than one favorite, the output would stay the same. If the person has only one favorite language, you could change the wording to reflect that. For example, you could say, “Sarah’s favorite language is C.”

> **NOTE**
> 
> *You should not nest lists and dictionaries too deeply. If you’re nesting items much deeper than what you see in the preceding examples, or if you’re working with someone else’s code with significant levels of nesting, there’s most likely a simpler way to solve the problem.*

## A Dictionary in a Dictionary

You can nest a dictionary inside another dictionary, but your code can get complicated quickly when you do. For example, if you have several users for a website, each with a unique username, you can use the usernames as the keys in a dictionary. You can then store information about each user by using a dictionary as the value associated with their username. In the following listing, we store three pieces of information about each user: their first name, last name, and location. We’ll access this information by looping through the usernames and the dictionary of information associated with each username:

```
**many_users.py**

```python
users = {
    'aeinstein': {
        'first': 'albert',
```
```

Username: mcurie
    Full name: Marie Curie
    Location: Paris

Notice that the structure of each user’s dictionary is identical. Although not required by Python, this structure makes nested dictionaries easier to work with. If each user’s dictionary had different keys, the code inside the for loop would be more complicated.

# TRY IT YOURSELF

- **6-7. People:** Start with the program you wrote for Exercise 6-1 (page 98). Make two new dictionaries representing different people, and store all three dictionaries in a list called `people`. Loop through your list of people. As you loop through the list, print everything you know about each person.
- **6-8. Pets:** Make several dictionaries, where each dictionary represents a different pet. In each dictionary, include the kind of animal and the owner’s name. Store these dictionaries in a list called `pets`. Next, loop through your list and as you do, print everything you know about each pet.
- **6-9. Favorite Places:** Make a dictionary called `favorite_places`. Think of three names to use as keys in the dictionary, and store one to three favorite places for each person. To make this exercise a bit more interesting, ask some friends to name a few of their favorite places. Loop through the dictionary, and print each person’s name and their favorite places.
- **6-10. Favorite Numbers:** Modify your program from Exercise 6-2 (page 98) so each person can have more than one favorite number. Then print each person’s name along with their favorite numbers.
- **6-11. Cities:** Make a dictionary called `cities`. Use the names of three cities as keys in your dictionary. Create a dictionary of information about each city and include the country that the city is in, its approximate population, and one fact about that city. The keys for each city’s dictionary should be something like `country`, `population`, and `fact`. Print the name of each city and all of the information you have stored about it.
- **6-12. Extensions:** We’re now working with examples that are complex enough that they can be extended in any number of ways. Use one of the example programs from this chapter, and extend it by adding new keys and values, changing the context of the program, or improving the formatting of the output.

# Summary

In this chapter, you learned how to define a dictionary and how to work with the information stored in a dictionary. You learned how to access and modify individual elements in a dictionary, and how to loop through all of the information in a dictionary. You learned to loop through a dictionary’s key-value pairs, its keys, and its values. You also learned how to nest multiple dictionaries in a list, nest lists in a dictionary, and nest a dictionary inside a dictionary.

In the next chapter you’ll learn about `while` loops and how to accept input from people who are using your programs. This will be an exciting chapter, because you’ll learn to make all of your programs interactive: they’ll be able to respond to user input.

When your program needs a list of names, you'll be able to prompt the user for a series of names. To do this, you'll use the `input()` function.

You'll also learn how to keep programs running as long as users want them to, so they can enter as much information as they need to; then, your program can work with that information. You'll use Python's `while` loop to keep programs running as long as certain conditions remain true.

With the ability to work with user input and the ability to control how long your programs run, you'll be able to write fully interactive programs.

# How the input() Function Works

The `input()` function pauses your program and waits for the user to enter some text. Once Python receives the user's input, it assigns that input to a variable to make it convenient for you to work with.

For example, the following program asks the user to enter some text, then displays that message back to the user:

```
**parrot.py**

```python
message = input("Tell me something, and I will repeat it back
to you: ")
print(message)
```
```

The `input()` function takes one argument: the *prompt* that we want to display to the user, so they know what kind of information to enter. In this example, when Python runs the first line, the user sees the prompt `Tell me something, and I will repeat it back to you: `. The program waits while the user enters their response and continues after the user presses ENTER. The response is assigned to the variable

message, then print(message) displays the input back to the user:

```
Tell me something, and I will repeat it back to you: Hello everyone!
Hello everyone!
```

**NOTE**
*Some text editors won’t run programs that prompt the user for input. You can use these editors to write programs that prompt for input, but you’ll need to run these programs from a terminal. See “Running Python Programs from a Terminal” on page 11.*

# Writing Clear Prompts

Each time you use the input() function, you should include a clear, easy-to-follow prompt that tells the user exactly what kind of information you’re looking for. Any statement that tells the user what to enter should work. For example:

greeter.py

```
name = input("Please enter your name: ")
print(f"\nHello, {name}!")
```

Add a space at the end of your prompts (after the colon in the preceding example) to separate the prompt from the user’s response and to make it clear to your user where to enter their text. For example:

```
Please enter your name: Eric
Hello, Eric!
```

Sometimes you’ll want to write a prompt that’s longer than one line. For example, you might want to tell the user
