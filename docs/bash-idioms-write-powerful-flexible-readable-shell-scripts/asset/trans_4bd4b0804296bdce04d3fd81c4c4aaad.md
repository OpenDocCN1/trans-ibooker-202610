

Embedded Documentation 115

bash Debugging 120

bash Unit Testing 122

Summary 122

## 11. Developing Your Style Guide. 123

Readability 124

Comments 126

Names 127

Functions 128

Quoting 129

Layout 130

Syntax 131

Other 132

Script Template 132

Other Style Guides 133

bash Linter 134

Summary 135

## Appendix. The bash Idioms Style Guide. 137

## Index. 145

# Preface

Webster’s Dictionary defines *idiom* as:¹

+   1: an expression in the usage of a language that is peculiar to itself either in having a meaning that cannot be derived from the conjoined meanings of its elements (such as up in the air for “undecided”) or in its grammatically atypical use of words (such as give way)
+   2a: the language peculiar to a people or to a district, community, or class: dialect
+   2b: the syntactical, grammatical, or structural form peculiar to a language
+   3: a style or form of artistic expression that is characteristic of an individual, a period or movement, or a medium or instrument

Why *bash Idioms*? One word—readability. Or perhaps a different word—understandability. In this book, those words mean the same thing. We don’t have to convince you that readability is critically important; unless this is the first book about programming you are reading,² you already get it. *Readability* means being able to read and understand code, especially code that someone else wrote, but it also means being able to write code that you, or someone else, can later read and understand. Clearly these aspects are different sides of the same coin, so we’ll explore both the clear idioms to use and the obscure ones to avoid.

We think of bash, informally, as a language to use to “run things.” If you need to do a lot of heavy data processing, bash may not be the first choice. You can do it, but it might not be pretty. Of course, if you already have the data processing tools you need and you just have to “glue” them together, well, then bash is great. But if all we do is run things, why do we care about the idioms of the language or its “structural form”? Programs grow, features creep, things change, and there is nothing more permanent than a “temporary solution.” Sooner or later someone is going to have to read the code, understand it, and make changes. If it’s written using an obscure idiom, the job is that much harder.

In a lot of ways, bash doesn’t look like other languages. It has a lot of history (some may say “baggage”), and there are reasons it looks and works the way it does. We’re not going to talk about that very much, because we cover a lot of that in the *bash Cookbook*. Shell scripts arguably “run the world,” at least in the Unix and Linux worlds (and Linux pretty much runs the cloud world), and a huge majority of those scripts are interpreted by bash. Maintaining backward compatibility back to the very first Unix shells is critically important, but it imposes some…restrictions.

Then there are the dialects. The big one, especially for backward compatibility, is POSIX. We won’t talk too much about that either—after all, this is *bash Idioms*, not *POSIX Idioms*. Other dialects may appear when programmers write bash code in a style that is more characteristic of another language they know. A flow that makes sense in C may feel clumsy or disjointed in bash, though. So with this book, we intend to demonstrate a “style or form of…expression that is characteristic” of bash (in the spirit of the third definition in Webster’s Dictionary). Python programmers talk about their style as “pythonic.” We’d like this book to illustrate and illuminate code that is “bashy.”

By the end of this book the reader will be able to do the following:

+   - Write useful, flexible, and readable bash code…with style
+   - Decode bash idioms such as `${MAKEMELC,,}` and `${PATHNAME##*/}`
+   - Save time and ensure consistency when automating tasks
+   - Amaze and impress colleagues with bash idioms
+   - Discover how bash idioms can make your code clean and concise

## Running bash

We’re going to assume you are already programming in bash, and therefore you don’t need to learn where to find or install it. Of course, bash is *just there* in almost all Linux distributions, and already there by default or installable on virtually any other operating system. You can get it in Windows using “Git for Windows”, Windows Subsystem for Linux (WSL), or various other options we cover in the *bash Cookbook*.

## bash on Mac

That said, watch out for stock bash on a Mac; it’s quite old, and many newer idioms (v4+) won’t work. You can obtain a newer version by installing MacPorts, Homebrew, or Fink and looking for bash. According to Apple, the issue is that newer versions of bash use GPLv3, which is a problem for Apple.

On a related note, Apple also says that macOS Catalina and newer will use Zsh as the default login and interactive shell. Zsh is mostly compatible with bash, but some code in this book won’t work unmodified. On Macs, bash isn’t going away (yet, at least), and having Zsh as your default shell will not affect a bash “shebang” line (see “Shebang!” on page 94), but again, unless you upgrade, you’ll be stuck with stone-age bash.

We’ve tagged example scripts “Does not work on Zsh 5.4.2” as a best-effort clue for Mac users, but this is a book about bash, so we’re going to stay focused.

## bash in Containers

Be careful in Docker or other containers where /bin/sh is not bash and /bin/bash may not even exist! This applies to Internet of Things and other constrained environments such as industrial controllers.

/bin/sh may be bash in “POSIX” mode, but it may also be Ash or Dash, or BusyBox (which is probably actually Dash), or maybe even something else. You’ll need to be specific (see also “Shebang!” on page 94) and possibly either ensure that bash is actually installed or stick to POSIX and avoid “bashisms.”

## Revision Control

We sincerely hope you are already using some kind of revision control system, so if you are, you can skip this paragraph. If you are not, you should start immediately. We cover all of that in an entire appendix in the bash Cookbook, but there are huge amounts of information about that on the internet, including one author’s thoughts on the subject. Go figure something out; we’ll wait.

## Hello World

In many other resources, you have to wait until the end of chapter 1 or maybe even chapter 2 or 3 before you get to “Hello World,” but we’re going to jump right in! Since you are already writing bash code and keeping it in revision control (right?), talking about echo 'Hello, World' would be pretty silly, so we won’t. Oops.

## Conventions Used in This Book

The following typographical conventions are used in this book:

Italic
Indicates new terms, URLs, email addresses, filenames, and file extensions.

Constant width
Used for program listings, as well as within paragraphs to refer to program elements such as variable or function names, databases, data types, environment variables, statements, and keywords.

Constant width bold
Shows commands or other text that should be typed literally by the user.

Constant width italic
Shows text that should be replaced with user-supplied values or by values determined by context.

![](img/553ffa1bc15093729b691f706befc0ac_11_0.png)

This element signifies a tip or suggestion.

![](img/553ffa1bc15093729b691f706befc0ac_11_1.png)

This element signifies a general note.

![](img/553ffa1bc15093729b691f706befc0ac_11_2.png)

This element indicates a warning or caution.

## Using Code Examples

Supplemental material (code examples, exercises, etc.) is available for download at https://github.com/vossenjp/bashidioms-examples.

If you have a technical question or a problem using the code examples, please send email to bookquestions@oreilly.com.

This book is here to help you get your job done. In general, if example code is offered with this book, you may use it in your programs and documentation. You

do not need to contact us for permission unless you’re reproducing a significant portion of the code. For example, writing a program that uses several chunks of code from this book does not require permission. Selling or distributing examples from O’Reilly books does require permission. Answering a question by citing this book and quoting example code does not require permission. Incorporating a significant amount of example code from this book into your product’s documentation does require permission.

We appreciate, but generally do not require, attribution. An attribution usually includes the title, author, publisher, and ISBN. For example: “bash Idioms by Carl Albing and JP Vossen (O’Reilly). Copyright 2022 Carl Albing and JP Vossen, 978-1-492-09475-3.”

If you feel your use of code examples falls outside fair use or the permission given above, feel free to contact us at permissions@oreilly.com.

## O’Reilly Online Learning

For more than 40 years, O’Reilly Media has provided technology and business training, knowledge, and insight to help companies succeed.

Our unique network of experts and innovators share their knowledge and expertise through books, articles, conferences, and our online learning platform. O’Reilly’s online learning platform gives you on-demand access to live training courses, in-depth learning paths, interactive coding environments, and a vast collection of text and video from O’Reilly and 200+ other publishers. For more information, please visit https://oreilly.com.

## How to Contact Us

Please address comments and questions concerning this book to the publisher:

O’Reilly Media, Inc.
1005 Gravenstein Highway North
Sebastopol, CA 95472
800-998-9938 (in the United States or Canada)
707-829-0515 (international or local)
707-829-0104 (fax)

We have a web page for this book, where we list errata, examples, and any additional information. You can access this page at https://oreil.ly/bashIdioms.

