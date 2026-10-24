

## Appendix 1

### Introduction to Dart

Dart is a new language, and you might not be aware of its syntax and some of its features. Here I’ll provide an overview of Dart so that you can have a better grasp on how it works.

Dart, in many ways, resembles C, C++, and Java, so developers with good knowledge of those languages shouldn’t have too many issues understanding the code in the book, but there are also differences that I’ll point out along the way.

This appendix is also a good starting point for people who don’t have much programming experience.

### Comments in Dart

Comments are sections of code that won’t be evaluated by the compiler and won’t be executed. Just like in C, Java, JavaScript, and similar languages, comments are created in two ways:

- Single-line comments are created by adding `//` before the comment.
- Multi-line comments are created by adding `/*` before the comment and `*/` after it.

For example:

```
// this is a comment
int a = 3; // this is also a comment
/* this is also
  a long, long
comment */
```

where `int a = 3;` is regular code (not a comment) and will be evaluated by the compiler and executed.

### Variables and Conditions

Variables can be declared in two ways in Dart:

- In the *traditional* C-like way, by prefixing the variable name with the data type:

  ```
  int a = 3;
  ```

- By prefixing the variable name with `var`:

  ```
  var a = 3;
  ```

A list of some of the most common data types is provided at the end of this appendix.

### Characters and Strings

In some languages there are `char` types for single characters and arrays of chars or separate types for strings. In Dart, the `String` type is the only character/string type, and it can contain one or more characters. For that reason, unlike in other languages, you can use single or double quotes to enclose what you assign to the string (called a *string literal*):

```
String greeting = "Hi!";
```

will work just as well as:

```
var greeting = 'Hi!';
```

Like in many other modern languages, two strings can be concatenated using the same `+` operator you use for addition between numbers:

```
var firstGreeting = "Hi" + "!";
assert(firstGreeting == 'Hi!');
```

What’s `assert`? It’s something you will get used to seeing reading this appendix: its effect in compiled code is that, in a testing/debugging situation (it does nothing on production code), it terminates the execution if the condition in its parentheses isn’t true, but it’s widely used in programming language tutorials and documentation to explain language features. Whenever you see one in this appendix, you should expect the condition inside its parentheses to always be true.

### The Backslash

Just like in many other languages, the backslash can be used to escape some characters or insert special characters.

For example, if you wanted to have quotes inside a string, you have two ways to do it:

- You use different kind of quotes to enclose the string (for example `'He said: "Hi!"'`).
- You escape the quotes (for example, `"He said: \"Hi!\""`).

For the special characters part, this will be familiar especially to C developers; the `\n` character can be used to go to a new line:

```
Text("First line\nSecond line")
```

will render *Second line* below *First line*.

Obviously, the backslash itself has to be escaped to be used as part of a string:

```
Text("\\ is the backslash character")
```

will display `\ is the backslash character`.

### String Interpolation

String interpolation allows you to insert one or more variables’ value into a string. Let’s first explore how things would be done *without* string interpolation.

For example, if you wanted to display the value of an `int` with a `Text`, you can’t just write:

```
int number;

// (...)

Text(number)
```

because `Text` expects a `String`. This means you need to change that to:

```
Text(number.toString())
```

What if you want to display a string and the number?
Then you would need to write something like this, using string concatenation:

```
"You tapped the button " + number.toString() + " times"
```

What string interpolation allows you to do is write this instead:

```
"You tapped the button $number times"
```

By prefixing a variable’s name with `$` you can insert its value into the string. If you need to access an element of a `List`, the member of a class, or write any kind of expression, you need to enclose the statement in curly braces. If you had a list like the following:

```
List numbers = [1, 2, 3];
```

you would use string interpolation like this:

```
"There are ${numbers.length} and second element is number ${numbers[1]}"
```

### Multi-Line Strings and Raw Strings

Like in many other languages, you can make multi-line strings by using triple quotes and you can make raw strings by prefixing the opening quotes with the letter “r”.

A multi-line string makes it so that you can turn this:

```
"First line\nSecond line";
```

or this:

```
"First line\n" +
```

### Lists

A `List` is what is commonly referred to as an array: an ordered collection of data, the elements of which are indexed with the values from `0` to `length-1`, where the `length` of a `List` is the number of elements it contains. A `List` can be initialized with a list literal, which is a comma-separated list of values enclosed in square brackets:

```
var list = [30, 20, 500];
assert(list[0] == 30);
assert(list[1] == 20);
assert(list[2] == 500);
```

### If and For in List Literals

List literals in Dart can do more than that, though—you can have `if` statements in them:

```
bool condition;
// ...
var list = [
    if(condition) "First"
    else "First, alternative",
    "Second"
];
assert(list[1] == "Second");
if(condition)
    assert(list[0] == "First");
else
    assert(list[0] == "First, alternative");
/*
 * As in other languages, these last two assert() calls could become one:
 * assert((condition && list[0] == "First") ||
 *        (!condition && list[0] == "First, alternative"))
 */
```

### Adding and Removing Items from Lists

An item can be added to the end of a list by using `List.add`:

```
var l = [1, 2, 3];
l.add(4);
assert(l[3] == 4);
```

An item can be removed in two main ways (there are others, feel free to check out all `List` methods at the official Dart language reference [55]). One of them is by passing the value you want to remove with `List.remove` (which will only remove the first occurrence):

```
var l = ["Test", "test", "Test"];

l.remove("Test");

assert(l[0] == "test");
assert(l[1] == "Test"); // The second occurrence won't be removed
```

The other is by passing the index at which you want to remove an element to `List.removeAt`:

```
var l = [5, 6, 7];

l.removeAt(1);

assert(l[1] == 7);
```

### Generating Lists

One of the ways to create lists is by using the `List.generate` constructor, which takes two arguments: the number of elements the list will have and a callback function (a function to be run by another function in certain conditions and with certain arguments) that generates each element based on its index.

For example, this is how you’d create a list from the concatenation of the strings contained in two lists:

```
var l1 = ["First", "Second", "Third"];
var l2 = ["1", "2", "3"];

var l = List.generate(3, (index) => l1[index] + l2[index]);
```

If this example wasn’t clear to you, check out the section about functions (Functions) and the one about classes (Classes).

### Iterating Over Lists: for, map, and forEach

We’ve found out how to generate lists, now what about how to iterate over the elements of a list? There are multiple ways to do that:

- The `for in` loop.
- The `.forEach` method.
- The `.map` method.

### The for in Loop

Present in most modern languages, it’s a special kind of loop: `while`, `do while`, and `for` that iterates over the elements of a list:

```
var listName = [
  "First",
  "Second",
  "Third",
  "Fourth"
];

for(var elName in listName) {
  print(elName);
}
```

### map and forEach

Particularly loved by some for their functional style, the `Iterable`'s `map` and `forEach` methods are alternatives to the `for in` loop.

`forEach` is the most similar to the `for` loop, as it is used to execute an operation on (or for) each of the elements of a list. The callback closure you use is of type `void`, so you can’t return anything. Here’s an example, showing how to print each element of a list:

```
listName.forEach((el) {
  print(el);
});
```

`map` is different: it is used to create an `Iterable` from calling a function on each element of the `List`, here’s an example:

```
var numbers = [
  1,
  2,
  3,
  4,
  5,
  6
];

var iter = numbers.map((el) => "#$el");
```

`iter` is an `Iterable`, not a proper `List`. If you want to access the elements directly just like you do with a list, you need to use the `toList` method:

