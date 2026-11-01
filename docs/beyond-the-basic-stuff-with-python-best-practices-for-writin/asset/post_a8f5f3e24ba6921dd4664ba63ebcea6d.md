
Unless you’re writing “throwaway” code that you don’t intend to maintain after you run the program once, you should put some thought into selecting good names in your program. If you simply use a, b, and c for variable names, your future self will expend unnecessary effort to remember what these variables were initially used for.

Names are a subjective choice that you must make. An automated formatting tool, such as Black, described in Chapter 3, can’t decide what you should name your variables. This chapter provides you with some guidelines to help you choose suitable names and avoid poor names. As always, these guidelines aren’t written in stone: use your judgment to decide when to apply them to your code.

## METASYNTACTIC VARIABLES

We commonly use a *metasyntactic variable* in tutorials or code snippets when we need a generic variable name. In Python, we often name variables spam, eggs, bacon, and ham in code examples where the variable name isn’t important. That’s why this book uses these names in code examples; they aren’t meant for you to use them in real-world programs. These names come from Monty Python’s “Spam” sketch (https://en.wikipedia.org/wiki/Spam_(Monty_Python)). The names foo and bar are also common for metasyntactic variables. These are derived from FUBAR, the World War II era US Army slang acronym that indicates a situation is “[messed] up beyond all recognition.”

## Casing Styles

Because Python identifiers are case sensitive and cannot contain whitespace, programmers use several styles for identifiers that include multiple words:

- `snake_case` separates words with an underscore, which looks like a flat snake in between each word. This case often implies that all letters are lowercase, although constants are often written in UPPER_SNAKE_CASE.
- `camelCase` separates words by capitalizing the start of each word after the first. This case often implies the first word begins with a lowercase letter. The uppercase letters look like a camel’s humps.
- `PascalCase`, named for its use in the Pascal programming language, is similar to `camelCase` but capitalizes the first word as well.

Casing is a code formatting issue and we cover it in Chapter 3. The most common styles are snake_case and camelCase. Either is fine to use as long as your project consistently uses one or the other, not both.

## PEP 8’s Naming Conventions

The PEP 8 document introduced in Chapter 3 has some recommendations for Python naming conventions:

- All letters should be ASCII letters—that is, uppercase and lowercase English letters that don’t have accent marks.
- Modules should have short, all lowercase names.
- Class names should be written in PascalCase.
- Constant variables should be written in uppercase SNAKE_CASE.
- Function, method, and variable names should be written in lowercase snake_case.
- The first argument for methods should always be named self in lowercase.
- The first argument for class methods should always be named cls in lowercase.
- Private attributes in classes should always begin with an underscore (_).
- Public attributes in classes should never begin with an underscore (_).

You can bend or break these rules as required. For example, although English is the dominant language in programming, you can use letter characters in any language as identifiers: コンピューター = 'laptop' is syntactically valid Python code. As you can see in this book, my preference for variable names goes against PEP 8, because I use camelCase rather than snake_case. PEP 8 contains a reminder that a programmer doesn’t need to strictly follow PEP 8. The important readability factor isn’t which style you choose but consistency in using that style.

You can read PEP 8’s “Naming Conventions” section online at https://www.python.org/dev/peps/pep-0008/#naming-conventions.

## Appropriate Name Length

Obviously, names shouldn’t be too long or too short. Long variable names are tedious to type, whereas short variable names can be confusing or mysterious. Because code is read more often than it’s written, it’s safer to err on the side of too long variable names. Let’s look at some examples of names that are too short and too long.

### Too Short Names

The most common naming mistake is choosing names that are too short. Short names often make sense to you when you first write them, but their precise meaning can be lost a few days or weeks later. Let’s consider a few types of short names.

- A one- or two-letter name like g probably refers to some other word that begins with g, but there are many such words. Acronyms and names that are only one or two letters long are easy for you to write but difficult for someone else to read. This also applies to . . .
- . . . an *abbreviated name* like *mon*, which could stand for *monitor*, *month*, *monster*, or any number of words.
- A *single-word name* like *start* can be vague: the start of what? Such names could be missing context that isn't readily apparent when read by someone else.

One- or two-letter, abbreviated, or single-word names might be understandable to you, but you always need to keep in mind that other programmers (or even you a few weeks into the future) will have difficulty understanding their meaning.

There are some exceptions where short variable names are fine. For example, it's common to use *i* (for *index*) as a variable name in for loops that loop over a range of numbers or indexes of a list, and *j* and *k* (because they come after *i* in the alphabet) if you have nested loops:

```
>>> for i in range(10):
...     for j in range(3):
...         print(i, j)
...
0 0
0 1
0 2
1 0
--snip--
```

Another exception is using *x* and *y* for Cartesian coordinates. In most other cases, I caution against using single-letter variable names. Although it might be tempting to use, say, *w* and *h* as shorthand for *width* and *height*, or *n* as shorthand for *number*, these meanings might not be apparent to others.

### DON'T DRP LTTRS FRM YR SRC CD

Don't drop letters from your source code. Although dropped letters in names like `memcpy` (memory copy) and `strcmp` (string compare) were popular in the C programming language before the 1990s, they're an unreadable style of naming that you shouldn't use today. If a name isn't easily pronounceable, it isn't easily understood.

Additionally, feel free to use short phrases that can make your code read like plain English. For example, `number_of_trials` is more readable than simply `number_trials`.

### Too Long Names

In general, the larger the name's scope, the more descriptive it should be. A short name like payment is fine for a local variable inside a single, short function. But payment might not be descriptive enough if you use it for a global variable across a 10,000-line program, because such a large program might process multiple kinds of payment data. A more descriptive name, such as salesClientMonthlyPayment or annual_electric_bill_payment, could be more suitable. The additional words in the name provide more context and resolve ambiguity.

It's better to be overly descriptive than not descriptive enough. But there are guidelines for determining when longer names are unnecessary.

## Prefixes in Names

The use of common prefixes in names could indicate unnecessary detail in the name. If a variable is an attribute of a class, the prefix might provide information that doesn't need to be in the variable name. For example, if you have a Cat class with a weight attribute, it's obvious that weight refers to the cat's weight. So the name catWeight would be overly descriptive and unnecessarily long.

Similarly, an old and now obsolete practice is the use of Hungarian notation, the practice of including an abbreviation of the data type in names. For example, the name strName indicates that the variable contains a string value, and iVacationDays indicates that the variable contains an integer. Modern languages and IDEs can relay this data type information to the programmer without the need for these prefixes, making Hungarian notation an unnecessary practice today. If you find you're including the name of a data type in your names, consider removing it.

On the other hand, the is and has prefixes for variables that contain Boolean values, or functions and methods that return Boolean values, make those names more readable. Consider the following use of a variable named is_vehicle and a method named has_key():

```
if item_under_repair.has_key('tires'):
    is_vehicle = True
```

The has_key() method and is_vehicle variable support a plain English reading of the code: “if the item under repair has a key named ‘tires,’ then it’s true that the item is a vehicle.”

Similarly, adding units to your names can provide useful information. A weight variable that stores a floating-point value is ambiguous: is the weight in pounds, kilograms, or tons? This unit information isn't a data type, so including a prefix or suffix of kg or lbs or tons isn't the same as Hungarian notation. If you aren't using a weight-specific data type that contains unit information, naming the variable something like weight_kg could be prudent. Indeed, in 1999 the Mars Climate Orbiter robotic space probe was lost when software supplied by Lockheed Martin produced calculations in imperial standard
