
# O'REILLY®

# WebAssembly

# The Definitive Guide

Safe, Fast, and Portable Code

![](img/16b7990eb0dbf589db177bdbd2f4a189_0_0.png)

# Early Release
RAW & UNEDITED

Brian Sletten

# WebAssembly: The Definitive Guide

Safe, Fast, and Portable Code

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

# Brian Sletten

# WebAssembly: The Definitive Guide

by Brian Sletten

Copyright © 2022 Bosatsu Consulting, Inc.. All rights reserved.

Printed in the United States of America.

Published by O’Reilly Media, Inc., 1005 Gravenstein Highway North, Sebastopol, CA 95472.

O’Reilly books may be purchased for educational, business, or sales promotional use. Online editions are also available for most titles (http://oreilly.com). For more information, contact our corporate/institutional sales department: 800-998-9938 or corporate@oreilly.com.

Acquisitions Editor: Suzanne McQuade

Development Editor: Angela Rufino

Production Editor: Kate Galloway

Copyeditor: Piper Editorial Consulting, LLC

Interior Designer: David Futato

Cover Designer: Karen Montgomery

Illustrator: Kate Dullea

December 2021: First Edition

# Revision History for the Early Release

- 2020-11-05: First Release
- 2020-11-16: Second Release
- 2021-02-04: Third Release
- 2021-03-10: Fourth Release
- 2021-03-23: Fifth Release
- 2021-05-04: Sixth Release
- 2021-10-08: Seventh Release

See http://oreilly.com/catalog/errata.csp?isbn=9781492089841 for release details.

The O’Reilly logo is a registered trademark of O’Reilly Media, Inc.
*WebAssembly: The Definitive Guide*, the cover image, and related trade dress are trademarks of O’Reilly Media, Inc.

The views expressed in this work are those of the author, and do not represent the publisher’s views. While the publisher and the author have used good faith efforts to ensure that the information and instructions contained in this work are accurate, the publisher and the author disclaim all responsibility for errors or omissions, including without limitation responsibility for damages resulting from the use of or reliance on this work. Use of the information and instructions contained in this work is at your own risk. If any code samples or other technology this work contains or describes is subject to open source licenses or the intellectual property rights of others, it is your responsibility to ensure that your use thereof complies with such licenses and/or rights.

978-1-492-08977-3

## Dedication

This book was born on a mountain of privilege during a period when many people didn’t have the luxury of working from home. It is therefore dedicated to the front line and essential workers who kept the lights on during a dark time.

## Preface

### A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the author at wasmbook@bosatsu.net.

I believe WebAssembly is an ascendant technology that has the potential to transform the entire software development industry in one form or another. I do not believe WebAssembly is going to be transformative because I am writing a book on the topic. I am writing a book on it because I believe it will be transformative.

Presumably you are interested in the technology as well. The problem is, I think I have less of an idea of who you are as a reader than many authors do. If this were a book about a particular programming language or a specific topic, there would be a self-selecting aspect to the audience and I could proceed apace. But WebAssembly is a much larger topic than most people realize and I am trying to paint a very large picture with this book. Most of the other books that have been published have focused on a single aspect of it and I can understand why.

Some of you might think it is a technology for killing JavaScript. It isn’t. Some of you may think it is about bringing applications to the browser. It is that, but it is also so much more. It is useful on the server side, in the video game world, as a plugin mechanism, in support of serverless functions and edge computing, in embedded systems, the blockchain, and many other

topics we will investigate together. This is the first attempt I know of to be this comprehensive with the topic and I felt it was important and time to tell this more complete version.

In the lead up to the publication of the book, I have mostly gotten positive support and excitement from people I have spoken to about the project. One limited form of pushback I have gotten is with respect to the title. Some folks felt it was premature to have “The Definitive Guide” for this new of a technology. That is a fair position to take, but because I am trying describe an extremely big and encompassing technical landscape, I thought it was reasonable. I hope by the end of the book you agree.

All I ask is that you have an open mind and a bit of patience. WebAssembly touches a lot languages, runtimes, and operational environments. In addition to teaching you about the low-level details, we will look at integrations with the dominant programming languages in this space and several different use cases. I have tried not to make too many assumptions about your background so I have heavily annotated the text with breadcrumbs for further exploration and discovery via footnotes. If you are a more advanced developer just seeking details about WebAssembly, feel free to ignore these and don’t take offense. I expect a rather wide audience will be at least perusing this book and I want them to feel welcome too.

If you are on the junior side development-wise, this will be a challenging book. But I have tried to make it possible for you to at least see what is going on. Consider the various links and references as a personal guide into a more sophisticated development reality. Don’t get overwhelmed, just tackle things one at a time in whatever order interests you or makes sense. There is no single way into this industry and however you get there is legitimate.

At the end of the day, WebAssembly is going to allow us to basically choose our programming languages and run them securely in just about any computational surface area. We have been promised this before, but I think this time it is more likely to come to fruition. Thank you for giving me the opportunity to explain why.

## Conventions Used in This Book

The following typographical conventions are used in this book:

*Italic*

Indicates new terms, URLs, email addresses, filenames, and file extensions.

*Constant width*

Used for program listings, as well as within paragraphs to refer to program elements such as variable or function names, databases, data types, environment variables, statements, and keywords.

*Constant width bold*

Shows commands or other text that should be typed literally by the user.

*Constant width italic*

Shows text that should be replaced with user-supplied values or by values determined by context.

TIP
This element signifies a tip or suggestion.

NOTE
This element signifies a general note.

WARNING
This element indicates a warning or caution.

## Using Code Examples

Supplemental material (code examples, exercises, etc.) is available for download at https://github.com/bsletten/wasm_tdg.

If you have a technical question or a problem using the code examples, please send email to bookquestions@oreilly.com.

This book is here to help you get your job done. In general, if example code is offered with this book, you may use it in your programs and documentation. You do not need to contact us for permission unless you’re reproducing a significant portion of the code. For example, writing a program that uses several chunks of code from this book does not require permission. Selling or distributing examples from O’Reilly books does require permission. Answering a question by citing this book and quoting example code does not require permission. Incorporating a significant amount of example code from this book into your product’s documentation does require permission.

We appreciate, but generally do not require, attribution. An attribution usually includes the title, author, publisher, and ISBN. For example: “WebAssembly The Definitive Guide by Brian Sletten(O’Reilly). Copyright 2022 Bosatsu Consulting, Inc., 978-1492089841.”

If you feel your use of code examples falls outside fair use or the permission given above, feel free to contact us at permissions@oreilly.com.

## O’Reilly Online Learning

> NOTE
> For more than 40 years, O’Reilly Media has provided technology and business training, knowledge, and insight to help companies succeed.
