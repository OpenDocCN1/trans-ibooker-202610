![](assets/cover.png)

![](assets/cover.png)

# Learning Go

by Jon  Bodner

Copyright © 2026 Jon Bodner. All rights reserved.

Printed in the United States of America.

Published by O’Reilly Media, Inc., 1005 Gravenstein Highway North, Sebastopol, CA 95472.

O’Reilly books may be purchased for educational, business, or sales promotional use. Online editions are also available for most titles ([http://oreilly.com](http://oreilly.com)). For more information, contact our corporate/institutional sales department: 800-998-9938 or *corporate@oreilly.com*.

*   Acquisitions Editor: Suzanne McQuade
*   Developmental Editor: Sarah Grey
*   Production Editor: Destiny Baitinger
*   Interior Designer: David Futato
*   Cover Designer: Karen Montgomery

*   March 2021: First Edition
*   January 2024: Second Edition
*   April 2027: Third Edition

Add additional revdate spans below as needed. 

# Revision History for the Early Realease

*   2026-05-30: First Release

See [http://oreilly.com/catalog/errata.csp?isbn=0642572348533](http://oreilly.com/catalog/errata.csp?isbn=0642572348533) for release details.

The O’Reilly logo is a registered trademark of O’Reilly Media, Inc. *Learning Go*, the cover image, and related trade dress are trademarks of O’Reilly Media, Inc.

The views expressed in this work are those of the author, and do not represent the publisher’s views. While the publisher and the author have used good faith efforts to ensure that the information and instructions contained in this work are accurate, the publisher and the author disclaim all responsibility for errors or omissions, including without limitation responsibility for damages resulting from the use of or reliance on this work. Use of the information and instructions contained in this work is at your own risk. If any code samples or other technology this work contains or describes is subject to open source licenses or the intellectual property rights of others, it is your responsibility to ensure that your use thereof complies with such licenses and/or rights. PROD: Uncomment the following sentence if appropriate and add it to the above para: This book is not intended as \[legal/medical/financial; use the appropriate reference\] advice. Please consult a qualified professional if you require \[legal/medical/financial\] advice.

9798341674592

<p class="printer">\[LSI\]</p> 

# Brief Table of Contents (*Not Yet Final*)

Preface (available)

*Chapter 1: Setting Up Your Go Environment* (uavailable)

Chapter 2: Predeclared Types and Declarations (available)

*Chapter 3: Composite Types* (unavailable)

*Chapter 4: Blocks, Shadows, and Control Structures* (unavailable)

*Chapter 5: Functions* (unavailable)

*Chapter 6: Pointers* (unavailable)

*Chapter 7: Types, Methods, and Interfaces* (unavailable)

*Chapter 8: Generics* (unavailable)

*Chapter 9: Errors* (unavailable)

*Chapter 10: Modules, Packages, and Imports* (unavailable)

*Chapter 11: Go Tooling* (unavailable)

*Chapter 12: Concurrency in Go* (unavailable)

*Chapter 13: The Standard Library* (unavailable)

*Chapter 14: The Context* (unavailable)

Chapter 15: Writing Tests (available)

*Chapter 16: Monitoring, Profiling, Optimizing, and Tracing* (unavailable)

*Chapter 17: Here Be Dragons: Reflect, Unsafe, and Cgo* (unavailable)

 italicize any chapter titles that are not yet available in ER  

# Preface

I wasn’t sure there should be a third edition of Learning Go.

When the term “vibe coding” was coined in early 2025 by Andrej Karpathy, it was a bit of a joke. By the end of 2025, no one was laughing. LLMs got *really* good at writing code *really* fast. Developers were using them for larger and more complicated projects. If anything, the momentum behind automating coding is increasing.

Given this, why would anyone want to write (or read) an updated book on a programming language? Who cares about idiomatic usage and new features when most code is automatically generated? Maybe my time would be better spent writing romance novels before LLMs master those, too.

After some additional thinking, I realized that a book on idiomatic Go might be more needed now than ever before. There are two reasons. The first is that idiomatic code works better. When you write code in an idiomatic way, the Go runtime is able to execute your code more efficiently. If you try to bend Go into something like Java, C++, or Python you will likely find your programs running more slowly and using more memory than if you just adopted the style and techniques that the Go runtime is optimized to use. When a programmer who is familiar with one language asks an LLM to generate code in another language, they need to understand what a Go program looks like to know if it is going to run well.

And that leads to the other reason, which gets to the heart of what “idiomatic” means and why programs should be written in an idiomatic style. Harold Abelson, Gerald Jay Sussman, and Julie Sussman’s book *Structure and Interpretation of Computer Programs* was the introductory computer science textbook at MIT for decades. It includes the famous quote, “Programs must be written for people to read, and only incidentally for machines to execute.” This was true in 1984 and it is still true over 40 years later, even if programs are writing programs.

An idiom is just a word for a familiar pattern. When a program follows a familiar pattern, that is a shortcut to understanding what it is doing and why it is doing it. No matter how code was produced, at some point a person is going to need to look at it and understand what that code does. If it follows a familiar pattern, it will be easier. If it does not, it will be harder, which makes the inevitable maintenance more difficult. In short, non-idiomatic code is likely to be buggy, unmaintainable code, and no one wants that.

This insight helped me figure out what sort of updates should be in the third edition. In addition to covering changes to the Go language, runtime, and ecosystem, this edition includes tips on what to look for when reviewing code, as well as additional exercises to help you get more comfortable writing idiomatic Go code and knowing what it looks like. In addition, there’s a new chapter on monitoring and measuring your Go code so you can better understand how your Go program is working, both internally and in collaboration with other software.

Welcome to the latest edition of Learning Go. I hope you learn best practices for writing and reviewing Go code. And if you turn off the LLM to experience the joy of writing code all by yourself, I won’t tell anyone.

# Who Should Read This Book

This book is targeted at developers who are looking to pick up a second (or fifth) language. The focus is on people who are new to Go. This ranges from those who don’t know anything about Go other than that it has a cute mascot to those who have already worked through a Go tutorial or even written some Go code. The focus of *Learning Go* isn’t just how to write programs in Go; it’s how to write Go *idiomatically*. More experienced Go developers can find advice on how to best use the newer features of the language. The most important thing is that you’re here to learn how to write Go code that looks like Go.

I assume you have some experience with the tools of the developer trade, such as version control (preferably Git) and IDEs. You should be familiar with basic computer science concepts like concurrency and abstraction, as the book explains how they work in Go. You can download most of the code examples from GitHub, and try out dozens more online on [The Go Playground](https://go.dev/play/). While an internet connection isn’t required, it is helpful when reviewing executable examples. Since Go is often used to build and call HTTP servers, some examples also assume familiarity with basic HTTP concepts.

While most of Go’s features are found in other languages, Go makes different tradeoffs, so programs written in it have a different structure. *Learning Go* starts by looking at how to set up a Go development environment, and then covers variables, types, control structures, and functions. If you are tempted to skip over this material, resist the urge and take a look. The details are often what make your Go code idiomatic. Some of what seems obvious at first glance might actually be subtly surprising when you think about it in depth.

# Conventions Used in This Book

The following typographical conventions are used in this book:

*Italic*
: 

Indicates new terms, URLs, email addresses, filenames, and file extensions.

`Constant width`
: 

Used for program listings, as well as within paragraphs to refer to program elements such as variable or function names, databases, data types, environment variables, statements, and keywords.

**`Constant width bold`**
: 

Shows commands or other text that should be typed literally by the user.

*`Constant width italic`*
: 

Shows text that should be replaced with user-supplied values or by values determined by context.

###### Tip

This element signifies a tip or suggestion.

###### Note

This element signifies a general note.

###### Warning

This element indicates a warning or caution.

# O’Reilly Online Learning

###### Note

For more than 40 years, [*O’Reilly Media*](http://oreilly.com) has provided technology and business training, knowledge, and insight to help companies succeed.

Our unique network of experts and innovators share their knowledge and expertise through books, articles, and our online learning platform. O’Reilly’s online learning platform gives you on-demand access to live training courses, in-depth learning paths, interactive coding environments, and a vast collection of text and video from O’Reilly and 200+ other publishers. For more information, visit [*http://oreilly.com*](http://oreilly.com).

# How to Contact Us

Please address comments and questions concerning this book to the publisher:

*   O’Reilly Media, Inc.
*   1005 Gravenstein Highway North
*   Sebastopol, CA 95472
*   800-889-8969 (in the United States or Canada)
*   707-827-7019 (international or local)
*   707-829-0104 (fax)
*   [*support@oreilly.com*](mailto:support@oreilly.com)
*   [*https://www.oreilly.com/about/contact.html*](https://www.oreilly.com/about/contact.html)

Don't forget to update the link above. 

For news and information about our books and courses, visit [*https://oreilly.com*](https://oreilly.com).

Find us on LinkedIn: [*https://linkedin.com/company/oreilly-media*](https://linkedin.com/company/oreilly-media)

Follow us on Twitter: [*https://twitter.com/oreillymedia*](https://twitter.com/oreillymedia)

Watch us on YouTube: [*https://youtube.com/oreillymedia*](https://youtube.com/oreillymedia)

# Acknowledgments for the 3rd Edition

Acknowledgments for third edition are forthcoming.

# Acknowledgments for the 2nd Edition

I’ve been humbled and flattered by the response to the first edition of *Learning Go*. It was started in late 2019, completed in late 2020, and released in early 2021. It was, unintentionally, my own pandemic project. Some people baked bread; I explained pointers.

After I finished, people asked me what my next book would be. I told them that I was going to take a well-deserved break and then get to work on a romance novel. But before I could start writing about pirates and princesses, something amazing happened: *Learning Go* was a huge success. It spent the better part of a year as one of the five most popular books on O’Reilly Learning. As sales continued, I saw things in the first edition that I wanted to fix and received letters from readers pointing out errors and omissions. Some were fixed as errata in later printings, but I contacted O’Reilly to see if they would like a new edition, and they were interested.

Returning to *Learning Go* and O’Reilly for the second edition was a pleasant experience. Rita Fernando provided guidance, feedback, and editing. This edition has been made immeasurably better by feedback from Jonathan Amsterdam, Leam Hall, Katie Hockman, Thomas Hunter, Max Horstmann, and Natalie Pistunovich. Chris Hines did an incredibly thorough job finding my mistakes and suggesting better examples. The comments and feedback from Abby Deng and the members of the Go Book Club at Datadog allowed me to makes changes based on feedback from developers new to Go. The (hopefully few) remaining errors are all mine.

My wife and children were kind enough to not mind me skipping out on family movie nights while I figured out the best way to describe new Go features.

I also want to thank the readers of the first edition who reached out to me with their kind words. I thank you all for your support and encouragement.

# Acknowledgments for the 1st Edition

Writing a book seems like a solitary task, but it doesn’t happen without the help of a great number of people. I mentioned to Carmen Andoh that I wanted to write a book on Go and at GopherCon 2019, and she introduced me to Zan McQuade at O’Reilly. Zan guided me through the acquisition process and continued to provide me advice while I was writing *Learning Go*. Michele Cronin edited the text, gave feedback, and listened during the inevitable rough patches. Tonya Trybula’s copyediting and Beth Kelly’s production editing made my draft production-quality.

While writing, I received critical feedback (and encouragement) from many people including Jonathan Altman, Jonathan Amsterdam, Johnny Ray Austin, Chris Fauerbach, Chris Hines, Bill Kennedy, Tony Nelson, Phil Pearl, Liz Rice, Aaron Schlesinger, Chris Stout, Kapil Thangavelu, Claire Trivisonno, Volker Uhrig, Jeff Wendling, and Kris Zaragoza. I’d especially like to recognize Rob Liebowitz, whose detailed notes and rapid responses made this book far better than it would have been without his efforts.

My family put up with me spending nights and weekends at the computer instead of with them. In particular, my wife, Laura, graciously pretended that I didn’t wake her up when I’d come to bed at 1 A.M. or later.

Finally, I want to remember the two people who started me on this path four decades ago. The first is Paul Goldstein, the father of a childhood friend. In 1982, Paul showed us a Commodore PET, typed PRINT 2 + 2, and hit the Enter key. I was amazed when the screen said 4 and was instantly hooked. He later taught me how to program and even let me borrow the PET for a few weeks. Second, I’d like to thank my mother for encouraging my interest in programming and computers, despite having no idea what any of it was for. She bought me the BASIC programming cartridge for the Atari 2600, a VIC-20, and then a Commodore 64, along with the programming books that inspired me to want to write my own someday.

Thank you all for helping make this dream of mine come true.

Fill in... 

# Chapter 1. Predeclared Types and Declarations

Now that you have your development environment set up, it’s time to start looking at Go’s language features and how to best use them. When trying to figure out what “best” means, there is one overriding principle: write your programs in a way that makes your intentions clear. As I go through features and discuss the options, I’ll explain why I find that a particular approach produces clearer code.

I’ll start by looking at the types that are built into Go and how to declare variables of those types. While every programmer has experience with these concepts, Go does some things differently, and subtle differences exist between Go and other languages.

# The Predeclared Types

Go has many types built into the language. These are called *predeclared* types. They are similar to types that are found in other languages: booleans, integers, floats, and strings. Using these types idiomatically is sometimes a challenge for developers who are transitioning from another language. You’ll look at these types and see how they work best in Go. Before I review the types, let’s cover some of the concepts that apply to all types.

## The Zero Value

Go, like most modern languages, assigns a default *zero value* to any variable that is declared but not assigned a value. Having an explicit zero value makes code clearer and removes a source of bugs found in C and C++ programs. As I talk about each type, I will also cover the zero value for the type. You can find details on the zero value in [The Go Programming Language Specification](https://oreil.ly/3d3e6).

## Literals

A Go *literal* is an explicitly specified number, character, or string. Go programs have four common kinds of literals. (I’ll cover a rare fifth kind of literal when discussing complex numbers.)

An *integer literal* is a sequence of numbers. Integer literals are base 10 by default, but different prefixes are used to indicate other bases: 0b for binary (base 2), 0o for octal (base 8), or 0x for hexadecimal (base 16). You can use either upper- or lowercase letters for the prefix. A leading 0 with no letter after it is another way to represent an octal literal. Do not use it, as it is very confusing.

To make it easier to read longer integer literals, Go allows you to put underscores in the middle of your literal. This allows you to, for example, group by thousands in base 10 (1\_234). These underscores have no effect on the value of the number. The only limitations on underscores are that they can’t be at the beginning or end of numbers, and you can’t have them next to each other. You could put an underscore between every digit in your literal (1\_2\_3\_4), but don’t. Use them to improve readability by breaking up base 10 numbers at the thousands place or to break up binary, octal, or hexadecimal numbers at 1-, 2-, or 4-byte boundaries.

A *floating-point literal* has a decimal point to indicate the fractional portion of the value. They can also have an exponent specified with the letter `e` and a positive or negative number (such as 6.03e23). You also have the option to write them in hexadecimal by using the 0x prefix and the letter `p` for indicating any exponent (0x12.34p5, which is equal to 582.5 in base 10). As integer literals, you can use underscores to format your floating-point literals.

A *rune literal* represents a character and is surrounded by single quotes. Unlike many other languages, in Go single quotes and double quotes are *not* interchangeable. Rune literals can be written as single Unicode characters (`'a'`), 8-bit octal numbers (`'\141'`), 8-bit hexadecimal numbers (`'\x61'`), 16-bit hexadecimal numbers (`'\u0061'`), or 32-bit Unicode numbers (`'\U00000061'`). There are also several backslash-escaped rune literals, with the most useful ones being newline (`'\n'`), tab (`'\t'`), single quote (`'\''`), and backslash (`'\\'`).

Practically speaking, use base 10 to represent your integer and floating-point literals. Octal representations are rare, mostly used to represent POSIX permission flag values (such as 0o777 for rwxrwxrwx). Hexadecimal and binary are sometimes used for bit filters or networking and infrastructure applications. Avoid using any of the numeric escapes for rune literals, unless the context makes your code clearer.

There are two ways to indicate *string literals*. Most of the time, you should use double quotes to create an *interpreted string literal* (e.g., type **`"Greetings and`** **`Salutations"`**). These contain zero or more rune literals. They are called “interpreted” because they interpret rune literals (both numeric and backslash escaped) into single characters.

###### Note

One rune literal backslash escape is not legal in a string literal: the single quote escape. It is replaced by a backslash escape for double quotes.

The only characters that cannot appear in an interpreted string literal are unescaped backslashes, unescaped newlines, and unescaped double quotes. If you use an interpreted string literal and want your greetings on a different line from your salutations and want “Salutations” to appear in quotes, you need to type **`"Greetings and\n\"Salutations\""`**.

If you need to include backslashes, double quotes, or newlines in your string, using a *raw string literal* is easier. These are delimited with backquotes (`` ` ``) and can contain any character except a backquote. There’s no escape character in a raw string literal; all characters are included as is. When using a raw string literal, you write a multiline greeting like so:

```
`Greetings and
"Salutations"`
```

Literals are considered *untyped*. I’ll explore this concept more in [“Literals Are Untyped”](#ch01.html#literal_untyped). As you will see in [“var Versus :=”](#ch01.html#var_declarations), there are situations in Go where the type isn’t explicitly declared. In those cases, Go uses the *default type* for a literal; if there’s nothing in the expression that makes clear what the type of the literal is, the literal defaults to a type. I will mention the default type for literals when discussing the different predeclared types.

## Booleans

The `bool` type represents Boolean variables. Variables of `bool` type can have one of two values: `true` or `false`. The zero value for a `bool` is `false`:

```
var flag bool // no value assigned, set to false
var isAwesome = true
```

It’s hard to talk about variable types without showing a variable declaration, and vice versa. I’ll use variable declarations first and describe them in [“var Versus :=”](#ch01.html#var_declarations).

## Numeric Types

Go has a large number of numeric types: 12 types (and a few special names) that are grouped into three categories. If you are coming from a language like JavaScript that gets along with only a single numeric type, this might seem like a lot. And in fact, some types are used frequently while others are more esoteric. I’ll start by looking at integer types before moving on to floating-point types and the very unusual complex type.

### Integer types

Go provides both signed and unsigned integers in a variety of sizes, from one to eight bytes. They are shown in [Table 1-1](#ch01.html#table2_1).

Table 1-1. The integer types in Go

  
| Type name || Value range |
| --- | --- |
| `int` || platform dependent |
| --- | --- |
| `int8` || –128 to 127 |
| `int16` || –32768 to 32767 |
| `int32` || –2147483648 to 2147483647 |
| `int64` || –9223372036854775808 to 9223372036854775807 |
| `uint` || platform dependent |
| `uint8` || 0 to 255 |
| `uint16` || 0 to 65535 |
| `uint32` || 0 to 4294967295 |
| `uint64` || 0 to 18446744073709551615 |

It might be obvious from the name, but the zero value for all of the integer types is `0`.

### The special integer types

Go does have some special names for integer types. A `byte` is an alias for `uint8`; it is legal to assign, compare, or perform mathematical operations between a `byte` and a `uint8`. However, you rarely see `uint8` used in Go code; just call it a `byte`.

The second special name is `int`. On a 32-bit CPU, `int` is a 32-bit signed integer like an `int32`. On 64-bit CPUs, `int` is a 64-bit signed integer, just like an `int64`. Because `int` isn’t consistent from platform to platform, it is a compile-time error to assign, compare, or perform mathematical operations between an `int` and an `int32` or `int64` without a type conversion (see [“Explicit Type Conversion”](#ch01.html#type_conversion) for more details). Integer literals default to being of `int` type.

The third special name is `uint`. It follows the same rules as `int`, only it is unsigned (the values are always 0 or positive).

There are two other special names for integer types, `rune` and `uintptr`. You looked at rune literals earlier and I’ll discuss the `rune` type in [“A Taste of Strings and Runes”](#ch01.html#string_rune) and `uintptr` in Chapter 16.

### Choosing which integer to use

Go provides more integer types than some other languages. Given all these choices, you might wonder when you should use each of them. You should follow three simple rules:

*   By default, use `int`.
    
*   If you are working with a binary file format or network protocol that has an integer of a specific size or sign, use the corresponding integer type.
    
*   If you are writing a library function that should work with any integer type, take advantage of Go’s generics support and use a generic type parameter to represent any integer type (I talk more about functions and their parameters in Chapter 5 and more about generics in Chapter 8.)
    

###### Note

You’ll likely find legacy code where there’s a pair of functions that do the same thing, but one has `int64` for the parameters and variables and the other has `uint64`. The reason is that the API was created before generics were added to Go. Without generics, you needed to write functions with slightly different names to implement the same algorithm with different types. Using `int64` and `uint64` meant that you could write the code once and let callers use type conversions to pass values in and convert data that’s returned.

You can see this pattern in the Go standard library within the functions `FormatInt` and `FormatUint` in the `strconv` package. Due to the compatibility promise, these functions will never go away, but they implement an out-of-date style.

### Integer operators

Go integers support the usual arithmetic operators: +, `-`, `*`, `/`, with `%` for modulus. The result of an integer division is an integer; if you want to get a floating-point result, you need to use a type conversion to make your integers into floating-point numbers. Also, be careful not to divide an integer by 0; this causes a panic (I talk more about panics in the Panic Recover section).

###### Note

Integer division in Go follows truncation toward zero. Modulus follows the same rules; `-7 % 2 == -1`. See the Go spec’s section on [arithmetic operators](https://oreil.ly/zp3OJ) for the full details.

You can combine any of the arithmetic operators with `=` to modify a variable: `+=`, `-=`, `*=`, `/=`, and `%=`. For example, the following code results in `x` having the value 20:

```
var x int = 10
x *= 2
```

Go also includes the `++` and `--` operators to increment and decrement the value of a variable. Unlike Java, C++, and several other languages, Go requires these operators to appear after the variable.

```
var x int = 10
x ++ // ok
++x  // syntax error, does not compile
```

You compare integers with `==`, `!=`, `>`, `>=`, `<`, and `<=`. There are three unary integer operators: `+`, `-`, and `^`. The first two make a number positive or negative, while the third performs a bitwise NOT on the integer.

Go also has bit-manipulation operators for integers. You can bit shift left and right with `<<` and `>>`, or do bit masks with `&` (bitwise AND), `|` (bitwise OR), `^` (bitwise XOR), and `&^` (bitwise AND NOT). As with the arithmetic operators, you can also combine all the bitwise operators with `=` to modify a variable: `&=`, `|=`, `^=`, `&^=`, `<<=`, and `>>=`.

## Integer overflow

Like many other languages, Go allows you to increment and decrement integer variables past their limits. Adding one to a variable set to the maximum value for a type returns the minimum value for the type, and subtracting one from the minimum returns the maximum. Be careful when writing code; don’t assume that `x + 1` is always greater than `x` and `x - 1` is always less than `x`.

```
var x uint = 0
x--
fmt.Println(x) // prints 18446744073709551615
x++
fmt.Println(x) // prints 0
```

### Floating-point types

Go has two floating-point types, as shown in [Table 1-2](#ch01.html#table2_2).

Table 1-2. The floating-point types in Go

  
| Type name || Largest absolute value || Smallest (nonzero) absolute value |
| --- | --- | --- |
| `float32` || 3.40282346638528859811704183484516925440e+38 || 1.401298464324817070923729583289916131280e-45 |
| --- | --- | --- |
| `float64` || 1.797693134862315708145274237317043567981e+308 || 4.940656458412465441765687928682213723651e-324 |

Like the integer types, the zero value for the floating-point types is 0.

Floating point in Go is similar to floating-point math in other languages. Go uses the IEEE 754 specification, giving a large range and limited precision. Picking which floating-point type to use is straightforward: unless you have to be compatible with an existing format, use `float64`. Floating-point literals have a default type of `float64`, so always using `float64` is the simplest option. It also helps mitigate floating-point accuracy issues since a `float32` has only six- or seven-decimal digits of precision. Don’t worry about the difference in memory size unless you have used the profiler to determine that it is a significant source of problems. (Testing and profiling are covered in [Chapter 2](#ch02.html#unique_chapter_id_15).)

The bigger question is whether you should be using a floating-point number at all. In many cases, the answer is no. Just like other languages, Go floating-point numbers have a huge range, but they cannot store every value in that range; they store the nearest approximation. Because floats aren’t exact, they can be used only in situations where inexact values are acceptable or the rules of floating point are well understood. That limits them to things like graphics, statistics, and scientific operations. Some functions that predate generics use `float64` parameters as a way to provide a single implementation for all floating point and integer types, such as the `Min`, and `Max` function in the standard library’s `math` package. This is an obsolete pattern (and one that fails to work correctly for 64-bit integers outside the range of -2^53 to 2^53).

###### Warning

A floating-point number cannot represent a decimal value exactly. Do not use them to represent money or any other value that must have an exact decimal representation! In Chapter 10, you’ll learn how to use the third-party `shopspring/decimal` module to handle exact decimal values.

You can use all the standard mathematical and comparison operators with floats, except `%`. Floating-point division has a couple of interesting properties. Dividing a nonzero floating-point variable by 0 returns `+Inf` or `-Inf` (positive or negative infinity), depending on the sign of the number. Dividing a floating-point variable set to 0 by 0 returns `NaN` (Not a Number).

While Go lets you use `==` and `!=` to compare floats, don’t do it. Because of the inexact nature of floats, two floating-point values might not be equal when you think they should be. Instead, define a maximum allowed variance and see if the difference between two floats is less than that. This value (sometimes called *epsilon*) depends on your accuracy needs; I can’t give you a simple rule. If you aren’t sure, consult your friendly local mathematician for advice. If you can’t find one, [The Floating Point Guide has a “Comparison” page](https://oreil.ly/n9ws3) that can help you out (or possibly convince you to avoid floating-point numbers unless absolutely necessary). Another gotcha is that by definition, `NaN` isn’t equal to anything, including itself. You can see this demonstrated on [The Go Playground](https://go.dev/play/p/LK8aj87-vSI). If it’s possible that a floating point variable could be `NaN`, use the function `math.IsNaN()` to test it.

### Complex types (you’re probably not going to use these)

There is one more numeric type and it is pretty unusual. Go has first-class support for complex numbers. If you don’t know what complex numbers are, you are not the target audience for this feature; feel free to skip ahead.

There isn’t a lot to the complex number support in Go. Go defines two complex number types. `complex64` uses `float32` values to represent the real and imaginary part, and `complex128` uses `float64` values. Both are declared with the `complex` built-in function:

```
var complexNum = complex(20.3, 10.2)
```

The type returned by `complex` depends on its parameters. A `complex64` is returned if both parameters are `float32` or one parameter is a `float32` and the other is an untyped constant or literal that fits within a `float32`. Otherwise, the function returns a `complex128`.

All the standard floating-point arithmetic operators work on complex numbers. Just as with floats, you can use `==` or `!=` to compare them, but they have the same precision limitations, so it’s best to use the epsilon technique. You can extract the real and imaginary portions of a complex number with the `real` and `imag` built-in functions, respectively. The `math/cmplx` package has additional functions for manipulating `complex128` values.

The zero value for both types of complex numbers has 0 assigned to both the real and imaginary portions of the number.

[Example 1-1](#ch01.html#EX2_4) shows a simple program that demonstrates how complex numbers work. You can run it for yourself on [The Go Playground](https://go.dev/play/p/rRofeSJAazA) or in the *sample\_code/complex\_numbers* directory in the [Chapter 2 repository](https://oreil.ly/zXZqI).

##### Example 1-1. Complex numbers

```
func main() {
	x := complex(2.5, 3.1)
	y := complex(10.2, 2)
	fmt.Println(x - y)
	fmt.Println(real(x))
	fmt.Println(imag(x))
	fmt.Println(cmplx.Abs(x))
}
```

Running this code gives you the following:

```
(-7.699999999999999+1.1i)
2.5
3.1
3.982461550347975
```

You can see floating-point imprecision on display in the results.

If you were wondering what the fifth kind of primitive literal was, Go supports imaginary literals to represent the imaginary portion of a complex number. They look just like floating-point literals, but they have an `i` for a suffix.

Despite having complex numbers as a predeclared type, Go is not a popular language for numerical computing. Adoption has been limited because other features (like matrix support) are not part of the language and libraries have to use inefficient replacements, like slices of slices. (You’ll look at slices in Chapter 3 and how they are implemented in Chapter 6.) But if you need to calculate a Mandelbrot set as part of a larger program, or implement a quadratic equation solver, complex number support is there for you.

You might be wondering why Go includes complex numbers. The answer is simple: Ken Thompson, one of the creators of Go (and Unix), thought they would be [interesting](https://oreil.ly/eBmkq). There was a discussion about [removing complex numbers](https://oreil.ly/Q76EV) from Go, but the compatibility promise means they are here to stay.

###### Note

If you do want to write numerical computing applications in Go, use the third-party [Gonum](https://www.gonum.org) package. It takes advantage of complex numbers and provides useful libraries for things like linear algebra, matrices, integration, graphs, and statistics. Other languages have better numerical computing support, but Go’s focus on software engineering might be an advantage for larger projects.

## A Taste of Strings and Runes

This brings us to strings. Like most modern languages, Go includes strings as a built-in type. The zero value for a string is the empty string. Go supports Unicode; as I showed [“Literals”](#ch01.html#goliterals), you can put any Unicode character into a string. Like integers and floats, strings are compared for equality using `==`, difference with `!=`, or ordering with `>`, `>=`, `<`, or `<=`. They are concatenated by using the `+` operator.

Strings in Go are immutable; you can reassign the value of a string variable, but you cannot change the value of the string that is assigned to it.

Go also has a type that represents a single code point. The *rune* type is an alias for the `int32` type, just as `byte` is an alias for `uint8`. As you could probably guess, a rune literal’s default type is a rune, and a string literal’s default type is a string.

If you are referring to a character, use the rune type, not the `int32` type. They might be the same to the compiler, but you want to use the type that clarifies the intent of your code:

```
var myFirstInitial rune = 'J' // good - the type name matches the usage
var myLastInitial int32 = 'B' // bad - legal but confusing
```

I am going to talk a lot more about strings in the next chapter, covering some implementation details, relationships with bytes and runes, as well as advanced features and pitfalls.

# Exercises

The following exercises will test your knowledge of the predeclared types and their limits. The exercise solutions can be found in the exercise\_solutions directory in the Chapter 2 repository.

1.  Write a program that declares variables using each of the four literal forms (integer in base 2, 8, 10, and 16 with underscore separators), a floating-point literal with an exponent, a rune literal using a Unicode escape, and both an interpreted and raw string literal. Use `fmt.Printf` with appropriate verbs to display each value and its type. You can find the verbs documented at [pkg.go.dev](https://pkg.go.dev/fmt@go1.26.2).
    
2.  Declare a variable b of type `byte` with the value 255 and a variable s of type `int8` with the value 127. Add 1 to each and print the results. Then declare a `byte` with value 0, subtract 1, and print that result. Explain in a comment what is happening and why Go doesn’t produce an error at runtime.
    
3.  Write a program that assigns the value 1 << 53 (2^53) to a `float64` variable a, and 1<<53 + 1 to a `float64` variable b. Print whether a == b.
    

## Explicit Type Conversion

Most languages that have multiple numeric types automatically convert from one to another when needed. This is called *automatic type promotion*, and while it seems very convenient, it turns out that the rules to properly convert one type to another can get complicated and produce unexpected results. As a language that values clarity of intent and readability, Go doesn’t allow automatic type promotion between variables. You must use a *type conversion* when variable types do not match. Even different-sized integers and floats must be converted to the same type to interact. This makes it clear exactly what type you want without having to memorize any type conversion rules (see [Example 1-2](#ch01.html#EX2_1)).

##### Example 1-2. Type conversions

```
var x int = 10
var y float64 = 30.2
var sum1 float64 = float64(x) + y
var sum2 int = x + int(y)
fmt.Println(sum1, sum2)
```

In this sample code, you define four variables. `x` is an `int` with the value 10, and `y` is a `float64` with the value 30.2. Since these are not identical types, you need to convert them to add them together. For `sum1`, you convert `x` to a `float64` using a `float64` type conversion, and for `sum2`, you convert `y` to an `int` using an `int` type conversion. When you run this code, it prints out 40.2 40.

The same behavior applies with different-sized integer types (see [Example 1-3](#ch01.html#EX2_1a)).

##### Example 1-3. Integer type conversions

```
var x int = 10
var b byte = 100
var sum3 int = x + int(b)
var sum4 byte = byte(x) + b
fmt.Println(sum3, sum4)
```

You can run these examples on [The Go Playground](https://oreil.ly/VoE7H) or in the *sample\_code/type\_conversion* directory in the [Chapter 2 repository](https://oreil.ly/dGtos).

This strictness around types has other implications. Since all type conversions in Go are explicit, you cannot treat another Go type as a boolean. In many languages, a nonzero number or a nonempty string can be interpreted as a boolean `true`. Just like automatic type promotion, the rules for “truthy” values vary from language to language and can be confusing. Unsurprisingly, Go doesn’t allow truthiness. In fact, *no other type can be converted to a bool, implicitly or explicitly*. If you want to convert from another data type to boolean, you must use one of the comparison operators (`==`, `!=`, `>`, `<`, `<=`, or `>=`). For example, to check if variable `x` is equal to 0, the code would be `x == 0`. If you want to check if string `s` is empty, use `s == ""`.

###### Note

Type conversions are one of the places where Go chooses to add a little verbosity in exchange for a great deal of simplicity and clarity. You’ll see this trade-off multiple times. Idiomatic Go values comprehensibility over conciseness.

## min and max

Go provides two built-in functions for comparing numbers and strings, `min`, and `max`. Both take one or more values, all of which must be the same type. `min` returns the smallest value and `max` returns the largest.

In addition to the built-in functions, Go provides several functions in the standard library that find the minimum or maximum of numbers. Unfortunately, they have different behaviors when handling NaN, Inf, and -Inf. For the built-in `min` and `max` functions, if any of the values passed to the functions is `NaN`, the functions return `NaN`, even if the other values include Inf or -Inf. The `slices.Min` and `slices.Max` functions behave the same. However, the `math.Min` and `math.Max` functions are the opposite. They treat negative infinity as the smallest value and positive infinity as the biggest value. You can see this demonstrated on [The Go Playground](https://go.dev/play/p/YdKF8pk3rde).

Which function you should use depends on the circumstances. If you have a need to be compatible with the behavior of `math.Min` and `math.Max`, use them. If the number of values being compared can vary, use `slices.Min` and `Max`. In any other situation, the built-in functions are preferred.

###### Note

Why are `min` and `max` built-in functions? You can read [the long discussion](https://github.com/golang/go/issues/59488) before they were implemented to learn about the history. Two reasons stand out, one practical and one philosophical. First, implementing `min` and `max` as built-in variadic functions avoids allocating memory every time they are called (memory allocation is covered in Chapter 6). Perhaps more importantly, there was a feeling that these are fundamental operations, like addition or subtraction, and fundamental operations should be built-in. As Russ Cox said, “It is ultimately a matter of design intuition and judgement.”

## Literals Are Untyped

While you can’t add two integer variables together if they are declared to be of different types of integers, Go lets you use an integer literal in floating-point expressions or even assign an integer literal to a floating-point variable:

```
var x float64 = 10
var y float64 = 200.3 * 5
```

This is because, as I mentioned earlier, literals in Go are untyped. Go is a practical language, and it makes sense to avoid forcing a type until the developer specifies one. This means they can be used with any variable whose type is compatible with the literal. When you look at user-defined types in Chapter 7, you’ll see that you can even use literals with user-defined types based on predefined types. Being untyped goes only so far. You can’t assign a literal string to a variable with a numeric type or a literal number to a string variable, nor can you assign a float literal to an `int`. These are all flagged by the compiler as errors. Size limitations also exist. While you can write numeric literals that are larger than any integer can hold, it is a compile-time error to try to assign a literal whose value overflows the specified variable, such as trying to assign the literal 1000 to a variable of type `byte`.

# var Versus :=

For a small language, Go has a lot of ways to declare variables. There’s a reason for this: each declaration style communicates something about how the variable is used. Let’s go through the ways you can declare a variable in Go and see when each is appropriate.

The most verbose way to declare a variable in Go uses the `var` keyword, an explicit type, and an assignment. It looks like this:

```
var x int = 10
```

If the type on the righthand side of the `=` is the expected type of your variable, you can leave off the type from the left side of the `=`. Since the default type of an integer literal is `int`, the following declares `x` to be a variable of type `int`:

```
var x = 10
```

Conversely, if you want to declare a variable and assign it the zero value, you can keep the type and drop the `=` on the righthand side:

```
var x int
```

You can declare multiple variables at once with `var`, and they can be of the same type:

```
var x, y int = 10, 20
```

You can declare all zero values of the same type:

```
var x, y int
```

or of different types:

```
var x, y = 10, "hello"
```

There’s one more way to use `var`. If you are declaring multiple variables at once, you can wrap them in a *declaration list*:

```
var (
    x    int
    y        = 20
    z    int = 30
    d, e     = 40, "hello"
    f, g string
)
```

Go also supports a short declaration and assignment format. When you are within a function, you can use the `:=` operator to replace a `var` declaration that uses type inference. The following two statements do exactly the same thing—they declare `x` to be an `int` with the value of 10:

```
var x = 10
x := 10
```

As with `var`, you can declare multiple variables at once using `:=`. These two lines both assign 10 to `x` and “hello” to `y`:

```
var x, y = 10, "hello"
x, y := 10, "hello"
```

The `:=` operator can do one trick that you cannot do with `var`: it allows you to assign values to existing variables too. As long as at least one new variable is on the lefthand side of the `:=`, any of the other variables can already exist:

```
x := 10
x, y := 30, "hello"
```

Using `:=` has one limitation. If you are declaring a variable at the package level, you must use `var` because `:=` is not legal outside of functions.

How do you know which style to use? As always, choose what makes your intent clearest. The most common declaration style within functions is `:=`. Outside of a function, use declaration lists on the rare occasions when you are declaring multiple package-level variables.

In some situations within functions, you should avoid `:=`:

*   When initializing a variable to its zero value, use `var x int`. This makes it clear that the zero value is intended.
    
*   When assigning an untyped constant or a literal to a variable and the default type for the constant or literal isn’t the type you want for the variable, use the long `var` form with the type specified. While it is legal to use a type conversion to specify the type of the value and use `:=` to write `x := byte(20)`, it is idiomatic to write `var x byte = 20`.
    
*   Because `:=` allows you to assign to both new and existing variables, it sometimes creates new variables when you think you are reusing existing ones (see [Link to Come] for details). In those situations, explicitly declare all your new variables with `var` to make it clear which variables are new, and then use the assignment operator (`=`) to assign values to both new and old variables.
    

While `var` and `:=` allow you to declare multiple variables on the same line, use this style only when assigning multiple values returned from a function or the comma ok idiom (see Chapter 5).

You should rarely declare variables outside of functions, in what’s called the *package block* (see Chapter 4). Package-level variables whose values change are a bad idea. When you have a variable outside of a function, it can be difficult to track the changes made to it, which makes it hard to understand how data is flowing through your program. This can lead to subtle bugs. As a general rule, you should only declare variables in the package block that are effectively immutable.

###### Tip

Avoid declaring variables outside of functions because they complicate data flow analysis.

You might be wondering: does Go provide a way to *ensure* that a value is immutable? It does, but it is a bit different from what you may have seen in other programming languages. It’s time to learn about `const`.

# Using const

Many languages have a way to declare a value as immutable. In Go, this is done with the `const` keyword. At first glance, it seems to work exactly as it would in other languages. Try out the code in [Example 1-4](#ch01.html#EX2_7) on [The Go Playground](https://oreil.ly/FdG-W) or in the *sample\_code/const\_declaration* directory in the [Chapter 2 repository](https://oreil.ly/QxTTT).

##### Example 1-4. `const` declarations

```
package main

import "fmt"

const x int64 = 10

const (
    idKey   = "id"
    nameKey = "name"
)

const z = 20 * 10

func main() {
    const y = "hello"

    fmt.Println(x)
    fmt.Println(y)

    x = x + 1 // this will not compile!
    y = "bye" // this will not compile!
    fmt.Println(x)
    fmt.Println(y)
}
```

When you run this code, compilation fails with the following error messages:

```
./prog.go:20:2: cannot assign to x (constant 10 of type int64)
./prog.go:21:2: cannot assign to y (untyped string constant "hello")
```

As you see, you declare a constant at the package level or within a function. Just as with `var`, you can (and should) declare a group of related constants within a set of parentheses.

Be aware that `const` in Go is very limited. Constants in Go are a way to give names to literals. They can only hold values that the compiler can figure out at compile time. This means that they can be assigned:

*   Numeric literals
    
*   `true` and `false`
    
*   Strings
    
*   Runes
    
*   The values returned by the built-in functions `complex`, `real`, `imag`, `len`, and `cap`
    
*   Expressions that consist of operators and the preceding values
    

###### Note

I’ll cover the `len` and `cap` functions in the next chapter. Another value that can be used with `const` is called `iota`. I’ll talk about `iota` when I discuss creating your own types in Chapter 7.

Go doesn’t provide a way to specify that a value calculated at runtime is immutable. For example, the following code will fail to compile with the error `x + y (value of type int) is not constant`:

```
x := 5
y := 10
const z = x + y // this won't compile!
```

As you’ll see in the next chapter, there are no immutable arrays, slices, maps, or structs, and there’s no way to declare that a field in a struct is immutable. This is less limiting than it sounds. Within a function, it is clear if a variable is being modified, so immutability is less important. In Chapter 5, you’ll see how Go prevents modifications to variables that are passed as parameters to functions.

###### Tip

Constants in Go are a way to give names to literals. There is *no* way in Go to declare that a variable is immutable.

# Typed and Untyped Constants

Constants can be typed or untyped. An untyped constant works exactly like a literal; it has no type of its own but does have a default type that is used when no other type can be inferred. A typed constant can be directly assigned only to a variable of that type.

Whether to make a constant typed depends on why the constant was declared. If you are giving a name to a mathematical constant that could be used with multiple numeric types, keep the constant untyped. In general, leaving a constant untyped gives you more flexibility. In certain situations, you’ll want a constant to enforce a type. You’ll see a use for typed constants when I cover enumerations with `iota` in Chapter 7.

Here’s what an untyped constant declaration looks like:

```
const x = 10
```

All of the following assignments are legal:

```
var y int = x
var z float64 = x
var d byte = x
```

Here’s what a typed constant declaration looks like:

```
const typedX int = 10
```

This constant can be assigned directly only to an `int`. Assigning it to any other type produces a compile-time error like this:

```
cannot use typedX (type int) as type float64 in assignment
```

# Exercises

1.  Write a program that declares a variable x of type int with the value 7, and a variable y of type float64 with the value 3.5. Compute x + y by converting to the appropriate type, storing the result as a float64 called sum. Print sum. Then compute the integer division of x by 3 and the modulus of x by 3. Print both results and verify they satisfy the relationship (x/3)\*3 + (x%3) == x.
    
2.  Write a program that declares an untyped constant maxSpeed with the value 299\_792\_458. Assign it to variables of type int, float64, int64, and uint32. Print all four. Then declare a typed constant typedSpeed int64 = 299\_792\_458 and try assigning it to an int variable. What happens, and why?
    
3.  Write a program that demonstrates the difference between var and := in three scenarios: (a) declaring a byte variable with a specific value (show why var x byte = 20 is preferred over x := byte(20)), (b) declaring a variable to its zero value, and (c) a situation where := accidentally shadows an existing variable inside an if block. Add comments explaining which style is idiomatic in each case.
    

# Unused Variables

One of the goals for Go is to make it easier for large teams to collaborate on programs. To do so, Go has some rules that are unique among programming languages. In Chapter 1, you saw that Go programs need to be formatted in a specific way with `go fmt` to make it easier to write code-manipulation tools and to provide coding standards. Another Go requirement is that *every declared local variable must be read*. It is a *compile-time error* to declare a local variable and to not read its value.

The compiler’s unused variable check is not exhaustive. As long as a variable is read once, the compiler won’t complain, even if there are writes to the variable that are never read. The following is a valid Go program that you can run on [The Go Playground](https://oreil.ly/8JLA6) or in the *sample\_code/assignments\_not\_read* directory in the [Chapter 2 repository](https://oreil.ly/FqALl):

```
func main() {
    x := 10 // this assignment isn't read!
    x = 20
    fmt.Println(x)
    x = 30 // this assignment isn't read!
}
```

While the compiler and `go vet` do not catch the unused assignments of 10 and 30 to `x`, third-party tools can detect them. I’ll talk about these tools in the Code Quality section.

###### Note

The Go compiler won’t stop you from creating unread package-level variables. This is one more reason you should avoid creating package-level variables.

# Naming Variables and Constants

There is a difference between Go’s rules for naming variables and the patterns that Go developers follow when naming their variables and constants. Like most languages, Go requires identifier names to start with a letter or underscore, and the name can contain numbers, underscores, and letters. Go’s definition of “letter” and “number” is a bit broader than many languages. Any Unicode character considered a letter or digit is allowed. This makes all the variable definitions in [Example 1-5](#ch01.html#EX1) perfectly valid Go.

##### Example 1-5. Variable names you should never use

```
_0 := 0_0
_𝟙 := 20
π := 3
ａ := "hello" // Unicode U+FF41
__ := "double underscore" // two underscores
fmt.Println(_0)
fmt.Println(_𝟙)
fmt.Println(π)
fmt.Println(ａ)
fmt.Println(__)
```

You can test out this awful code on [The Go Playground](https://oreil.ly/VIYOk). While it works, *do not* use variable names like this. These names are considered nonidiomatic because they break the fundamental rule of making sure that your code communicates what it is doing. These names are confusing or difficult to type on many keyboards. Look-alike Unicode code points are the most insidious, because even if they appear to be the same character, they represent entirely different variables. You can run the code shown in [Example 1-6](#ch01.html#EX2_10) on [The Go Playground](https://oreil.ly/hrvb6) or in the *sample\_code/look\_alike\_code\_points* directory in the [Chapter 2 repository](https://oreil.ly/7nLfx).

##### Example 1-6. Using look-alike code points for variable names

```
func main() {
    ａ := "hello"   // Unicode U+FF41
    a := "goodbye" // standard lowercase a (Unicode U+0061)
    fmt.Println(ａ)
    fmt.Println(a)
}
```

When you run this program, you get:

```
hello
goodbye
```

Even though the underscore is a valid character in a variable name, it is rarely used, because idiomatic Go doesn’t use snake case (names like `index_counter` or `number_tries`). Instead, idiomatic Go uses camel case (names like `indexCounter` or `numberTries`) when an identifier name consists of multiple words.

###### Note

An underscore by itself (`_`) is a special identifier name in Go; I’ll talk more about it when I cover functions in Chapter 5.

In many languages, constants are always written in all uppercase letters, with words separated by underscores (names like `INDEX_COUNTER` or `NUMBER_TRIES`). Go does not follow this pattern. This is because Go uses the case of the first letter in the name of a package-level declaration to determine if the item is accessible outside the package. I will revisit this when I talk about packages in Chapter 10.

Within a function, favor short variable names. *The smaller the scope for a variable, the shorter the name that’s used for it*. It is common in Go to see single-letter variable names used with `for` loops. For example, the names `k` and `v` (short for *key* and *value*) are used as the variable names in a `for-range` loop. If you are using a standard `for` loop, `i` and `j` are common names for the index variable. There are other idiomatic ways to name variables of common types; I will mention them as I cover more parts of the standard library.

Some languages with weaker type systems encourage developers to include the expected type of the variable in the variable’s name. Since Go is strongly typed, you don’t need to do this to keep track of the underlying type. However, you may see Go code where the first letter of a type is used as the variable name (for example, `i` for integers or `f` for floats). When you define your own types, similar patterns apply, especially when naming receiver variables (which are covered in Chapter 7).

These short names serve two purposes. The first is that they eliminate repetitive typing, keeping your code shorter. Second, they serve as a check on how complicated your code is. If you find it hard to keep track of your short-named variables, your block of code is likely doing too much.

When naming variables and constants in the package block, use more descriptive names. The type should still be excluded from the name, but since the scope is wider, you need a more complete name to clarify what the value represents.

For more discussion of Go naming recommendations, read the [Naming section of Google’s Go Style Decisions](https://oreil.ly/6AUc_).

# Exercises

These exercises demonstrate the concepts discussed in the chapter. Solutions to these exercises, along with the programs in this chapter, are in the [Chapter 2 repository](https://oreil.ly/nGUVd).

1.  Use the built-in `min` function to find the minimum of the values 10, -3, 7.2, and 800. Assign this to a variable called `smallest`. Use the built-in `max` function to find the maximum of the values 10, -3, 7.2, and 800. Assign this to a variable called `biggest`. Print `smallest` and `biggest`. What is the type of these variables?
    
2.  Look at *main.go* in the *exercises/ex8* directory in the [Chapter 2 Repository](https://TODO). Without compiling the code, guess if this code will compile, and if not, why not. Fix the program so it does compile by adding `fmt.Println` statements. Which variables and constants must be used for the program to compile, and why?
    
3.  Look at *main.go* in the *exercises/ex9* directory in the [Chapter 2 Repository](https://TODO). The program works, but the variable and constant names are bad. Rewrite the program so every variable and constant has an idiomatic Go name.
    

# Wrapping Up

You’ve covered a lot of ground here, understanding how to use the predeclared types, declare variables, and work with assignments and operators. In the next chapter, we will look at the composite types in Go: arrays, slices, maps, and structs. We will also take another look at strings and runes and how they interact with character encodings.

# Chapter 2. Writing Tests

Since the 2000s, the widespread adoption of automated testing has probably done more to improve code quality than any other software engineering technique. As a language and ecosystem focused on improving software quality, it’s not surprising that Go includes testing support as part of its standard library. Go makes it so easy to test your code, there’s no excuse to not do it.

In this chapter, you’ll see how to test Go code, group tests into unit and integration tests, examine code coverage, write benchmarks, and learn how to check code for concurrency issues by using the Go data race detector. Along the way, I’ll discuss how to write code that is testable and why this improves our code quality.

# Understanding the Basics of Testing

Go’s testing support has two parts: libraries and tooling. The `testing` package in the standard library provides the types and functions to write tests, while the `go test` tool that’s bundled with Go runs your tests and generates reports. Unlike many other languages, Go places its tests in the same directory and package as the production code, so they can access and test unexported functions and variables. You’ll see in a bit how to write tests that ensure that you are testing only a public API.

###### Note

Complete code samples for this chapter are found in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15).

Let’s write a simple function and then a test to make sure the function works. In the *sample\_code/adder* directory, in the file *adder.go*, you have the following:

```
func addNumbers(x, y int) int {
    return x + x
}
```

The corresponding test is in *adder\_test.go*:

```
func Test_addNumbers(t *testing.T) {
    result := addNumbers(2,3)
    if result != 5 {
        t.Error("incorrect result: expected 5, got", result)
    }
}
```

Every test is written in a file whose name ends with *\_test.go*. If you are writing tests against *foo.go*, place your tests in a file named *foo\_test.go*.

Test functions start with the word `Test` and take in a single parameter of type `*testing.T`. By convention, this parameter is named `t`. Test functions do not return any values. The name of the test (apart from starting with the word “Test”) is meant to document what you are testing, so pick something that explains what you are testing. When writing unit tests for individual functions, the convention is to name the unit test `Test` followed by the name of the function. When testing unexported functions, some people use an underscore between the word `Test` and the name of the function.

Also note that you use standard Go code to call the code being tested and to validate that the responses are as expected. When there’s an incorrect result, you report the error with the `t.Error` method, which works like the `fmt.Print` function. You’ll see other error-reporting methods in a bit.

You’ve just seen the library portion of Go’s test support. Now let’s take a look at the tooling. Just as `go build` builds a binary and `go run` runs a program, the command `go test` runs the test functions in the current directory:

```
$ go test
--- FAIL: Test_addNumbers (0.00s)
    adder_test.go:8: incorrect result: expected 5, got 4
FAIL
exit status 1
FAIL    test_examples/adder     0.006s
```

It looks like you found a bug in your code. Taking a second look at `addNumbers`, you see that you are adding `x` to `x`, not `x` to `y`. Let’s change the code and rerun the test to verify that the bug is fixed:

```
$ go test
PASS
ok      test_examples/adder     0.006s
```

The `go test` command allows you to specify which packages to test. Using `./...` for the package name specifies that you want to run tests in the current directory and all subdirectories of the current directory. Include a `-v` flag to get verbose testing output.

## Reporting Test Failures

There are several methods on `*testing.T` for reporting test failures. You’ve already seen `Error`, which builds a failure description string out of a comma-separated list of values.

If you’d rather use a `Printf`-style formatting string to generate your message, use the `Errorf` method instead:

```
t.Errorf("incorrect result: expected %d, got %d", 5, result)
```

While `Error` and `Errorf` mark a test as failed, the test function continues running. If you think a test function should stop processing as soon as a failure is found, use the `Fatal` and `Fatalf` methods. The `Fatal` method works like `Error`, and the `Fatalf` method works like `Errorf`. The difference is that the test function exits immediately after the test failure message is generated. Note that this doesn’t exit *all* tests; any remaining test functions will execute after the current test function exits.

When should you use `Fatal`/`Fatalf` and when should you use `Error`/`Errorf`? If the failure of a check in a test means that further checks in the same test function will always fail or cause the test to panic, use `Fatal` or `Fatalf`. If you are testing several independent items (such as validating fields in a struct), then use `Error` or `Errorf` so you can report many problems at once. This makes it easier to fix multiple problems without rerunning your tests over and over.

## Logging

It is often helpful to output additional information when running tests. Go provides access to the testing logger via a set of methods on `*testing.T`. While you can use `fmt.Println` and `fmt.Printf`, it is better to use the testing logger because its output is only displayed when a test fails (or when you turn on verbose output).

The most commonly-used test logging methods are `Log` and `Logf`, which provide unformatted and formatted output, respectively. The log message is indented and prefixed with the file name and line number of the log statement.

If your log output is processed by external tools, you can include structured data in your output with the `Attr` method. It takes two string parameters, a key and a value. The meaning of the key/value pair is up to you; the only limitation that the key cannot contain any whitespace characters. The method writes a line with the following values, separated by whitespace: `===` `ATTR`, the name of the test, the key, and the value.

Finally, the `Output` method provides access to the test logger’s underlying `io.Writer`. Like `Log` and `Logf`, anything written using this `io.Writer` is indented, but there’s no file name/line number prefix.

```
func TestLogging(t *testing.T) {
    fmt.Println("fmt.Println does work, but it's always there and unindented")
    t.Log("This is a simple string message")
    t.Logf("This message has %d %s specifiers in it", 2, "formatting")
    t.Attr("key_for_processing", "value_for_processing with whitespace")
    t.Output().Write([]byte("a final entry"))
}
```

Here’s the output for this test:

```
$ go test -v
=== RUN   TestLogging
fmt.Println does work, but it's always there and unindented
    logging_test.go:10: This is a simple string message
    logging_test.go:11: This message has 2 formatting specifiers in it
=== ATTR  TestLogging key_for_processing value_for_processing with whitespace
    a final entry
--- PASS: TestLogging (0.00s)
PASS
ok      github.com/learning-go-book-3e/ch15/sample_code/logging 0.204s
```

## Saving Test Artifacts

It is sometimes useful to capture output files from a test (for additional analysis, perhaps). Instead of hard-coding a location into your test code, take advantage of Go’s artifact directory support. Use the `ArtifactDir` method to get the name of the directory where the file should be written. By default, this is a temporary directory that is deleted when the test completes. This gives you control over when an artifact is permanently written; the code to generate the artifact can always be present, but you decide when to capture a new artifact. To make an artifact permanent, set the `-artifacts` flag when running `go test`. This changes the artifact directory to a subdirectory of the directory where `go test` is running.

```
func TestArtifacts(t *testing.T) {
    myDir := t.ArtifactDir()
    t.Log(myDir)
    f, err := os.Create(myDir + "/test.txt")
    if err != nil {
        t.Fatal(err)
    }
    defer f.Close()
    f.Write([]byte("valuable test data"))
}
```

```
$ go test -v
=== RUN   TestArtifacts
    artifact_test.go:10: /var/folders/hj/mr53lrfj1bqf4hg62wqkp4000000gn/T/TestArtifacts1377298058/001
--- PASS: TestArtifacts (0.00s)

$ go test -v -artifacts
=== RUN   TestArtifacts
=== ARTIFACTS TestArtifacts /Users/gobook/projects/learning-go-3e-code/ch15/sample_code/artifact/_artifacts/1837011095
    artifact_test.go:10: /Users/gobook/projects/learning-go-3e-code/ch15/sample_code/artifact/_artifacts/1837011095
--- PASS: TestArtifacts (0.00s)
```

###### Note

If you want to customize the artifact output directory, set the `-outputdir` flag on `go test`.

## Setting Up and Tearing Down

Sometimes you have some common state that you want to set up before any tests run and remove when testing is complete. Use a `TestMain` function to manage this state and run your tests:

```
var testTime time.Time

func TestMain(m *testing.M) {
    fmt.Println("Set up stuff for tests here")
    testTime = time.Now()
    exitVal := m.Run()
    fmt.Println("Clean up stuff after tests here")
    os.Exit(exitVal)
}

func TestFirst(t *testing.T) {
    slog.Info("TestFirst uses stuff set up in TestMain", testTime)
}

func TestSecond(t *testing.T) {
    slog.Info("TestSecond also uses stuff set up in TestMain", testTime)
}
```

Both `TestFirst` and `TestSecond` refer to the package-level variable `testTime`. Assume that it needs to be initialized in order for the tests to run properly. You declare a function called `TestMain` with a parameter of type `*testing.M`. If there’s a function named `TestMain` in a package, `go test` calls it instead of the test functions. It is the responsibility of the `TestMain` function to set up any state that’s necessary to make the tests in the package run correctly.

Once the state is configured, the `TestMain` function calls the `Run` method on `*testing.M`. This runs the test functions in the package. The `Run` method returns the exit code; `0` indicates that all tests passed. Finally, the `TestMain` function must call `os.Exit` with the exit code returned from `Run`.

Running `go test` on this produces the output:

```
$ go test
Set up stuff for tests here
TestFirst uses stuff set up in TestMain 2026-03-30 23:10:55.190364 -0400 EDT
    m=+0.000474084
TestSecond also uses stuff set up in TestMain 2026-03-30 23:10:55.190364 -0400
    EDT m=+0.000474084
PASS
Clean up stuff after tests here
ok      test_examples/testmain  0.006s
```

###### Note

Be aware that `TestMain` is invoked once, not before and after each individual test. You can have only one `TestMain` per package.

`TestMain` is useful in two common situations:

*   When you need to set up data in an external repository, such as a database
    
*   When the code being tested depends on package-level variables that need to be initialized
    

As I’ve mentioned before (and will again!), you should avoid package-level variables in your programs. They make it hard to understand how data flows through your program. If you are using `TestMain` for this reason, consider refactoring your code.

The `Cleanup` method on `*testing.T` is used to clean up temporary resources created for a single test. This method has a single parameter: a function with no input parameters or return values. The function runs when the test completes. For simple tests, you can achieve the same result by using a `defer` statement, but `Cleanup` is useful when tests rely on helper functions to set up sample data, as you see in [Example 2-1](#ch02.html#EX15_1). It’s fine to call `Cleanup` multiple times. Just like `defer`, the functions are invoked in last-added, first-called order.

##### Example 2-1. Using `t.Cleanup`

```
// createFile is a helper function called from multiple tests
func createFile(t *testing.T) (_ string, err error) {
    f, err := os.Create("tempFile")
    if err != nil {
        return "", err
    }
    defer func() {
        err = errors.Join(err, f.Close())
    }()
    // write some data to f
    t.Cleanup(func() {
        os.Remove(f.Name())
    })
    return f.Name(), nil
}

func TestFileProcessing(t *testing.T) {
    fName, err := createFile(t)
    if err != nil {
        t.Fatal(err)
    }
    // do testing, don't worry about cleanup
}
```

If your test uses temporary files, you can avoid writing cleanup code by taking advantage of the `TempDir` method on `*testing.T`. This method creates a new temporary directory every time it is invoked and returns the full path of the directory. It also registers a handler with `Cleanup` to delete the directory and its contents when the test has completed. You can use it to rewrite the previous example:

```
// createFile is a helper function called from multiple tests
func createFile(tempDir string) (_ string, err error) {
    f, err := os.CreateTemp(tempDir, "tempFile")
    if err != nil {
        return "", err
    }
    defer func() {
        err = errors.Join(err, f.Close())
    }()
    // write some data to f
    return f.Name(), nil
}

func TestFileProcessing(t *testing.T) {
    tempDir := t.TempDir()
    fName, err := createFile(tempDir)
    if err != nil {
        t.Fatal(err)
    }
    // do testing, don't worry about cleanup
}
```

## Test Helpers

Functions like `createFile` are sometimes referred to as *test helpers* since they set up or tear down the state needed to properly test code, but do no testing themselves. If something goes wrong it is usually a fatal error, as the test cannot continue without valid initial conditions. To mark the function as a test helper, it is idiomatic to pass a `*testing.T` instance to these functions and add a call to `t.Helper` as the first line:

```
func TestFileProcessingWithCreateFileHelper(t *testing.T) {
    fName := createFileHelper(t)
    // do testing, don't worry about cleanup
    if !strings.Contains(fName, "tempFile") {
        t.Error("unexpected name")
    }
}

// createFileHelper is a helper function called from multiple tests
func createFileHelper(t *testing.T) string {
    t.Helper()
    tempDir := t.TempDir()
    f, err := os.CreateTemp(tempDir, "tempFile")
    if err != nil {
        t.Fatal(err)
    }
    // write some data to the file
    if err := f.Close(); err != nil {
        t.Fatal(err)
    }
    return f.Name()
}
```

When `t.Helper` is called, it modifies the output of any calls to `t.Error` or `t.Fatal` within the test helper. Rather than print the filename and line number of the line in the test helper, the filename and line number of the line in the test function that called the test helper is displayed. The *sample\_code/helper* directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/helper) has an example. It is also available on [the Go Playground](https://go.dev/play/p/zboR2P3V4bl). Run the test with the `t.Helper()` line commented and then uncommented and look at the difference in the error output.

Never test any business logic within a test helper; only use `t.Error` or `t.Fatal` if setup or teardown failed for some reason.

## Testing with Environment Variables

It’s a common (and very good) practice to configure applications with environment variables. To simplify writing tests for code that depends on environment variables, Go provides a helper method on `testing.T`. Call `t.Setenv` to register a value for an environment variable for your test. Behind the scenes, it calls `Cleanup` to revert the environment variable to its previous state when the test exits:

```
// assume ProcessEnvVars is a function that processes environment variables
// and returns a struct with an OutputFormat field
func TestEnvVarProcess(t *testing.T) {
    t.Setenv("OUTPUT_FORMAT", "JSON")
    cfg := ProcessEnvVars()
    if cfg.OutputFormat != "JSON" {
        t.Error("OutputFormat not set correctly")
    }
    // value of OUTPUT_FORMAT is reset when the test function exits
}
```

###### Note

While it is good to use environment variables to configure your application, it is also good to make sure that most of your code is entirely unaware of them. Be sure to copy the values of environment variables into configuration structs before your program starts its work, in your `main` function or soon afterward. Doing so makes it easier to reuse and test code, since *how* the code is configured has been abstracted away from *what* the code is doing.

Rather than writing this code yourself, you should strongly consider using a third-party configuration library, like [Viper](https://oreil.ly/-RUA-) or [envconfig](https://oreil.ly/rhGYk). Also, look at [GoDotEnv](https://oreil.ly/sN2Sp) as a way to store environment variables in *.env* files for development or continuous integration machines.

## Storing Sample Test Data

As `go test` walks your source code tree, it uses the current package directory as the current working directory. If you want to use sample data to test functions in a package, create a subdirectory named *testdata* to hold your files. Go reserves this directory name as a place to hold test files. When reading from *testdata*, always use a relative file reference. Since `go test` changes the current working directory to the current package, each package accesses its own *testdata* via a relative file path.

###### Tip

The [`text` package](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/text) in the *sample\_code* directory of the Chapter 15 repository demonstrates how to use *testdata*.

## Caching Test Results

As you learned in Chapter 10, Go caches compiled packages if they haven’t changed. Go also caches test results when running tests across multiple packages if they have passed and their code hasn’t changed. The tests are recompiled and rerun if you change any file in the package or in the *testdata* directory. You can also force tests to always run if you pass the flag `-count=1` to `go test`.

## Testing Your Public API

The tests that you’ve written are in the same package as the production code. This allows you to test both exported and unexported functions.

If you want to test just the public API of your package, Go has a convention for specifying this. You still keep your test source code in the same directory as the production source code, but you use `packagename_test` for the package name. Let’s redo the initial test case, using an exported function instead. You can find the code in the *sample\_code/pubadder* directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/pubadder). In the *adder\_public.go* file in `pubadder` package, the following exported function is declared:

```
func AddNumbers(x, y int) int {
    return x + y
}
```

You test the function as public API by using the following code in a file in the `pubadder` package named *adder\_public\_test.go*:

```
package pubadder_test

import (
    "github.com/learning-go-book-3e/ch15/sample_code/pubadder"
    "testing"
)

func TestAddNumbers(t *testing.T) {
    result := pubadder.AddNumbers(2, 3)
    if result != 5 {
        t.Error("incorrect result: expected 5, got", result)
    }
}
```

Notice that the package name for the test file is `pubadder_test`. You have to import `github.com/learning-go-book-3e/ch15/sample_code/pubadder` even though the files are in the same directory. To follow the convention for naming tests, the test function name matches the name of the `AddNumbers` function. Also note that you use `pubadder.AddNumbers`, since you are calling an exported function in a different package.

Just as you can call exported functions from within a package, you can test your public API from a test that is in the same package as your source code. The advantage of using the `_test` suffix in the package name is that it lets you treat your tested package as a “black box.” You are forced to interact with it only via its exported functions, methods, types, constants, and variables.

###### Tip

A package can include test source files that use the `_test` suffix for the package declaration alongside test source files that use the package name. This allows you to organize your tests for exported and unexported functions into different files.

## Tests and the Context

When writing tests for functions that expect a context (as covered in Chapter 14), don’t use `context.Background` to generate a context. Instead, use `t.Context`. This returns a context that is automatically canceled when the test exits (if you registered any functions with `t.Cleanup`, the context is cancelled just before they run). Using the test context allows any code under test to properly shut down. This includes goroutines that exit upon context cancellation. (In [“Quickly and Accurately Test Concurrent Code with `synctest`”](#ch02.html#synctest), you will learn how to find goroutines that don’t shut down when the context is canceled.) Sample code that demonstrates the test context is in the *sample\_code/oracle* directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/oracle):

```
func TestOracle(t *testing.T) {
    ctx := t.Context()
    ch := oracle.Launch(ctx)
    outCh := make(chan string)
    ch <- oracle.Request{
        Query:    "Is the sky blue?",
        Response: outCh,
    }
    result := <-outCh
    if result != "Is the sky blue? Yes!" {
        t.Error("the oracle is wearing rose-tinted glasses")
    }
}
```

## Using go-cmp to Compare Test Results

Writing a thorough comparison of a compound type’s two instances can be verbose. While you can use `reflect.DeepEqual` to compare structs, maps, and slices, there’s a better way. Back in 2017, Google released a third-party module called [`go-cmp`](https://oreil.ly/9bWJf) that does the comparison for you and returns a detailed description of what does not match. Let’s see how it works by defining a simple `struct` and a factory function that populates it. You can find this code in the *sample\_code/cmp* directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/cmp):

```
type Person struct {
    Name      string
    Age       int
    DateAdded time.Time
}

func CreatePerson(name string, age int) Person {
    return Person{
        Name:      name,
        Age:       age,
        DateAdded: time.Now(),
    }
}
```

In your test file, import `github.com/google/go-cmp/cmp`. The test function looks like this:

```
func TestCreatePerson(t *testing.T) {
    expected := Person{
        Name: "Dennis",
        Age:  37,
    }
    result := CreatePerson("Dennis", 37)
    if diff := cmp.Diff(expected, result); diff != "" {
        t.Error(diff)
    }
}
```

The `cmp.Diff` function takes in the expected output and the output that was returned by the function that you’re testing. It returns a string that describes any mismatches between the two inputs. If the inputs match, it returns an empty string. You assign the output of the `cmp.Diff` function to a variable called `diff` and then check whether `diff` is an empty string. If it is not, an error occurred.

When you build and run the test, you’ll see the output that `go-cmp` generates when two struct instances don’t match:

```
$ go test
--- FAIL: TestCreatePerson (0.00s)
    ch13_cmp_test.go:16:   ch13_cmp.Person{
              Name:      "Dennis",
              Age:       37,
        -     DateAdded: s"0001-01-01 00:00:00 +0000 UTC",
        +     DateAdded: s"2020-03-01 22:53:58.087229 -0500 EST m=+0.001242842",
          }

FAIL
FAIL    ch13_cmp    0.006s
```

The lines with a `-` and `+` indicate the fields whose values differ. The test failed because the dates didn’t match. This is a problem because you can’t control what date the `CreatePerson` function assigns. You have to ignore the `DateAdded` field. The `go-cmp` module includes a helper package, `cmpopts`, which contains functions that provide additional control over the comparisons performed by `cmp.Diff`. In this case, use `cmpopts.IgnoreFields` to (unsurprisingly) ignore a field:

```
if diff := cmp.Diff(expected, result, cmpopts.IgnoreFields(Person{}, "DateAdded")); diff != "" {
    t.Error(diff)
}
```

This is only a quick preview of the most useful features in `go-cmp`. Check its [documentation](https://oreil.ly/rmiWO) to learn more about how to control what is compared and the output format.

# Running Table Tests

Most of the time, it takes more than a single test case to validate that a function is working correctly. You could write multiple test functions to validate your function or multiple tests within the same function, but you’ll find that a great deal of the testing logic is repetitive. You set up supporting data and functions, specify inputs, check the outputs, and compare to see if they match your expectations.

Rather than writing this over and over, you can take advantage of a pattern called *table tests*. Let’s take a look at a sample. You can find this code in the *sample\_code/table* directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/table). Assume you have the following function in the `table` package:

```
func DoMath(num1, num2 int, op string) (int, error) {
    switch op {
    case "+":
        return num1 + num2, nil
    case "-":
        return num1 - num2, nil
    case "*":
        return num1 + num2, nil
    case "/":
        if num2 == 0 {
            return 0, ErrDivZero
        }
        return num1 / num2, nil
    default:
        return 0, &UnknownOpErr{Op: op}
    }
}
```

To test this function, you need to check the different branches, trying out inputs that return valid results, as well as inputs that trigger errors. You could write code like this, but it would be very repetitive:

```
func TestDoMath(t *testing.T) {
    result, err := DoMath(2, 2, "+")
    if result != 4 {
        t.Error("Should have been 4, got", result)
    }
    if err != nil {
        t.Error("Should have been nil error, got", err)
    }
    result2, err2 := DoMath(2, 2, "-")
    if result2 != 0 {
        t.Error("Should have been 0, got", result2)
    }
    if err2 != nil {
        t.Error("Should have been nil error, got", err2)
    }
    // and so on...
}
```

Let’s replace this repetition with a table test. First, declare a slice of anonymous structs. The struct contains fields for the name of the test, the input parameters, and the return values. Each entry in the slice represents another test:

```
data := []struct {
    name     string
    num1     int
    num2     int
    op       string
    expected int
    err      error
}{
    {"addition", 2, 2, "+", 4, nil},
    {"subtraction", 2, 2, "-", 0, nil},
    {"multiplication", 2, 2, "*", 4, nil},
    {"division", 2, 2, "/", 1, nil},
    {"bad_division", 2, 0, "/", 0, ErrDivZero},
}
```

Next, loop over each test case in `data`, invoking the `Run` method each time. This is the line that does the magic. You pass two parameters to `Run`, a name for the subtest and a function with a single parameter of type `*testing.T`. Inside the function, you call `DoMath` by using the fields of the current entry in `data`, using the same logic over and over. When you run these tests, you’ll see that not only do they pass, but when you use the `-v` flag, each subtest also now has a name:

```
for _, d := range data {
    t.Run(d.name, func(t *testing.T) {
        result, err := DoMath(d.num1, d.num2, d.op)
        if result != d.expected {
            t.Errorf("Expected %d, got %d", d.expected, result)
        }
        if !errors.Is(err, d.err) {
            t.Errorf("expected %v, got %v", d.err, err)
        }
    })
}
```

###### Tip

Comparing error messages in tests is fragile, because there may not be any compatibility guarantees on the message text. If you are testing code that is using `errors.New` and `fmt.Errorf` to make errors, you have two options: rewrite your code to use custom error types and sentinel errors, or compare the messages and hope for the best. It’s better to re-write your code.

# Running Tests Concurrently

By default, unit tests are run sequentially. Since each unit test is supposed to be independent from every other unit test, they make ideal candidates for concurrency. To make a unit test run concurrently with other tests, call the `Parallel` method on `*testing.T` as the first line in your test:

```
func TestMyCode(t *testing.T) {
    t.Parallel()
    // rest of test goes here
}
```

Parallel tests run concurrently with other tests marked as parallel. You can also call `Parallel` from within a table test:

```
func TestParallelTable(t *testing.T) {
    data := []struct {
        name   string
        input  int
        output int
    }{
        {"a", 10, 20},
        {"b", 30, 40},
        {"c", 50, 60},
    }
    for _, d := range data {
        t.Run(d.name, func(t *testing.T) {
            t.Parallel()
            t.Log(d.input, d.output)
            out := toTest(d.input)
            if out != d.output {
                t.Error("didn't match", out, d.output)
            }
        })
    }
}
```

You can run this code on [The Go Playground](https://oreil.ly/b0S4n) or in the *sample\_code/parallel* directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/parallel).

The advantage of parallel tests is they can speed up long-running test suites. There are some disadvantages, though. If you have multiple tests that rely on the same shared mutable state, do not mark them as parallel, because you will get inconsistent results. (But after all of my warnings, you don’t have any shared mutable state in your application, right?) Also be aware that your test will panic if you mark it as parallel and use the `Setenv` method in your test function.

###### Note

Go 1.22 (released in February, 2024) changed the lifecycle of the index and value variables in `for` loops. The compiler now creates a new copy of the index and value variables on each iteration if there’s any chance they could be captured by a closure. This eliminated bugs in parallel tests that were using shared index and value state. If you are still using Go 1.21 or earlier (or have the Go version in the `go` directive in your *go.mod* file set to 1.2.1 or earlier), I strongly recommend upgrading to take advantage of this improved behavior.

Now that you have a way to run lots of tests, you’ll learn about code coverage to find out what your tests are testing.

# Exercises

These exercises help you practice the basics of Go testing. They are found in the *exercises* directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/exercises). Solutions are in the *exercise-solutions* directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/exercise-solutions).

1.  In the *ex1* [directory](https://github.com/learning-go-book-3e/ch15/tree/main/exercises/ex1), there is a file called *calculator.go*, with a function called `Average`. Create a *calculator\_test.go* file with a function to test `Average`. If you find a bug in the function, fix the bug and confirm that the test passes.
    
2.  In the *ex2* [directory](https://github.com/learning-go-book-3e/ch15/tree/main/exercises/ex2), there is a file called *temperature.go* with a function called `Convert`. Create a *temperature\_test.go* file with a function that uses a table test to validate all inputs and possible errors. If you find a bug in the function, fix the bug and confirm that the test passes.
    
3.  In the *ex3* [directory](https://github.com/learning-go-book-3e/ch15/tree/main/exercises/ex3), there is a file called *counter.go* with a function called `TotalFile`. Create a *counter\_test.go* file with a function to test `TotalFile`. Use a helper function to create both good and bad sample data.
    

# Checking Your Code Coverage

Code coverage is a very useful tool for knowing whether you’ve missed any obvious cases. However, reaching 100% code coverage doesn’t guarantee that there aren’t bugs in your code for some inputs. First you’ll see how `go test` displays code coverage and then you’ll look at the limitations of relying on code coverage alone.

Adding the `-cover` flag to the `go test` command calculates coverage information and includes a summary in the test output. If you include a second flag `-coverprofile`, you can save the coverage information to a file. Let’s go back to the *sample\_code/table* directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/table) and gather code coverage information:

```
$ go test -v -cover -coverprofile=c.out
```

If you run your table test with code coverage, the test output now includes a line that indicates the amount of test code coverage, 87.5%. That’s good to know, but it’d be more useful if you could see what you missed. The `cover` tool included with Go generates an HTML representation of your source code with that information:

```
$ go tool cover -html=c.out
```

When you run it, your web browser opens and shows you a page with code coverage information using a dark mode color scheme. Every file that’s tested appears in the combo box in the upper left. The source code is in one of three colors. Gray is used for lines of code that aren’t testable, green is used for code that’s been covered by a test, and red is used for code that hasn’t been tested. This reliance on color is unfortunate for those who have red-green color blindness (and those whose publishers don’t want to print large blocks of black ink). To address this issue, I have created a [fork of the cover tool](https://github.com/jonbodner/coverlight). Install and run it with:

```
$ go install github.com/jonbodner/coverlight@latest
$ coverlight -html c.out
```

The page that opens should look similar to [Figure 2-1](#ch02.html#initial_code_coverage). Code with test coverage is displayed in black, code without test coverage is displayed as white text with a black background, and gray is used for untestable lines of code. From looking at this, you can see that you didn’t write a test to cover the default case, when a bad operator is passed to your function. Add that case to your slice of test cases:

```
{"bad_op", 2, 2, "?", 0, &UnknownOpErr{Op: "?"}},
```

When you rerun `go test -v -cover -coverprofile=c.out` and `coverlight -html c.out`, you see in [Figure 2-2](#ch02.html#final_code_coverage) that the final line in `DoMath` is covered. There is still missing code coverage for the methods for `UnknownOppErr`. I’ll leave it as an exercise for the reader to write an additional test to get 100% test code coverage.

![](assets/lgo3_1501.png)![](assets/lgo3_1502.png)

Code coverage is a great thing, but it’s not enough. There’s actually a bug in your code, even though you have 100% coverage. Have you noticed it? If not, add another test case and rerun your tests:

```
{"another_mult", 2, 3, "*", 6, nil},
```

You should see the error:

```
table_test.go:60: Expected 6, got 5
```

Your case for multiplication has a typo. It adds the numbers together instead of multiplying them. (Beware the dangers of copy-and-paste coding!) Fix the code, rerun `go test -v -cover -coverprofile=c.out` and `go tool cover -html=c.out`, and you’ll see that the tests pass again.

###### Warning

Code coverage is necessary but not sufficient. You can have 100% code coverage and still have bugs in your code!

# Fuzzing

One of the most important lessons that every developer eventually learns is that all data is suspect. No matter how well specified a data format is, you will eventually have to process input that doesn’t match what you expect. This doesn’t happen for only malicious reasons. Data can be corrupted in transit, in storage, and even in memory. Programs that process data might have bugs, and specifications for data formats always have corner cases that developers interpret in different ways.

Even when developers do the work of writing good unit tests, it’s impossible to think of everything. As you’ve seen, even 100% unit test code coverage is no guarantee that your code is bug free. You need to supplement unit tests with generated data that could break your program in ways you didn’t anticipate. That’s where fuzzing helps.

*Fuzzing* is a technique for generating random data and submitting it to code to see whether it properly handles unexpected input. The developer can provide a *seed corpus* or set of known good data, and the fuzzer uses that as a basis for generating bad input. Let’s see how to use the fuzzing support in Go’s testing tools to discover additional test cases.

Assume that you are writing a program to process data files. You can find the code for this example [on GitHub](https://github.com/learning-go-book-3e/file_parser). You are sending a list of strings but want to efficiently allocate memory, so the number of strings in a file is sent as the first line, while the remaining lines are the lines of text. Here’s the sample function for processing this data:

```
func ParseData(r io.Reader) ([]string, error) {
    s := bufio.NewScanner(r)
    if !s.Scan() {
        return nil, ErrEmpty
    }
    countStr := s.Text()
    count, err := strconv.Atoi(countStr)
    if err != nil {
        return nil, &ParseError{CountStr: countStr}
    }
    out := make([]string, 0, count)
    for range count {
        hasLine := s.Scan()
        if !hasLine {
            return nil, ErrTooFewLines
        }
        line := s.Text()
        out = append(out, line)
    }
    return out, nil
}
```

You use a `bufio.Scanner` to read line-by-line from an `io.Reader`. If there’s no data to be read, an error is returned. You then read the first line and attempt to convert it to an int named `count`. If you can’t, an error is returned. Next, you allocate the memory for your slice of strings, and read `count` number of lines from the scanner. If there aren’t enough lines, an error is returned. If all goes well, you return the lines you read.

A unit test has already been written to validate the code:

```
func TestParseData(t *testing.T) {
    data := []struct {
        name string
        in   []byte
        out  []string
        err  error
    }{
        {
            name: "simple",
            in:   []byte("3\nhello\ngoodbye\ngreetings\n"),
            out:  []string{"hello", "goodbye", "greetings"},
            err:  nil,
        },
        {
            name: "empty_error",
            in:   []byte(""),
            out:  nil,
            err:  ErrEmpty,
        },
        {
            name: "zero",
            in:   []byte("0\n"),
            out:  []string{},
            err:  nil,
        },
        {
            name: "number_error",
            in:   []byte("asdf\nhello\ngoodbye\ngreetings\n"),
            out:  nil,
            err:  &ParseError{CountStr: "asdf"},
        },
        {
            name: "line_count_error",
            in:   []byte("4\nhello\ngoodbye\ngreetings\n"),
            out:  nil,
            err:  ErrTooFewLines,
        },
    }
    for _, d := range data {
        t.Run(d.name, func(t *testing.T) {
            r := bytes.NewReader(d.in)
            out, err := ParseData(r)
            if !errors.Is(err, d.err) {
                t.Errorf("expected %v, got %v", d.err, err)
            }
            if diff := cmp.Diff(d.out, out); diff != "" {
                t.Error(diff)
            }
        })
    }
}
```

The unit test has 100% line coverage for `ParseData`, handling all its error cases. You might think the code is ready for production, but let’s see if fuzzing can help you discover errors that you hadn’t considered.

###### Note

Be aware that fuzzing uses a lot of resources. A fuzz test can allocate (or attempt to allocate) many gigabytes of memory and might write several gigabytes of data to your local disk. If you are running something else on the same machine at the same time as a fuzz test, don’t be surprised if it slows down.

Start by writing a fuzz test:

```
func FuzzParseData(f *testing.F) {
    testcases := [][]byte{
        []byte("3\nhello\ngoodbye\ngreetings\n"),
        []byte("0\n"),
    }
    for _, tc := range testcases {
        f.Add(tc) // Use f.Add to provide a seed corpus
    }
    f.Fuzz(func(t *testing.T, in []byte) {
        r := bytes.NewReader(in)
        out, err := ParseData(r)
        if err != nil {
            t.Skip("handled error")
        }
        roundTrip := ToData(out)
        rtr := bytes.NewReader(roundTrip)
        out2, _ := ParseData(rtr)
        if diff := cmp.Diff(out, out2); diff != "" {
            t.Error(diff)
        }
    })
}
```

A fuzz test looks similar to a standard unit test. The function name starts with `Fuzz`, the only parameter is of type `*testing.F`, and it has no return values.

Next, set up a seed corpus, which is composed of one or more sets of sample data. The data can run successfully, error out, or even panic. The important things are that you know how your program behaves when this data is provided and that this behavior is accounted for by your fuzz test. These samples are mutated by the fuzzer to generate bad inputs. This example uses only a single field of data for each entry (a slice of bytes), but you can have as many fields as needed. The fields in a corpus entry are currently limited to only certain types:

*   Any integer type (including unsigned types, `rune`, and `byte`)
    
*   Any floating-point type
    
*   `bool`
    
*   `string`
    
*   `[]byte`
    

Each entry in the corpus is passed to the `Add` method on the `*testing.F` instance. In the example, you have a slice of bytes for each entry:

```
f.Add(tc)
```

If the function being fuzzed needed an `int` and a `string`, the call to `Add` would look like this:

```
f.Add(1, "some text")
```

It’s a runtime error to pass a value of an invalid type to `Add`.

Next, call the `Fuzz` method on your `*testing.F` instance. This looks a bit like a call to `Run` when writing a table test in a standard unit test. `Fuzz` takes in a single parameter, a function whose first parameter is of type `*testing.T` and whose remaining parameters exactly match the types, order, and count of the values that were passed in to `Add`. This also specifies the type of data generated by the fuzzing engine during fuzzing. There’s no way for the Go compiler to enforce this constraint, so it’s a runtime error if the convention is not followed.

Finally, let’s look at the body of the fuzz target. Remember, fuzzing is used to find cases where bad input is not handled correctly. Since the input is randomly generated, you can’t write tests that have knowledge of what the output should be. Instead, you have to use test conditions that will be true for all inputs. In the case of `ParseData`, you can check for two things:

*   Does the code return an error for bad input, or does it panic?
    
*   If you convert the slice of strings back to a slice of bytes and re-parse it, do you get the same result?
    

Let’s see what happens when you run the fuzz test:

```
$ go test -fuzz=FuzzParseData
fuzz: elapsed: 0s, gathering baseline coverage: 0/137 completed
failure while testing seed corpus entry: FuzzParseData/03f81b404ad91d09
fuzz: elapsed: 3s, gathering baseline coverage: 2/137 completed
--- FAIL: FuzzParseData (2.92s)
    --- FAIL: FuzzParseData (0.00s)
        testing.go:1927: panic: runtime error: makeslice: cap out of range
            goroutine 12 [running]:
            runtime/debug.Stack()
                /opt/homebrew/opt/go/libexec/src/runtime/debug/stack.go:26 +0xc4
            testing.tRunner.func1()
                /opt/homebrew/opt/go/libexec/src/testing/testing.go:1927 +0x218
            panic({0x100ca49c0?, 0x100ce5dc0?})
                /opt/homebrew/opt/go/libexec/src/runtime/panic.go:860 +0x12c
            file_parser.ParseData({0x100ce7a40?, 0x10e411dfcf30?})
                file_parser/file_parser.go:46 +0x134
```

If you don’t specify the `-fuzz` flag, your fuzz tests will be treated like unit tests and will be run against the seed corpus. Only a single fuzz test can be fuzzed at a time.

###### Note

If you want to get the full experience, delete the contents of the *testdata/fuzz/FuzzParseData* directory. This will cause the fuzzer to generate new seed corpus entries. Since the fuzzer generates random input, your samples might be different from the ones shown. The different entries will likely produce similar errors, though maybe not in the same order.

The fuzz test runs and in less than a second, it reports a panic. Panics are bad, so let’s look at the input that was generated. Every time a test case fails, the fuzzer writes it to the *testdata/fuzz/TESTNAME* subdirectory in the same package as the failed test, adding a new entry to the seed corpus. This automatically generated unit test runs when `go test -fuzz=FuzzParseData` is called, and acts as a regression test once you fix your bug.

Here are the contents of the file:

```
go test fuzz v1
[]byte("-1")
```

The first line is a header to indicate that this is test data for a fuzz test. The subsequent lines have the data that caused the failure.

Passing the name of the file in the failure message as a `-run` parameter to `go test` allows you to isolate this failing case when you rerun the test:

```
$ go test -run=FuzzParseData/03f81b404ad91d09
--- FAIL: FuzzParseData (0.00s)
    --- FAIL: FuzzParseData/03f81b404ad91d09 (0.00s)
panic: runtime error: makeslice: cap out of range [recovered, repanicked]
```

The line that’s generating the panic is shown here:

```
out := make([]string, 0, count)
```

You are trying to create a slice with negative capacity, which panics. An error type for this situation has been handily defined for you. Add a condition to your code to look for negative numbers before you call `make`:

```
if count < 0 {
    return nil, ErrNegative
}
```

If you run `go test -run=FuzzParseData/03f81b404ad91d09`, you’ll see that the test now passes. Run the fuzzer again to see whether it finds any more errors:

```
$ go test -fuzz=FuzzParseData
fuzz: elapsed: 0s, gathering baseline coverage: 0/137 completed
failure while testing seed corpus entry: FuzzParseData/0e4ad8073b1a5373
fuzz: elapsed: 3s, gathering baseline coverage: 2/137 completed
--- FAIL: FuzzParseData (2.95s)
    --- FAIL: FuzzParseData (0.00s)
        file_parser_test.go:87:   []string{
              ... // 3 identical elements
              "",
              "",
            -  "\r",
            +  "",
              "0",
            }
```

This time, the input file created by the fuzzer looks like this:

```
go test fuzz v1
[]byte("7\n\n\n\n\n\n\r\r\n0")
```

The fuzz test is generating blank lines with `\r` (return) characters. Blank lines are not expected in your input, so add some code to the loop that reads lines of text from the `Scanner`. You’ll check whether a line consists only of whitespace characters. If it does, return an error:

```
line = strings.TrimSpace(line)
if len(line) == 0 {
    return nil, ErrBlankLine
}
```

Validate that you fixed the bug with `go test -run=FuzzParseData/0e4ad8073b1a5373`, then run the fuzzer again to see whether it finds any more errors:

```
$ go test -fuzz=FuzzParseData
fuzz: elapsed: 0s, gathering baseline coverage: 0/137 completed
fuzz: elapsed: 3s, gathering baseline coverage: 136/137 completed
fuzz: elapsed: 6s, gathering baseline coverage: 136/137 completed
fuzz: elapsed: 9s, gathering baseline coverage: 136/137 completed
failure while testing seed corpus entry: FuzzParseData/e8bf251ff804c663
fuzz: elapsed: 11s, gathering baseline coverage: 136/137 completed
--- FAIL: FuzzParseData (10.83s)
    fuzzing process hung or terminated unexpectedly: exit status 2
```

In this case, the `go` command reports that your program has “hung or terminated unexpectedly”. We don’t want programs to hang or terminate, expectedly or unexpectedly, so let’s look at the generated test case to figure out what went wrong:

```
go test fuzz v1
[]byte("3000000000000")
```

The problem is that you are trying to allocate a slice with the capacity to hold 3 trillion strings. That requires quite a bit more RAM than my computer (and probably yours) has. You need to limit the number of expected text elements to a reasonable number. Use the sentinel error `ErrTooManyLines` and set the maximum number of rows to 1,000 by adding the following code to `ParseData` after you parse the number of expected rows:

```
if count > 1000 {
    return nil, ErrTooManyLines
}
```

Validate that the bug is fixed with `go test -run=FuzzParseData/e8bf251ff804c663`, and run the fuzzer yet again:

```
$ go test -fuzz=FuzzParseData
fuzz: elapsed: 0s, gathering baseline coverage: 0/147 completed
fuzz: elapsed: 0s, gathering baseline coverage: 147/147 completed,
    now fuzzing with 16 workers
fuzz: elapsed: 3s, execs: 1568753 (522795/sec), new interesting: 1 (total: 148)
fuzz: elapsed: 6s, execs: 3134921 (522087/sec), new interesting: 1 (total: 148)
fuzz: elapsed: 9s, execs: 4652294 (505854/sec), new interesting: 1 (total: 148)
...
fuzz: elapsed: 2m9s, execs: 59383661 (416674/sec), new interesting: 2 (total: 149)
fuzz: elapsed: 2m12s, execs: 60694659 (436957/sec), new interesting: 3 (total: 150)
fuzz: elapsed: 2m15s, execs: 61976072 (427059/sec), new interesting: 3 (total: 150)
^C
PASS
```

After a few minutes, no more errors are found, so hit Ctrl-C to end fuzzing.

Just because the fuzzer didn’t find additional issues doesn’t mean that the code is now bug free. However, fuzzing allowed you to automatically find some significant oversights in the original code. Writing fuzz tests takes practice, as they require a slightly different mindset than writing unit tests. Once you get the hang of them, they become an essential tool for validating how your code handles unexpected user input.

# Using Benchmarks

Determining how fast (or slow) code runs is surprisingly difficult. Rather than trying to figure it out yourself, use the benchmarking support that’s built into Go’s testing framework. Let’s explore it with a function in the *sample\_code/bench* directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/bench):

```
func FileLen(f string, bufsize int) (int, error) {
    file, err := os.Open(f)
    if err != nil {
        return 0, err
    }
    defer file.Close()
    count := 0
    for {
        buf := make([]byte, bufsize)
        num, err := file.Read(buf)
        count += num
        if err != nil {
            break
        }
    }
    return count, nil
}
```

This function counts the number of characters in a file. It takes in two parameters, the name of the file and the size of the buffer that you are using to read the file (you’ll see the reason for the second parameter in a moment).

Before seeing how fast it is, test your library to make sure that it works (it does). Here’s a simple test:

```
func TestFileLen(t *testing.T) {
    result, err := FileLen("testdata/data.txt", 1)
    if err != nil {
        t.Fatal(err)
    }
    if result != 65204 {
        t.Error("Expected 65204, got", result)
    }
}
```

Now you can see how long it takes your file-length function to run. Your goal is to find out what size buffer you should use to read from the file.

###### Note

Before you spend time going down an optimization rabbit hole, be sure that you need to optimize. If your program is already fast enough to meet your responsiveness requirements and is using an acceptable amount of memory, your time is better spent on adding features and fixing bugs. Your business requirements determine what “fast enough” and “acceptable amount of memory” mean.

In Go, *benchmarks* are functions in your test files that start with the word `Benchmark` and take in a single parameter of type `*testing.B`. This type includes all the functionality of a `*testing.T` as well as additional support for benchmarking. Let’s start by looking at a benchmark that uses a buffer size of 1 byte:

```
func BenchmarkFileLen1(b *testing.B) {
    for b.Loop() {
        _, err := FileLen("testdata/data.txt", 1)
        if err != nil {
            b.Fatal(err)
        }
    }
}
```

Go 1.24 introduced the `Loop` method on `*testing.B`. It manages two things that you had to do by hand in earlier versions of Go. The first is looping the proper number of times. The testing framework calls your benchmark functions over and over with larger and larger values until it is sure that the timing results are accurate. Using `b.Loop` ensures this is done properly.

The other thing the `b.Loop` method does is it prevents optimization. Compilers are pretty smart. They can automatically remove calculations or function calls whose values are never used. This is fantastic when running a program in production, but it’s not so great if you are calling a function in a benchmark to see how fast it is and don’t care about the result. The compiler might remove the function call from the benchmark, which is not helpful.

In order to address this, the Go compiler has a special rule when it sees that it is compiling a benchmark with a `for b.Loop` loop. In that one case, it makes sure that nothing in the body of the `for` loop is removed from the compiled code.

Using the `for b.Loop` pattern, the testing framework calls your benchmark functions repeatedly until it is sure that the timing results are accurate. You’ll see this in the output in a moment.

You run a benchmark by passing the `-bench` flag to `go test`. This flag expects a regular expression to describe the name of the benchmarks to run. Use `-bench=.` to run all benchmarks. A second flag, `-benchmem`, includes memory allocation information in the benchmark output. All tests are run before the benchmarks, so you can benchmark code only when tests pass.

Here’s the output for the benchmark on my computer:

```
BenchmarkFileLen1-16  44 24379298 ns/op  128 B/op  3 allocs/op
```

Running a benchmark with memory allocation information produces output with five columns. Here’s what each one means:

BenchmarkFileLen1-16
: 

The name of the benchmark, a hyphen, and the value of `GOMAXPROCS` for the benchmark.

44
: 

The number of times that the test ran to produce a stable result.

24379298 ns/op
: 

The amount of time it took to run a single pass of this benchmark, in nanoseconds (there are a billion nanoseconds in a second).

128 B/op
: 

The number of bytes allocated during a single pass of the benchmark.

3 allocs/op
: 

The number of times bytes had to be allocated from the heap during a single pass of the benchmark. This will always be less than or equal to the number of bytes allocated.

As I covered in the Memory section, we care about the number of bytes allocated on the heap and the number of times that our program allocates from the heap, because heap allocations create more work for the garbage collector. The more time your program spends garbage collecting, the less time it spends doing the work you wrote it to do.

Now that you have results for a buffer of 1 byte, let’s see what the results look like when you use buffers of different sizes:

```
func BenchmarkFileLen(b *testing.B) {
    for _, v := range []int{1, 10, 100, 1000, 10000, 100000} {
        b.Run(fmt.Sprintf("FileLen-%d", v), func(b *testing.B) {
            for b.Loop() {
                _, err := FileLen("testdata/data.txt", v)
                if err != nil {
                    b.Fatal(err)
                }
            }
        })
    }
}
```

Just as you launched table tests using `t.Run`, you’re using `b.Run` to launch benchmarks that vary based only on input. Here are the results of this benchmark on my computer:

```
BenchmarkFileLen/FileLen-1-16          49  24230445 ns/op     128 B/op    3 allocs/op
BenchmarkFileLen/FileLen-10-16        489   2458019 ns/op     128 B/op    3 allocs/op
BenchmarkFileLen/FileLen-100-16      4214    269902 ns/op   73376 B/op  657 allocs/op
BenchmarkFileLen/FileLen-1000-16    29570     40574 ns/op   68736 B/op   70 allocs/op
BenchmarkFileLen/FileLen-10000-16   72368     16581 ns/op   82048 B/op   11 allocs/op
BenchmarkFileLen/FileLen-100000-16  62640     19032 ns/op  213121 B/op    5 allocs/op
```

Parts of these results are expected and parts are surprising. As you increase the size of the buffer, it takes fewer loops to read all the data and your code runs faster, until the buffer is bigger than the file. When the buffer is bigger than the size of the file, extra allocations slow the output. If you expect files of roughly this size, a buffer of 10,000 bytes works best.

In the Slice Stack section, I mentioned that slices can sometimes be allocated on the stack instead of the heap; this benchmark provides a demonstration. The 1 byte and 10 byte buffers uses little heap memory because they are allocated on the stack. The `benchmem` flag only shows expensive heap allocations, not cheap stack allocations. For larger buffers, the heap is used.

But you can make a change that improves the numbers more. You are reallocating the buffer every time you get the next set of bytes from the file. That’s unnecessary. If you move the byte slice allocation before the loop and rerun your benchmark, you see an improvement:

```
BenchmarkFileLen/FileLen-1-16          48  24040199 ns/op     128 B/op  3 allocs/op
BenchmarkFileLen/FileLen-10-16        490   2435033 ns/op     128 B/op  3 allocs/op
BenchmarkFileLen/FileLen-100-16      4680    255315 ns/op     240 B/op  4 allocs/op
BenchmarkFileLen/FileLen-1000-16    34837     34776 ns/op    1152 B/op  4 allocs/op
BenchmarkFileLen/FileLen-10000-16   93732     12751 ns/op   10368 B/op  4 allocs/op
BenchmarkFileLen/FileLen-100000-16  84337     14236 ns/op  106624 B/op  4 allocs/op
```

The number of allocations is now consistent and small: just three or four for every buffer size. What is interesting is that you now can make trade-offs. If you are tight on memory, you can use a smaller buffer size and save memory at the expense of performance.

If benchmarking reveals that you have a performance or memory problem, the next step is figuring out exactly what the problem is. Go includes profiling tools that gather CPU and memory usage data from a running program as well as tools that help you visualize and interpret the generated data. You can even expose a web service endpoint to gather profiling information remotely from a running Go service. This data can even be used at compile time to improve performance automatically. I talk about instrumentating code and understanding profile data in Chapter 17>>.

## benchstat

How do you keep track of performance changes between versions of code and how do you know that the differences are significant? You could eyeball the output and try to remember, but it’s better to use `benchstat`, a tool built by the Go team that automates the process. Add `benchstat` to your Go module using:

```
go get -tool golang.org/x/perf/cmd/benchstat
```

When capturing benchmarks for `benchstat`, you need to run them multiple times and save the output to disk. The `-count` flag controls the number of times each benchmark runs; set it to 10 by default:

```
go test -bench=BenchmarkFileLen$ -count=10 -benchmem > base.txt
# move the allocation from inside the for loop to before the for loop
go test -bench=BenchmarkFileLen$ -count=10 -benchmem > new.txt
```

Compare the runs using `go tool benchstat base.txt new.txt`. Here are the results I obtained by running `benchstat` for benchmarks on last two versions of the `FileLen` function:

```
$ go tool benchstat base.txt new.txt
goos: darwin
goarch: arm64
pkg: github.com/learning-go-book-3e/ch15/sample_code/bench
cpu: Apple M3 Max
                          │  base.txt   │               new.txt               │
                          │   sec/op    │   sec/op     vs base                │
FileLen/FileLen-1-16        23.83m ± 1%   23.80m ± 0%        ~ (p=0.796 n=10)
FileLen/FileLen-10-16       2.433m ± 1%   2.412m ± 0%   -0.85% (p=0.000 n=10)
FileLen/FileLen-100-16      263.3µ ± 2%   249.4µ ± 1%   -5.31% (p=0.000 n=10)
FileLen/FileLen-1000-16     38.93µ ± 1%   33.80µ ± 0%  -13.18% (p=0.000 n=10)
FileLen/FileLen-10000-16    16.13µ ± 2%   12.69µ ± 2%  -21.35% (p=0.000 n=10)
FileLen/FileLen-100000-16   19.02µ ± 2%   14.15µ ± 1%  -25.60% (p=0.000 n=10)
geomean                     238.1µ        210.5µ       -11.63%

                          │   base.txt    │                new.txt                 │
                          │     B/op      │     B/op      vs base                  │
FileLen/FileLen-1-16           128.0 ± 0%     128.0 ± 0%        ~ (p=1.000 n=10) ¹
FileLen/FileLen-10-16          128.0 ± 0%     128.0 ± 0%        ~ (p=1.000 n=10) ¹
FileLen/FileLen-100-16       73040.0 ± 0%     240.0 ± 0%  -99.67% (p=0.000 n=10)
FileLen/FileLen-1000-16     66.125Ki ± 0%   1.125Ki ± 0%  -98.30% (p=0.000 n=10)
FileLen/FileLen-10000-16     80.12Ki ± 0%   10.12Ki ± 0%  -87.36% (p=0.000 n=10)
FileLen/FileLen-100000-16    208.1Ki ± 0%   104.1Ki ± 0%  -49.97% (p=0.000 n=10)
geomean                      10.35Ki        1.277Ki       -87.66%
¹ all samples are equal

                          │   base.txt   │               new.txt                │
                          │  allocs/op   │ allocs/op   vs base                  │
FileLen/FileLen-1-16          3.000 ± 0%   3.000 ± 0%        ~ (p=1.000 n=10) ¹
FileLen/FileLen-10-16         3.000 ± 0%   3.000 ± 0%        ~ (p=1.000 n=10) ¹
FileLen/FileLen-100-16      654.000 ± 0%   4.000 ± 0%  -99.39% (p=0.000 n=10)
FileLen/FileLen-1000-16      69.000 ± 0%   4.000 ± 0%  -94.20% (p=0.000 n=10)
FileLen/FileLen-10000-16     11.000 ± 0%   4.000 ± 0%  -63.64% (p=0.000 n=10)
FileLen/FileLen-100000-16     5.000 ± 0%   4.000 ± 0%  -20.00% (p=0.000 n=10)
geomean                       16.78        3.634       -78.34%
¹ all samples are equal
```

The first table shows the difference in CPU time, the second the difference in number of bytes allocated on the heap, and the third the difference in number of times the memory had to be allocated from the heap. Each of the numbers is given a variance range (such as ± 1%); lower is better. The p value ranges from 0 to 1, telling you the likelihood that the comparison is valid. Zero means you can trust the result, one means you can’t, and a number inbetween indicates the degree of trust. If the difference between the base case and the new case is not statistically significant, you will see a high value for p; feel free to ignore it. In other cases, a p value less than 0.05 is ideal.

If the variance or the p value is high, ensure that your computer wasn’t busy running other tasks while you were collecting benchmark data. You can also use the `-count` flag to increase the number of benchmark runs or the `-benchtime` flag to force each benchmark to run for a set number of seconds (`-benchtime=2s` for a two second run) or to run a set number of times (`-benchtime=100000x` to force each benchmark to run 100,000 times).

# Finding Concurrent Data Access Problems with the Data Race Detector

Even with Go’s built-in support for concurrency, bugs still happen. It’s easy to accidentally reference a variable from two different goroutines without acquiring a lock. The computer science term for this is a *data race*. To help find these sorts of bugs, Go includes a *race checker*. It isn’t guaranteed to find every single data race in your code, but if it finds one, you should put proper locks around what it finds.

Look at a simple example in the file *sample\_code/race/race.go* in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/blob/main/sample_code/race/race.go):

```
func getCounter() int {
    var counter int
    var wg sync.WaitGroup
    for range 5 {
        wg.Go(func() {
            for range 1000 {
                counter++
            }
        })
    }
    wg.Wait()
    return counter
}
```

This code launches five goroutines, has each of them update a shared `counter` variable 1,000 times, and then returns the result. You’d expect it to be 5,000, so let’s verify this with a unit test in *sample\_code/race/race\_test.go*:

```
func TestGetCounter(t *testing.T) {
    counter := getCounter()
    if counter != 5000 {
        t.Error("unexpected counter:", counter)
    }
}
```

If you run `go test` a few times, you’ll see that sometimes it passes, but most of the time it fails with an error message like this:

```
unexpected counter: 3673
```

The problem is that there’s a data race in the code. In a program this simple, the cause is obvious: multiple goroutines are trying to update `counter` simultaneously, and some of their updates are lost. In more complicated programs, these sorts of races are harder to see. Let’s see what the data race detector does. Use the flag `-race` with `go test` to enable it:

```
$ go test -race
==================
WARNING: DATA RACE
Read at 0x00c000128070 by goroutine 10:
  test_examples/race.getCounter.func1()
      test_examples/race/race.go:12 +0x45

Previous write at 0x00c000128070 by goroutine 8:
  test_examples/race.getCounter.func1()
      test_examples/race/race.go:12 +0x5b
```

The traces make it clear that the line `counter++` is the source of your problems.

###### Warning

Some people try to fix race conditions by inserting “sleeps” into their code, trying to space out access to the variable that’s being accessed by multiple goroutines. *This is a bad idea*. Doing so might eliminate the problem in some cases, but the code is still wrong and will fail in some situations.

You can also use the `-race` flag when you build your programs. This creates a binary that includes the data race detector and that reports any races it finds to the console. This allows you to find data races in code that doesn’t have tests.

If the data race detector is so useful, why isn’t it enabled all the time for testing and production? A binary with `-race` enabled runs approximately 10 times slower than a normal binary. That isn’t a problem for test suites that take a second to run, but for large test suites that take several minutes, a 10× slowdown reduces productivity.

For more information on the data race detector, check out its [official documentation](https://oreil.ly/0uLcW).

# Quickly and Accurately Test Concurrent Code with `synctest`

You might be surprised to discover that a language with strong support for both testing and concurrency did not initially include strong support for testing concurrency. The release of Go 1.25 addressed this oversight with the `synctest` package. Let’s see what it can do.

There are three common situations that were awkward to test before the release of `synctest`. First, there’s code that depends on time passing. Say that you implement a simple cache with expiration. You can put values into it and read values from it. If a value isn’t read within the expiration period, it is no longer returned by the cache.

```
func (c *Cache[K, V]) Get(key K) (V, bool) {
    c.mu.Lock()
    defer c.mu.Unlock()
    e, ok := c.items[key]
    if !ok || time.Now().After(e.expiresAt) {
        delete(c.items, key)
        var zero V
        return zero, false
    }
    return e.value, true
}
```

This code gets a value from the cache. (The complete implementation is in the [cachev1 directory in the Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/synctest/cachev1)) It compares the time that an entry was placed into the cache against the current time. Being a good developer, you want to write a test to make sure this code functions correctly. Since the cache has different behavior depending on how much time has passed, you need to include `time.Sleep` calls to put pauses into reads and writes to the cache.

Here’s a test that puts a value into the cache, sleeps until the value expires, and then confirms that the value isn’t present:

```
func TestCache_GetAfterExpiry_standard(t *testing.T) {
    c := cachev1.New[string, int](1 * time.Second)
    c.Set("answer", 42)

    // Jump past the TTL.
    time.Sleep(1100 * time.Millisecond)

    _, ok := c.Get("answer")
    if ok {
        t.Fatal("expected key to be expired after TTL")
    }
}
```

If you run this test, it passes, but that call to `time.Sleep` means it takes about a second to run. It’s only one second, but properly testing this cache requires additional tests that also require `time.Sleep` calls. Other parts of your program are liable to have their own tests that require the passage of time and this adds up. Unit tests should execute quickly to prevent developers from being distracted. That’s where `synctest` is useful.

The `synctest` package defines two functions: `synctest.Test` and `synctest.Wait`. I’ll talk about `Test` first. It takes in two parameters, a `*testing.T` and a function that takes a `*testing.T` as an input parameter and has no output parameters. This looks a lot like the `t.Run` method used in [“Running Table Tests”](#ch02.html#running_table_tests). Like `t.Run`, `synctest.Test` creates a sub-environment. In the case of `synctest.Test`, this environment has magic powers. Each call to `synctest.Test` creates what the Go team calls a “bubble”. In this bubble, there is a fake clock, which starts at January 1st, 2000 UTC. Within the bubble, every `time.Sleep` call or use of a `time.Ticker` or `time.Timer` is tracked across all goroutines launched in the bubble. This allows the fake clock to move forward without waiting for the specified time to elapse in the real world.

An example makes this easier to understand. Say you have a test that looks like this:

```
func TestTimeWaiting(t *testing.T) {
    start := time.Now() // real wall clock time/date
    go func() {
        time.Sleep(1 * time.Second)
        t.Log(time.Since(start)) // always logs "1s"
        t.Log(time.Now())
    }()
    time.Sleep(2 * time.Second) // the goroutine above will run before this Sleep returns
    t.Log(time.Since(start))    // always logs "2s"
    t.Log(time.Now())
}
```

Running this test takes 2 seconds to complete. But if you use `synctest.Test`, the test completes instantly:

```
func TestTime(t *testing.T) {
    synctest.Test(t, func(t *testing.T) {
        start := time.Now() // always midnight UTC 2000-01-01
        go func() {
            time.Sleep(1 * time.Second)
            t.Log(time.Since(start)) // always logs "1s"
            t.Log(time.Now())
        }()
        time.Sleep(2 * time.Second) // the goroutine above will run before this Sleep returns
        t.Log(time.Since(start))    // always logs "2s"
        t.Log(time.Now())
    })
}
```

###### Note

Using `synctest` requires your module’s `go.mod` file to declare that you are using Go 1.25 or later. If you get an error like this:

`panic: synctest.Run not supported with asynctimerchan!=0`

Check your `go.mod` file and update the specified version of Go.

If we apply this pattern to the `TestCache_GetAfterExpiry_standard` function from earlier, running the test is now instanteous.

```
func TestCache_GetAfterExpiry_synctest(t *testing.T) {
    synctest.Test(t, func(t *testing.T) {
        c := cachev1.New[string, int](1 * time.Second)
        c.Set("answer", 42)

        // Jump past the TTL.
        time.Sleep(1100 * time.Millisecond)

        _, ok := c.Get("answer")
        if ok {
            t.Fatal("expected key to be expired after TTL")
        }
    })
}
```

Using `synctest` also helps ensure that all goroutines properly exit. As mentioned in Chapter 12, you must make sure that every goroutine launched also exits. Let’s demonstrate this with an improved cache implementation. The first version doesn’t remove elements when they expire; it waits for them to be looked up again. This leaks memory if an entry is placed in the cache and never referenced again. Let’s add a background job to remove expired elements:

```
func (c *Cache[K, V]) clean(interval time.Duration) {
    ticker := time.NewTicker(interval)
    defer ticker.Stop()
    for range ticker.C {
        c.mu.Lock()
        now := time.Now()
        for k, e := range c.items {
            if now.After(e.expiresAt) {
                delete(c.items, k)
                c.Stats.RemovedBySweep++
            }
        }
        c.mu.Unlock()
    }
}

func New[K comparable, V any](ttl, sweepInterval time.Duration) *Cache[K, V] {
    c := &Cache[K, V]{
        items: make(map[K]entry[V]),
        ttl:   ttl,
        done:  make(chan struct{}),
    }
    go c.clean(sweepInterval)
    return c
}
```

Every time you create a new cache instance with the `New` function, it also launches a goroutine to remove any items that have already expired. (The rest of the code for this cache implementation is in the [cachev2 directory in the Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/synctest/cachev2).) Again, being a good developer, you write a test to validate that this code works. The implementation tracks how many entries are removed via `clean`, so you can check this value:

```
func TestCache_Clean(t *testing.T) {
    c := cachev2.New[string, string](2*time.Second, 1*time.Second)
    c.Set("key", "hello")
    time.Sleep(4 * time.Second)
    val, ok = c.Get("key")
    if ok {
        t.Fatal("expected key to not be present")
    }
    if c.Stats.RemovedByClean != 1 {
        t.Error("expected removed by Sweep to be 1")
    }
}
```

If you run this test, it will pass, but it takes 4 seconds for the test to complete. Wrapping it in `synctest.Test` to speed up the test produces a surprising result:

```
=== RUN   TestCache_Clean_synctest
--- FAIL: TestCache_Clean_synctest (0.00s)
panic: deadlock: main bubble goroutine has exited but blocked goroutines remain [recovered, repanicked]
...
```

The synctest bubble ensures that every goroutine created within the bubble exit before the bubble ends. (I wish the Go team had called this “popping”, but the software industry is not in a whimsical moment.) If any goroutines continue to run, the test panics. Looking at the implemmentation of `clean`, there’s no way to signal to the goroutine when to exit. Without `synctest`, problems like this are very hard to detect.

The following changes fix this. First we update our cache implementation:

```
func (c *Cache[K, V]) clean(interval time.Duration) {
    ticker := time.NewTicker(interval)
    defer ticker.Stop()
    for {
        select {
        case <-ticker.C:
            c.mu.Lock()
            now := time.Now()
            for k, e := range c.items {
                if !now.Before(e.expiresAt) {
                    delete(c.items, k)
                    c.Stats.RemovedBySweep++
                }
            }
            c.mu.Unlock()
        case <-c.done:
            return
        }
    }
}

func (c *Cache[K, V]) Done() {
    c.mu.Lock()
    defer c.mu.Unlock()
    select {
    case <-c.done:
        return
    default:
        close(c.done)
    }
}
```

Next, we fix the test to call `c.Done`:

```
func TestCache_Clean_synctest(t *testing.T) {
    synctest.Test(t, func(t *testing.T) {
        c := cachev2.New[string, string](2*time.Second, 1*time.Second)
        t.Cleanup(c.Done)
        c.Set("key", "hello")
        time.Sleep(4 * time.Second)
        synctest.Wait()
        val, ok = c.Get("key")
        if ok {
            t.Error("expected key to not be present")
        }
        if c.Stats.RemovedBySweep != 1 {
            t.Error("expected removed by Sweep to be 1")
        }
    })
}
```

With the new `clean` implementation and the addition of the `Done` method, the test passes when it includes a call to `defer c.Done()`.

This brings us to the third problem addressed by `synctest`. You might have noticed the call to `synctest.Wait()` in the fixed code and wondered what it does. Before I answer that question, think about this: how do you *know* that sleeping for 4 seconds is enough? You know that the background cleaner goroutine will receive a tick from its timer within 4 seconds, but how do you know that it will finish looking for expired cache entries to remove and get back to waiting for the next tick of its timer?

The fact is that you don’t know how long to wait; you are taking for granted that it should be enough time. If you write tests this way and run them on a machine with lots of load (like a shared CI/CD server), you might find enough isn’t enough. Unless you use an absurdly big value for `time.Sleep`, tests fail randomly. In a `synctest` bubble where time “passes” instanteously, it becomes more likely that the work won’t complete.

The `synctest.Wait` function provides a solution for this problem. When you call it from inside a closure that’s passed to `synctest.Test`, the test pauses until all other goroutines run to completion, are waiting to read from or write to a channel, sleeping with a `time.Sleep`, waiting on a `WaitGroup.Wait`, or waiting on a `Cond.Wait`. The synctest spec calls this being “durably blocked”.

To make our background cleaner test correct, we need to add a `synctest.Wait` call after the `time.Sleep(4 * time.Second)`. This guarantees that the `clean` goroutine not only gets a chance to run, but that it runs until it is waiting to read the next tick from the ticker channel. This ensures that the entry was removed from the cache.

The `synctest` package solves many of the problems in testing concurrent code, but it does have some limitations. For more details, look at the [package documentation](https://pkg.go.dev/testing/synctest) and [the blog post from the Go team](https://go.dev/blog/testing-time) that introduced `synctest`.

# Exercises

Now that you’ve seen the more advanced testing tools provided by Go, these exercises will give you a chance to try them out. They are found in the *exercises* directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/exercises). Solutions are in the *exercise-solutions* directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/exercise-solutions).

1.  In the *ex4* [directory](https://github.com/learning-go-book-3e/ch15/tree/main/exercises/ex4), there is a file called *wealth.go* with a function called `CalcWealth` and a test for this function in the file *wealth\_test.go*. Check the code coverage from this test. Examine the code and see if there’s a bug that wasn’t detected by the test. If so, add a test to demonstrate the bug, then fix the code.
    
2.  In the *ex5* [directory](https://github.com/learning-go-book-3e/ch15/tree/main/exercises/ex5), there is a file called *count.go*. It contains three functions that perform the same task, `CountWithFields`, `CountWithFieldsSeq`, and `CountManual`. Write tests to validate that these functions work, then write benchmarks to compare their performance and amount of memory used.
    
3.  In the *ex6* [directory](https://github.com/learning-go-book-3e/ch15/tree/main/exercises/ex6), there is a file called *tracker.go* with a type called `Tracker`. There is a test for this function in *tracker\_test.go*. See if the test passes. If it does not, use the data race detector to figure out why, and fix the code to make it pass. Hint: fixing a concurrent bug might require you to think about lock acquisition and release lifetimes.
    

# Using Stubs in Go

So far, you’ve written tests for functions that didn’t depend on other code. This is not typical, as most code is filled with dependencies. As you saw in Chapter 7, Go allows you to abstract function calls in two ways: defining a function type and defining an interface. These abstractions help you write not only modular production code but also unit tests.

###### Tip

When your code depends on abstractions, it’s easier to write unit tests!

Let’s take a look at an example in the *sample\_code/solver* directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/solver). Define a type called `Processor`:

```
type Processor struct {
    Solver MathSolver
}
```

It has a field of type `MathSolver`:

```
type MathSolver interface {
    Resolve(ctx context.Context, expression string) (float64, error)
}
```

You’ll implement and test `MathSolver` in a bit.

`Processor` also has a method that reads an expression from an `io.Reader` and returns the calculated value:

```
func (p Processor) ProcessExpression(ctx context.Context, r io.Reader)
                                    (float64, error) {
    curExpression, err := readToNewLine(r)
    if err != nil {
        return 0, err
    }
    if len(curExpression) == 0 {
        return 0, ErrNoExpression
    }
    answer, err := p.Solver.Resolve(ctx, curExpression)
    return answer, err
}
```

Let’s write the code to test `ProcessExpression`. First, you need a simple implementation of the `Resolve` method to write your test:

```
type mathSolverStub struct{}

func (ms mathSolverStub) Resolve(ctx context.Context, expr string)
                                (float64, error) {
    switch expr {
    case "2 + 2 * 10":
        return 22, nil
    case "( 2 + 2 ) * 10":
        return 40, nil
    case "( 2 + 2 * 10":
        return 0, &InvalidExpressionErr{Expression: "( 2 + 2 * 10"}
    }
    return 0, nil
}
```

Next, you write a unit test that uses this stub:

```
func TestProcessor_ProcessExpressions(t *testing.T) {
    p := Processor{mathSolverStub{}}
    in := strings.NewReader(`2 + 2 * 10
( 2 + 2 ) * 10
( 2 + 2 * 10`)
    data := []struct {
        name string
        val  float64
        err  error
    }{
        {name: "case1", val: 22, err: nil},
        {name: "case2", val: 40, err: nil},
        {name: "case3", val: 0,
         err: &InvalidExpressionErr{Expression: "( 2 + 2 * 10"}},
    }
    for _, d := range data {
        t.Run(d.name, func(t *testing.T) {
            result, err := p.ProcessExpression(t.Context(), in)
            if !errors.Is(err, d.err) {
                t.Errorf("expected error %v, got %v", d.err, err)
            }
            if result != d.val {
                t.Errorf("Expected result %f, got %f", d.val, result)
            }
        })
    }
}
```

You can then run your test and see that everything works.

While most Go interfaces specify only one or two methods, you’ll sometimes find yourself with an interface that has many methods. Let’s take a look at the code in the *sample\_code/stub* directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/stub). Assume you have an interface that looks like this:

```
type Entities interface {
    GetUser(id string) (User, error)
    GetPets(userID string) ([]Pet, error)
    GetChildren(userID string) ([]Person, error)
    GetFriends(userID string) ([]Person, error)
    SaveUser(user User) error
}
```

There are two patterns for testing code that depends on large interfaces. The first is to embed the interface in a struct, which automatically defines all the interface’s methods on the struct. It doesn’t provide any implementations of those methods, so you need to implement the methods that you care about for the current test. Let’s assume that `Logic` is a struct that has a field of type `Entities`:

```
type Logic struct {
    Entities Entities
}
```

Assume you want to test this method:

```
func (l Logic) GetPetNames(userId string) ([]string, error) {
    pets, err := l.Entities.GetPets(userId)
    if err != nil {
        return nil, err
    }
    out := make([]string, len(pets))
    for _, p := range pets {
        out = append(out, p.Name)
    }
    return out, nil
}
```

This method uses only one of the methods declared on `Entities`: `GetPets`. Rather than creating a stub that implements every single method on `Entities` just to test `GetPets`, you can write a stub struct that implements only the method you need to test this method:

```
type getPetNamesStub struct {
    Entities
}

func (ps getPetNamesStub) GetPets(userID string) ([]Pet, error) {
    switch userID {
    case "1":
        return []Pet{{Name: "Bubbles"}}, nil
    case "2":
        return []Pet{{Name: "Stampy"}, {Name: "Snowball II"}}, nil
    default:
        return nil, &InvalidIDError{ID: userID}
    }
}
```

You then write your unit test, with your stub injected into `Logic`:

```
func TestLogicGetPetNames(t *testing.T) {
    data := []struct {
        name     string
        userID   string
        petNames []string
        err      error
    }{
        {"case1", "1", []string{"Bubbles"}, nil},
        {"case2", "2", []string{"Stampy", "Snowball II"}, nil},
        {"case3", "3", nil, &InvalidIDError{ID: "3"}},
    }
    l := Logic{getPetNamesStub{}}
    for _, d := range data {
        t.Run(d.name, func(t *testing.T) {
            petNames, err := l.GetPetNames(d.userID)
            if !errors.Is(err, d.err) {
                t.Errorf("expected error %v, got %v", d.err, err)
            }
            if diff := cmp.Diff(d.petNames, petNames); diff != "" {
                t.Error(diff)
            }
        })
    }
}
```

(By the way, the `GetPetNames` method has a bug. Did you see it? Even simple methods can sometimes have bugs.)

###### Warning

If you embed an interface in a stub struct, make sure you provide an implementation for every method that’s called during your test! If you call an unimplemented method, your tests will panic.

If you need to implement only one or two methods in an interface for a single test, this technique works well. The drawback comes when you need to call the same method in different tests with different inputs and outputs. When that happens, you need to either include every possible result for every test within the same implementation or reimplement the struct for each test. This quickly becomes difficult to understand and maintain. A better solution is to create a stub struct that proxies method calls to function fields. For each method defined on `Entities`, define a function field with a matching signature on your stub struct:

```
type entitiesStub struct {
    getUser     func(id string) (User, error)
    getPets     func(userID string) ([]Pet, error)
    getChildren func(userID string) ([]Person, error)
    getFriends  func(userID string) ([]Person, error)
    saveUser    func(user User) error
}
```

Then make `entitiesStub` meet the `Entities` interface by defining the methods. In each method, invoke the associated function field. For example:

```
func (es entitiesStub) GetUser(id string) (User, error) {
    return es.getUser(id)
}

func (es entitiesStub) GetPets(userID string) ([]Pet, error) {
    return es.getPets(userID)
}
```

Once you create this stub, you can supply different implementations of different methods in different test cases via the fields in the data struct for a table test:

```
func TestLogicGetPetNames2(t *testing.T) {
    data := []struct {
        name     string
        getPets  func(userID string) ([]Pet, error)
        userID   string
        petNames []string
        err      error
    }{
        {"case1", func(userID string) ([]Pet, error) {
            return []Pet{{Name: "Bubbles"}}, nil
        }, "1", []string{"Bubbles"}, nil},
        {"case2", func(userID string) ([]Pet, error) {
            return []Pet{{Name: "Stampy"}, {Name: "Snowball II"}}, nil
        }, "2", []string{"Stampy", "Snowball II"}, nil},
        {"case3", func(userID string) ([]Pet, error) {
            return nil, &InvalidIDError{ID: "3"}
        }, "3", nil, &InvalidIDError{ID: "3"}},
    }
    l := Logic{}
    for _, d := range data {
        t.Run(d.name, func(t *testing.T) {
            l.Entities = entitiesStub{getPets: d.getPets}
            petNames, err := l.GetPetNames(d.userID)
            if diff := cmp.Diff(d.petNames, petNames); diff != "" {
                t.Error(diff)
            }
            if !errors.Is(err, d.err) {
                t.Errorf("Expected error %v, got %v", d.err, err)
            }
        })
    }
}
```

Add a field of function type to `data`’s anonymous struct. In each test case, specify a function that returns the data that `GetPets` would return. When you write your test stubs this way, it’s clear what the stubs should return for each test case. Each test run instantiates a new `EntitiesStub` and assigns the `getPets` function field in the test data to the `getPets` function field in `EntitiesStub`.

# Using httptest

It’s important to ensure that  calls to HTTP services send the right requests and handle responses properly. While integration testing is essential for validating a multi-service deployment, it is often slow. Quick-running unit tests for HTTP clients provide confidence during development. The Go standard library includes the [`net/http/httptest`](https://oreil.ly/0c_MX) package, which makes it easy to stub HTTP services. Let’s go back to the *sample\_code/solver* directory in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/tree/main/sample_code/solver) and provide an implementation of `MathSolver` that calls an HTTP service to evaluate expressions:

```
type RemoteSolver struct {
    MathServerURL string
    Client        *http.Client
}

func (rs RemoteSolver) Resolve(ctx context.Context, expression string)
                              (float64, error) {
    req, err := http.NewRequestWithContext(ctx, http.MethodGet,
        rs.MathServerURL+"?expression="+url.QueryEscape(expression), nil)
    if err != nil {
        return 0, err
    }
    resp, err := rs.Client.Do(req)
    if err != nil {
        return 0, err
    }
    defer resp.Body.Close()
    contents, err := io.ReadAll(resp.Body)
    if err != nil {
        return 0, err
    }
    if resp.StatusCode != http.StatusOK {
        return 0, &RequestErr{
            StatusCode: resp.StatusCode,
            Contents:   string(contents),
        }
    }
    result, err := strconv.ParseFloat(string(contents), 64)
    if err != nil {
        return 0, err
    }
    return result, nil
}
```

Now let’s see how to use the `httptest` library to test this code without standing up a server. The code is in the `TestRemoteSolver_Resolve` function in *sample\_code/solver/remote\_solver\_test.go* in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/blob/main/sample_code/solver/remote_solver_test.go), but here are the highlights. First, you want to make sure that the data that’s passed into the function arrives on the server. In your test function, define a type called `info` to hold your input and output and a variable called `io` that is assigned the current input and output:

```
type info struct {
    expression string
    code       int
    body       string
}
var io info
```

Next, set up your fake remote server and use it to configure an instance of `RemoteSolver`:

```
server := httptest.NewServer(
    http.HandlerFunc(func(rw http.ResponseWriter, req *http.Request) {
        // validate the incoming request to make sure the client is
        // sending the expected data
        if req.URL.Query().Get("expression") != io.expression {
            rw.WriteHeader(http.StatusBadRequest)
            return
        }
        rw.WriteHeader(io.code)
        rw.Write([]byte(io.body))
    }))
defer server.Close()
rs := RemoteSolver{
    MathServerURL: server.URL,
    Client:        server.Client(),
}
```

The `httptest.NewServer` function creates and starts an HTTP server on a random unused port. You need to provide an `http.Handler` implementation to process the request. Since this is a server, you must close it when the test completes. The `server` instance has its URL specified in the `URL` field of the `server` instance and a preconfigured `http.Client` for communicating with the test server. Pass these into `RemoteSolver`.

The rest of the function works like every other table test that you’ve seen:

```
data := []struct {
    name   string
    io     info
    result float64
    err    error
}{
    {"case1", info{"2 + 2 * 10", http.StatusOK, "22"}, 22, nil},
    {"case2", info{"( 2 + 2 ) * 10", http.StatusOK, "40"}, 40, nil},
    {"case3", info{"( 2 + 2 * 10", http.StatusBadRequest,
                    "invalid expression: ( 2 + 2 * 10"}, 0,
        &RequestErr{
            StatusCode: http.StatusBadRequest,
            Contents:   "invalid expression: ( 2 + 2 * 10",
        }},
}
for _, d := range data {
    t.Run(d.name, func(t *testing.T) {
        io = d.io
        result, err := rs.Resolve(t.Context(), d.io.expression)
        if result != d.result {
            t.Errorf("io `%f`, got `%f`", d.result, result)
        }
        if !errors.Is(err, d.err) {
            t.Errorf("expected error %v, got %v", d.err, err)
        }
    })
}
```

The variable `io` has been captured by two closures: the one for the stub server and the one for running each test. You write to it in one closure and read it in the other. This is a bad idea in production code, but it works well in test code within a single function.

# Using Integration Tests and Build Tags

Even though `httptest` provides a way to avoid testing against external services, you should still write *integration tests*, automated tests that connect to other services. These validate that your understanding of the service’s API is correct. The challenge is figuring out how to group your automated tests; you want to run integration tests only when the support environment is present. Also, integration tests tend to be slower than unit tests, so they are usually run less frequently.

In Chapter 11, I covered build tags, which the Go compiler uses to control when a file is compiled. While they are primarily intended to allow developers to write code that’s intended only for a specific operating system, CPU, or Go version, you can take advantage of the ability to specify custom build tags to control when integration tests are compiled and run.

Let’s try this out with the math-solving project. Use [Docker](https://oreil.ly/5FWcb) to download a server implementation with `docker pull jonbodner/math-server` and then run the server locally on port 8080 with `docker run -p 8080:8080` `jonbodner/math-server`. (The *sample\_code/solver* directory in the Chapter 15 repository has a Docker Compose file, so if you have cloned it locally, you can run `docker compose up`.)

###### Note

If you don’t have Docker installed or if you want to build the code for yourself, you can find the math-server code on [GitHub](https://oreil.ly/yjMzc).

You need to write an integration test to make sure that your `Resolve` method properly communicates with the math server. The *sample\_code/solver/remote\_solver\_integration\_test.go* file in the [Chapter 15 repository](https://github.com/learning-go-book-3e/ch15/blob/main/sample_code/solver/remote_solver_integration_test.go) has a complete test in the `TestRemoteSolver_ResolveIntegration` function. The test looks like every other table test that you’ve written. The interesting thing is the first line of the file, separated from the package declaration by a newline:

```
//go:build integration
```

To run your integration test alongside the other tests you’ve written, use this:

```
$ go test -tags integration -v ./...
```

Some people in the Go community prefer to use environment variables rather than build tags to create groups of integration tests. Peter Bourgon describes the concept in a blog post with the unsubtle title [“Don’t Use Build Tags for Integration Tests”](https://oreil.ly/BXSnu). His argument is that it’s hard to find out what build tags to set in order to run integration tests. An explicit check for an environment variable in each integration test, combined with a detailed message in a `t.Skip` method call, makes it clear that tests aren’t being run and how to run them. In the end, it’s a trade-off between verbosity and discoverability. Feel free to use either technique.

# Exercises

Now that you’ve learned about using mocks and stubs, complete these exercises to apply that knowledge to a sample application.

1.  In the ex7 directory, the `notifier.go` file contains a type called `NotificationService` with a method called `Notify`. Use a function field stub to write a table test to make sure this code properly handles correct data, invalid input, and a failed send.
    
2.  In the ex8 directory, the `report.go` file contains a type called `ReportBuilder` with a method called `PriceSummary`. `ReportBuilder` depends on a large interface (`DataStore`), but `PriceSummary` only used one method in the interface. Write a test to validate that `PriceSummary` works correctly.
    
3.  In the ex9 directory, the `weather.go` file contains a type called `Client` with a method called `GetTemperature`, which connects to a remote HTTP server to get weather data. Write a unit test to validate this method that doesn’t require you to run an external server.
    

# Wrapping Up

In this chapter, you’ve learned how to write tests and improve code quality by using Go’s built-in support for testing, code coverage, benchmarking, fuzzing, and data race checking. In the next chapter, you’re going to explore some Go features that allow you to break the rules: the `unsafe` package, reflection, and `cgo`.

# About the Author

**Jon Bodner** has been a software engineer, lead developer, and architect for over 25 years. In that time, he has worked on software across many fields, including education, finance, commerce, healthcare, law, government, and internet infrastructure.

Jon is currently a staff engineer at Datadog, working on improving the onboarding experience for customers.

Previously, Jon was a senior distinguished engineer at Capital One, where he worked on commercialization of technology, contributed to the company’s development and testing workflow, developed patented techniques for web payment page detection and population, and coauthored tools for finding and managing software development issues.

Jon is a frequent speaker at Go conferences, and his blog posts on Go and software engineering have been viewed more than 300,000 times. He is the creator of the [Proteus data access library](https://github.com/jonbodner/proteus) and codeveloper of [checks-out](https://github.com/capitalone/checks-out), a fork of the LGTM project.