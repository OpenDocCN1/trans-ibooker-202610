
## Part 3: Object-Oriented Visual Basic

## 12 Object-Oriented Programming
- Classes and fields
- Class declaration
- Fields
- Methods
- Method declaration
- Instantiating a class
- One-to-one has-a relationships
- Summary

## 13 Inheritance
- Inheritance in VB.NET
- Inheritance differences in family members
- VB6/VBA
- VB.NET
- Exception handling
- Is-a relationships
- Summary

## 14 Polymorphism
- Polymorphism
- Abstract classes
- Sealed classes

## 15 Interfaces
Summary
Interfaces
Developing interfaces
Interface declaration
Implementing an interface
Using interface polymorphism
Standard interfaces
Summary

## Project Part III
## Technical requirements
## Building the project in the VB.NET console
## Coding the Visual Basic.NET console grading tool
## Building the project in ASP.NET
## Coding an ASP.NET calculator with Visual Studio
## Summary

## Part 4: Server-Side Development

## 17 The Request and Response Model
- Classic ASP Request object
- ASP.NET Request object
- Classic ASP Response object
- ASP.NET Response object
- Summary

## 18 Variable Scope and Concurrency
- Session scope
- Application scope
- Request scope and cookies
- Request scope in ASP
- Cookies in ASP
- Summary

## 19 Project Part IV
## Technical requirements
## Building the project in Classic ASP
## Coding the Classic ASP grading tool with Notepad
## Building the project in ASP.NET
## Coding the ASP.NET server-side grade averager with Visual Studio
## Summary

## 20 Conclusions
## Summing it all up
## Variable declaration
## Data types
## Scope
## Decision branching
## Procedure modularization
## Collections
## File handling
## Object encapsulation
## Inheritance
## Polymorphism
## Web application HTTP model and concurrency
## Further reading topics
## VB6 form development
## VB.NET WinForms development
## COM object creation
## Suggested projects
## Summary

## Index
Other Books You May Enjoy

## Preface

In this book, you will learn about the Visual Basic language and all its different use cases. This journey will enable you to apply that understanding to maintain, enhance, administer, and secure Visual Basic console or Windows applications, Visual Basic websites, or Visual Basic scripts. This book starts from an understanding that many programmers will need to use Visual Basic for many different purposes. In each chapter, we highlight topics across the many different use cases Visual Basic is used for.

When learning how to program in a new language, you always want to give yourself time to digest the concepts. Many iterations of the topic help to solidify it in your mind. To get the most out of this book, you are encouraged to have your computer open as you read the book. Another terrific approach is to read a chapter for the first time and then go to your computer and revisit the examples by plugging them in yourself on your computer.

I also encourage you to dig deep into your creativity and think of ways to use the skills we cover in a creative project. I suggest several projects in the last chapter of the book, but you should try to find something meaningful for you. Good luck!

## Who this book is for

If you work in software engineering, information technology, or cybersecurity and need to maintain, enhance, administer, or secure Visual Basic console or Windows applications, Visual Basic websites, or Visual Basic scripts, then this book is for you.

At the time of writing this book, Microsoft is reducing its investment in Visual Basic. Despite that, there are many software development jobs that involve maintaining, enhancing, administering, and defending Visual Basic programs, websites, and scripts. These jobs will continue to exist for decades to come. This book is designed for software developers and web developers who find themselves in one of those jobs or want to acquire one of those jobs. This book presents all the different use cases of Visual Basic in a concise way.

## What this book covers

Chapter 1, The Visual Basic Family of Programming Languages, provides an overview of VB and its use cases over the years.

Chapter 2, Console Input and Output, teaches you how to retrieve keyboard input and send the results of computations to the computer screen.

Chapter 3, Data Types and Variables, discusses the different ways to store data in the computer memory during the program execution.

Chapter 4, Decision Branching, talks about how to build different pathways inside of our code based on an external environment such as input from the user.

Chapter 5, Iteration, shows how to repeat sections of code either a fixed number of times or until an environment is in a certain state.

Chapter 6, Functions and Procedures, dwells on modularizing sections of code for reuse. This makes it easier to have fewer bugs by only having one copy of code that does a specific thing.

Chapter 7, Project Part I, enables you to apply the programming skills you have learned to this point.

Chapter 8, Formatting and Modifying Data, explores the concept of rearranging data to use for display or for saving externally.

Chapter 9, File Input and Output, shows how to read and write data from and to files on the disk drive.

Chapter 10, Collections, discusses storing multiple values in one variable and then accessing those variables in code.

Chapter 11, Project Part II, provides an opportunity to apply the programming skills we have learned up to this point.

Chapter 12, Object-Oriented Programming, deals with arranging code into objects. Each object can have a state and actions it can perform.

Chapter 13, Inheritance, teaches us how to reuse code through the creation of taxonomies where we specify generic functionality higher in the tree.

Chapter 14, Polymorphism, shows us how to allow VB to execute the proper version of a method at runtime based on the context of the running program.

Chapter 15, Interfaces, explores code reuse through the definition of contracts for implementations that can be developed later but executed by our code.

*Chapter 16*, *Project Part III*, allows us to apply the programming skills we have learned up to this point.

*Chapter 17*, *The Request and Response Model*, discusses the web application communication model and how we send messages between partitions of our application.

*Chapter 18*, *Variable Scope and Concurrency*, explores handling multiple users on a web application.

*Chapter 19*, *Project Part IV*, provides a final opportunity to try out what we have learned.

*Chapter 20*, *Conclusions*, reviews the topics covered and suggests further learning and projects.

## To get the most out of this book

We cover many different platforms and use cases of Visual Basic in this book. You should focus on the VB family members that make sense to you at the time of reading and revisit the book later to think about the other family members. No prior programming experience is required, but if you have programmed in another language, you will find VB comes easily to you.

| Software/hardware covered in the book | Operating system requirements |
|---|---|
| VB.NET | Windows, macOS, or Linux |
| ASP.NET | Windows |
| VB6 | Windows |
| VBA | Windows |
| VBScript | Windows |

For VB6, we recommend you install Windows XP in a virtual machine such as VirtualBox.

> *If you are using the digital version of this book, we advise you to type the code yourself or access the code from the book’s GitHub repository (a link is available in the next section). Doing so will help you avoid any potential errors related to the copying and pasting of code.*

## Download the example code files

You can download the example code files for this book from GitHub at https://github.com/PacktPublishing/Learn-Visual-Basic-Quick-Start-Guide-. If there’s an update to the code, it will be updated in the GitHub repository.

We also have other code bundles from our rich catalog of books and videos available at https://github.com/PacktPublishing/. Check them out!

## Conventions used

There are a number of text conventions used throughout this book.

**Code in text:** Indicates code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles. Here is an example: “The following is sample Visual Basic.NET Windows Form code that would be stored in a file, with a name such as **program.vb.**”

A block of code is set as follows:

```
Public Class Form1
    Private Sub Form1_Load(sender As Object, e As EventArgs) Handles
    MyBase.Load
        MsgBox("Hello, World!")
    End Sub
End Class
```

**Bold:** Indicates a new term, an important word, or words that you see onscreen. For instance, words in menus or dialog boxes appear in **bold**. Here is an example: “Choose **Standard EXE** as the project type.”

*TIPS OR IMPORTANT NOTES*

*Appear like this.*

## Get in touch

Feedback from our readers is always welcome.

**General feedback:** If you have questions about any aspect of this book, email us at customercare@packtpub.com and mention the book title in the subject of your message.

**Errata:** Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you have found a mistake in this book, we would be grateful if you would report this to us. Please visit www.packtpub.com/support/errata and fill in the form.

**Piracy:** If you come across any illegal copies of our works in any form on the internet, we would be grateful if you would provide us with the location address or
