
## O'REILLY

## Command-Line
Rust

A Project-Based Primer for Writing Rust CLIs

![](img/fbe8ef8a7ecb95c985397f6370ec2c66_0_0.png)

Early Release
RAW & UNEDITED

Ken Youens-Clark

## Command-Line Rust

A Project-Based Primer for Writing Rust CLIs

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

Ken Youens-Clark

## Command-Line Rust

by Charles Kenneth Youens-Clark

Copyright © 2022. All rights reserved.

Printed in the United States of America.

Published by O’Reilly Media, Inc., 1005 Gravenstein Highway North, Sebastopol, CA 95472.

O’Reilly books may be purchased for educational, business, or sales promotional use. Online editions are also available for most titles (http://oreilly.com). For more information, contact our corporate/institutional sales department: 800-998-9938 or corporate@oreilly.com.

Acquisitions Editor: Suzanne McQuade

Development Editor: Corbin Collins

Production Editor: Caitlin Ghegan

Copyeditor: TO COME

Proofreader: TO COME

Indexer: TO COME

Interior Designer: David Futato

Cover Designer: TO COME

Illustrator: Kate Dullea

February 2022: First Edition

## Revision History for the Early Release:

- 2021-06-18: First Release
- 2021-08-23: Second Release

See http://oreilly.com/catalog/errata.csp?isbn=9781098109431 for release details.

The O’Reilly logo is a registered trademark of O’Reilly Media, Inc.
*Systems Programming with Rust*, the cover image, and related trade dress are trademarks of O’Reilly Media, Inc.

The views expressed in this work are those of the author, and do not represent the publisher’s views. While the publisher and the author have used good faith efforts to ensure that the information and instructions contained in this work are accurate, the publisher and the author disclaim all responsibility for errors or omissions, including without limitation responsibility for damages resulting from the use of or reliance on this work. Use of the information and instructions contained in this work is at your own risk. If any code samples or other technology this work contains or describes is subject to open source licenses or the intellectual property rights of others, it is your responsibility to ensure that your use thereof complies with such licenses and/or rights.

978-1-098-10941-7

[???]

## Preface

### A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the Preface of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the author at kyclark@gmail.com.

> *I already know the ending it’s the part that makes your face implode*
> 
> —They Might Be Giants

I remember years back when this new language called “JavaScript” came out. It sounded interesting, so I bought a big, thick reference book on the language and read it cover to cover. When I finished, I couldn’t write JavaScript to save my life. My problem was that I hadn’t written any programs in the language. I’ve since learned better how to learn a language, which is perhaps the most valuable skill you can develop as a programmer. When I want to learn a language now, I start by rewriting things I already know like Tic-Tac-Toe.

Rust is reputed to have a fairly steep learning curve, but I’m convinced you can learn it much more quickly by writing many small programs you already know. This book is about two things: systems programming and Rust. Writing Rust versions of basic systems tools like `head` and `cal` will reveal patterns that you’ll be able to use when you write your own programs—patterns like validating parameters, reading and writing file handles, parsing text, and using regular expressions.

## What Is Rust (And Why Is Everybody Talkin’ About It)?

Rust is a programming language created by Graydon Hoare while working at Mozilla Research. He first started working on it around 2006 as a personal project, and by 2010 Mozilla had sponsored and announced the project. While the language is relatively new, it has quickly earned a passionate following of programmers who claim to even *love* using it.

![](img/fbe8ef8a7ecb95c985397f6370ec2c66_6_0.png)

Figure P-1. Here is a logo I made from an old Rush logo. Being a kid playing the drums in the 1980s, you better believe I listened to a lot of Rush. Anyway, Rust is cool, and this logo proves it.

Often Rust is described as a systems programming language that has been designed for performance and safety. It has a syntax that resembles the C language, so you’ll find things like `for` loops, semicolon-terminated statements, and curly braces denoting block structures.

Rust is also a *statically typed* language, meaning that a variable can never change its type, like from a number to a string. If you’re coming from languages like C/C++ or Java, this will be familiar because those languages are also statically typed. You don’t always have to declare a variable’s type in Rust because the compiler can often figure it out from the context, but the variable is still never allowed to change its type.

If, like me, you are coming from a *dynamically typed* language like Perl, JavaScript, or Python, this could feel rather new. In those languages, a variable can change its type at any point in the program, like from a string to a file handle. The language may also silently treat a variable of one type like another; for instance, both Perl and Python will evaluate strings, numbers, lists, dictionaries, and more in a Boolean context which can lead to some surprising results. Interestingly, several dynamically typed languages including Python, Perl 6/Raku, and Julia have introduced type hints or *gradual typing* systems where a variable can be basically untyped (`Any`) or some specific type like an integer or string.

Although Rust bears a strong resemblance to imperative, C-style languages, it has borrowed many exciting concepts from other languages and programming paradigms. Rust is not an object-oriented (OO) language like Java as there are no classes or inheritance in Rust. Instead Rust uses a `struct` (structure) to represent complex data types and traits to describe how types can behave. These structures can have methods, can mutate the internal state of the data, and might even be called *objects* in the documentation, but they are not objects in the formal sense of the word.

Rust has borrowed many exciting ideas from purely functional languages like Haskell. For instance, variables are *immutable* by default, meaning they can’t be changed. Like many languages, functions are *first-class* values, which means they can be passed as arguments. Most exciting to my mind is Rust’s use of *enumerated* and *sum* types, also called *algebraic data types* (ADTs), which allow you to represent, for instance, that a function can return a `Result` which can be either an `Ok` containing some value or an `Err` containing some other kind of value. Any code that deals with these values must handle all possibilities, so you’re never at risk of forgetting to handle an error that could unexpectedly crash your program.

## Who Should Read This Book

If you’re reading this, I imagine you already know at least one programming language. Maybe your background is in statically typed languages like C++ or Java, or maybe like me, you’re coming from the world of dynamically typed languages. I’ve spent the bulk of my career using Perl and Python for web development and systems tasks, but I’ve been very interested in learning everything from Prolog and Haskell to JavaScript and Lisp. No matter your background, I hope to show you that Rust is a really fun language.

Rust requires learning some pretty low-level stuff about memory and types, so I can’t imagine it would be a great first language. I’m going to offload much of the nitty-gritty to reference books like *Programming Rust* (Blandy, Orendorff, and Tindall; O'Reilly, 2021) and *The Rust Programming Language* (Klabnik and Nichols; No Starch Press, 2019). I highly recommend that you read one or both of those along with this book to dig deeper into the language itself.

This book will focus on how to write practical programs, starting from scratch and working step-by-step to add features, build your program, work through error messages, and test your logic. You should read this book if you want to learn how to write complete programs that solve common systems problems. Although I’ll be showing why Rust is particularly well-suited for this, I think you should also try writing these programs in other languages you know so that you can contrast what makes Rust better or worse, easier or harder.

Lastly, you should also read this book if you care about testing. I’m an advocate for *test-driven development* where I write tests first and then try to write code that passes those tests. My experience teaching has convinced me that this is an effective way to think
