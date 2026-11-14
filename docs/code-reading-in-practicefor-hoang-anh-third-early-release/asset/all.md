![](assets/cover.png)

![](assets/cover.png)

# Code Reading in Practice

by Felienne  Hermans and Gien  Verschatse

Copyright © 2023 Felienne Hermans and Gien Verschatse. All rights reserved.

Printed in the United States of America.

Published by O’Reilly Media, Inc., 1005 Gravenstein Highway North, Sebastopol, CA 95472.

O’Reilly books may be purchased for educational, business, or sales promotional use. Online editions are also available for most titles ([http://oreilly.com](http://oreilly.com)). For more information, contact our corporate/institutional sales department: 800-998-9938 or *corporate@oreilly.com*.

*   Acquisitions Editor: Melissa Duffield
*   Development Editor: Sara Hunter
*   Production Editor: Clare Laylock
*   Copyeditor: \[TO COME\]
*   Proofreader: \[TO COME\]
*   Indexer: \[TO COME\]
*   Interior Designer: David Futato
*   Cover Designer: Karen Montgomery
*   Illustrator: Kate Dullea

*   September 2024: First Edition

Add additional revdate spans below as needed. 

# Revision History for the Early Release

*   2022-09-29: First Release
*   2023-02-09: Second Release
*   2023-10-19: Third Release

See [http://oreilly.com/catalog/errata.csp?isbn=9781098133825](http://oreilly.com/catalog/errata.csp?isbn=9781098133825) for release details.

The O’Reilly logo is a registered trademark of O’Reilly Media, Inc. *Code Reading in Practice*, the cover image, and related trade dress are trademarks of O’Reilly Media, Inc.

The views expressed in this work are those of the authors and do not represent the publisher’s views. While the publisher and the authors have used good faith efforts to ensure that the information and instructions contained in this work are accurate, the publisher and the authors disclaim all responsibility for errors or omissions, including without limitation responsibility for damages resulting from the use of or reliance on this work. Use of the information and instructions contained in this work is at your own risk. If any code samples or other technology this work contains or describes is subject to open source licenses or the intellectual property rights of others, it is your responsibility to ensure that your use thereof complies with such licenses and/or rights. PROD: Uncomment the following sentence if appropriate and add it to the above para: This book is not intended as \[legal/medical/financial; use the appropriate reference\] advice. Please consult a qualified professional if you require \[legal/medical/financial\] advice.

978-1-098-13376-4

\[TO COME\]

# Chapter 1. Structural Examination

The following chapters dive into five different dimensions of code reading: structure, domain, concepts, context, and collaboration.

I have often seen unfamiliar code and got deterred from reading on after seeing a few lines of code that did not make any sense to me, even in programming languages I was quite comfortable with. There are different reasons that code can be hard to read. For example the code can contain *code smells* like long methods, or methods with confusing or contradicting names. In other cases, code can be organized well, but hard to read because you yourself lack knowledge. A code base filled with domain concepts that you are unfamiliar with, or algorithms and data structures that you do not know will be hard to grok, even if it is written and documented perfectly.

But sometimes the confusion lies in the fact that the code is connected in ways that you do not yet know. When you are new to a code base, you are still learning what methods call which other methods, what parameters play a role in similar parts of the code base and how the important business domain concepts are represented in the code. This chapter focuses on that part of confusion about code: the lack of knowledge of how the various code elements relate to each other. Stop scrolling around in the code base in the hopes of finding understanding, or reading docs that do not yet have meaning, but instead expand your structural knowledge using focused exercises.

The exercises and techniques in this chapter help to answer questions like: Is the code organized in methods and classes? What methods and classes depend on each other? What is the ‘flow’ of the code? Are there clear *entry points* such as a main method where the code execution will start?

At the end of the chapter, you will know X techniques to be applied to more deeply understand the code structure of a new code base.

List of techniques in this chapter (to come)

# Background & Running example

Let’s study this small code snippet from the Hedy code base. Give yourself a few minutes to read the code, but do not worry if you do not fully understand it at this point.

![](assets/structural_examination_672754_01.png)

When you are reading code that is unfamiliar to you, it will be tempting to dive into all the details. What is a syntax tree? What does it mean for it to be complete, and what is a lookup table, and what are its entries?

However, these small details are likely to overwhelm your *working memory*. Your working memory acts a bit like the processor of your brain, it processes information when you read it. Research tells us that your working memory is really small, it only has about 2 to 6 “spots” to store information in! When you have only a little bit of prior knowledge, each line of code, or even each individual element might take up one spot, but if you know a lot about the code base, each method might hold a spot, causing you to be able to process a lot more. This process is called *chunking*.

As an illustration of the idea of chunking, imagine that you have to remember the numbers 1934 5023 7544 1846 and also 1918 1945 1990 2003. Most likely the second number set is a lot easier to understand since you can remember it using prior knowledge: First World War, Second World War, First Gulf War, Second Gulf War. Those four chunks make the second example easy to process, but lacking such chunks the first string of numbers will be extremely hard to remember or reason with.

This example underlines why it is so important to first gain some deeper understanding of the concepts and their relationships: without solid prior knowledge of the structure of the code, it will be harder than necessary to read unfamiliar code.

For the example above, this means that we will need to understand the meaning of the basic building blocks of the code: parse tree.

# Structural components

Now that you understand why it is important to understand the structure of code, you can start examining a number of techniques to gain this understanding. One of the first techniques to apply when reading unfamiliar code is to understand its division into components; this includes classes and methods in an Object-oriented project, or functional components in a more functional style program. It is tempting to start to read individual lines of code when you examine unfamiliar code for the first time, but as explained above, focusing on small details can be confusing and also feel overwhelming. Starting your analysis with the structural components instead provides a high level view that will aid in your more detailed line-by-line read later.

## Objects and functions

When we examine the code in the code snippet for this chapter, we immediately see that there are different types of code present. We see a class called `IsComplete`, a function called `is_program_complete()` and variables that are not included in classes, such as `command_make_color`. When finding our grounding in a new code base, it will help our working memory if we explicitly understand the components and their roles. Also, each type of component will be read in a different way: you go reading a class in a different way than a method, for example because you can step through a method. We therefor start with a division of code into three types of structural components:

1.  Firstly there are *object components*, which are used to create meaningful abstractions. Examples of object components in code are user-created datatypes, structs or classes. In the running example, `IsComplete` is an object. We will use Python naming conventions in the remainder of the book, so objects such as classes will be written in PascalCase.
    
2.  There are also *functional components* in which execution is grouped, like methods or functions. In our example `is_program_complete()` is a method. The convention in the remainder of the book will be to denote functions and methods with `()` at the end.
    
3.  Finally code can contain  *singular components* that are variables or constants not contained in either objects or functional components, such as `command_make_color`. We will use Python naming conventions in the remainder of the book, so objects such as classes will be written in `snake_case`.
    

## Components in different paradigms

These three components appear in different forms in different programming languages, and not all languages support all three objects. For example, objects are, of course, commonly present in object-oriented languages, but these languages traditionally do not allow for variables to be defined outside of classes and thus do not have variables or constants living outside of object or functional components.

Functional components on the other hand are, of course, the core component used in functional languages. In object-oriented languages, functional components take the form of methods within classes.

Singular components are not possible in most pure OO languages like Java or C#, components that are not objects must live within classes and as such there are no singular variables outside classes. Inside classes they take the form of fields. In functional languages variables may always be declared, inside or outside functions.

Nowadays, many languages allow for multiple paradigms including functional and object-oriented, for example Python or JavaScript. In these languages all three components can (and will often) be present.

## IDE support for structural reading

While Interactive Development Environments (IDEs) are mostly designed for writing code, they also contain a lot of features that can help in reading. Many IDEs support a code folding or collapse feature, which helps to manage the complexity in reading by showing only a summarized view of functions, methods or loops, rather than their implementations. For a method/function or class, the minimized view will show the definition, including parameters for method/functions, and including inheritance information for classes. For loops, typically the line containing the loop code (for, while, forEach) is shown in full, while the loop’s body is hidden.

### Code folding

In most cases, code folding will be done with a little minus in the *gutter*, the side bar typically placed next to code also containing line numbers, as shown in Figure 2-1. Many IDEs also have shortcuts to fold code. In Jetbrains IDEs like PyCharm or CLion ctrl (on Windows) or command ( on Mac) combined with - or + folds or unfolds the current block, while ctrl (on Windows) or command ( on Mac) combined with shift and -/+ folds or unfolds everything in the file.

`Ctrl+NumPad - / Ctrl+NumPad +` . PyCharm folds or unfolds the current code fragment, for example, a single method. To collapse or expand all code fragments, press `Ctrl+Shift+NumPad - / Ctrl+Shift+NumPad +`

![](assets/structural_examination_672754_02.png)

This is a great way to view the components without getting overwhelmed by the details. This view however makes it harder to see how big the individual components are, which can limit your insights into the code base, because size can be a useful indicator of priority. Larger functions tend to play a bigger role in a code base than smaller ones. You can work around this limitation by looking at the line numbers. Figure 2-2 shows, for example, a constant assignment starting on line 86 of 12 lines, and a method definition on line 99 containing 52 lines.

![](assets/structural_examination_672754_03.png)

While it might seem surprising, reading code in this folded fashion can already help you to gain initial understanding. For example, the folded view will show you which components are there, what their types are, how many components there are of the different types, and how their sizes compare to each other.

### Structure view

Another way of seeing all components in the code is using the Structure view which many IDEs contain. That view, demonstrated in Figure 2.1 shows an overview of the three types of components: objects marked in blue with C, singular variables marked in yellow with v and functions marked in pink with f.

![](assets/structural_examination_672754_04.png)

Classes can be opened, revealing the methods inside, as shown in Figure 2.2. Methods are also marked in pink, but with the letter m instead of f.

![](assets/structural_examination_672754_05.png)

### Exercise 2.1: List all components

  
Open the Hedy repository, navigate to the hedy.py file. The snippet above can be found at: [https://github.com/hedyorg/hedy/blob/ba6834b4a46edf1aac67c9d9e017bc279535062a/hedy.py#L2861](https://github.com/hedyorg/hedy/blob/ba6834b4a46edf1aac67c9d9e017bc279535062a/hedy.py#L2861). Give yourself a bit of time to orient, and then review only the components. What objects components (classes or structs) does this code snippet contain? What functional components does the code have (methods or functions)? How big are the components? Do they look balanced in size? Are there any large or small ones that catch your attention? And what singular components (variables, constants) does the code contain?

Fill out the table below to guide your reading. In this exercise and all following in the book, a few fields will have been filled out in *italics* to help you get started.

| Object components  |
| --- |
| **Component name**  || **Lines of Code**  || **Size (small/medium/large)**  |
| *ParseTree*  || *267*  || *Large*  |
| *Variable*  || *53*  || *Small*  |
|    ||    ||    |
|    ||    ||    |
| **Functional components**  |
| **Method/function name**  || **Lines of Code**  || **Size (small/medium/large)**  |
| *parse()*  || *121*  || *Large*  |
| *execute()*  || *53*  || *Medium*  |
| *checht\_parse\_tree()*  ||    ||    |
| **Singular components**  |
| **Variable name**  ||    ||    |
| *characters\_that\_need\_escaping*  ||    ||    |
| *character\_skuplt \_cant\_parse*  ||    ||    |
|    ||    ||    |

# Relationships between components

Now that the core three components of the code have been identified, it is important to understand the relationship between them. Again, it can be tempting to start to read the code to investigate its functionality in detail, but there is a lot still to be learned from how the components relate without even looking at the functionality of the code. Chapter X outlines a number of techniques to examine details of the code.

An important part of the structural dimension of code is not only the components but also the way they are connected to each other. To understand why the relationships in code are important, you need to understand a bit more about how people process new information in general. When understanding new information, your brain always tries to relate new information to things you already know or understand. As such, your brain forms a *mental model* of the information that you can use to reason about the new information. Explicitly making connections between information visible will support your brain in easier understanding.

When you read very unfamiliar code, you have no model of the code in your long-term memory. Simply scrolling through the code will give your brain some nuggets of new information, but it will not directly help to form a mental model. Making the relationships explicit and storing them in a table or graphs supports your memory in quickly processing new insights about the code.

When reading unfamiliar code, and sometimes even when reading familiar code, it can be hard to get a good overview of all the dependencies in the code base. Often, when bugs occur in code, an overlooked dependency is the cause, for example when a method is not called which should have been. So, for a deeper level of understanding code, understanding the connections within the code is an important first step. It will help you form a mental model of the code and as such, strengthen your prior knowledge and thus make it easier to reason about the code.

Let’s dive into the types of relationships that code can contain, since three different types of components can relate to each other in different ways. Table 2-1 presents an overview of the different relationships.

Firstly, when object components are classes, two components can relate to other classes with inheritance in object oriented languages. For example, because one class inherits from another class, but also when both classes inherit from a similar base class. Classes can also contain functional components as methods and classes.

In some languages functional components can use objects as parameters, and functions can call static methods defined on classes. Furthermore functional components can call each other creating a dependency between two functional components in both object-oriented and functional languages. In object-oriented languages methods can also be related because one method overrides the other one. Singular components can be used inside the body of functional components.

Singular components do not depend on the other two object types.

Table 1-1. Different ways in which components can relate to each other 

 
|  ️ || Object Component || Functional Component || Singular Component |
| --- | --- | --- | --- |
| Object Component || Inherits from (OO) || Contains as method (OO) || Contains as field (OO) |
| Functional Component || Uses are parameter (OO, FP)  
Calls on static method (OO) || Calls (OO, FP)  
Overrides (OO) || Used in body |
| Singular Component || \- || \- || \- |

## IDE support for dependency analysis

The IDE can help you to determine relationships between components in different ways.

### Search

A simple and quick way to understand dependencies in a code base is using **search functionality**. While you will probably use search functionalities all the time when writing and debugging code, you can use the IDE in a slightly different way when specifically looking to find relationships, by searching when the code is **folded**.

When you search for names of constants, variables or methods when the code is folded, results will be highlighted in the folded code blocks. This gives a quick overview of usage of constants and variables, or the calling of methods. For example, suppose you want to know what methods use the PREFIX\_CODE constant defined in the code snippet above. See figure 2-3 for the results that appear when searching the code for the string PREFIX\_CODE in the search bar.

![](assets/structural_examination_672754_06.png)

This view can immediately help you find the specific dependency of object or functional components on singular objects (in figure 2-3 on the singular object PREFIX\_CODE).

Of course, string matching is limited. In fact, the full code base from Figure 2-3 has a second constant called TURTLE\_PREFIX\_CODE that would also be found with a search for PREFIX\_CODE in other places in the code (which are not shown in the figure).

### Dependency features

A second, more powerful method to examine structure is to use **dependency features** in the IDE to list dependencies explicitly. In many IDEs, right clicking a constant, variable or method will reveal the option to list all usages, as shown in [Figure 1-7](#ch01.html#fig_6_find_usages_feature_shown_in_pycharm).

![](assets/structural_examination_672754_07.png)

The list of usages which the IDE now shows will also be the usages of PREFIX\_CODE in functions and objects.

In most IDEs, find usages-features do not just find dependencies on variables, they can also detect other relationships including class inheritance and function calls. However, often IDEs do not clearly distinguish between different types of relationships, which can be confusing when you are trying to form a mental model of the code.

For example, figure 2-5 shows the result of finding dependencies for a class called `ConvertToPython`. You can see an inheritance relationship in line 1186, as well as calls to static methods (lines 1359 and 1479) in the class.

![](assets/structural_examination_672754_08.png)

### Exercise 2.2: Relationships

\[To Come\]

## Informal relationships in code

So far we have examined formal relationships, but often code components also have informal relationships, which by default can’t be found by tools. We will therefore expand our understanding with a bit of manual analysis and look for these connections. You can do this by lightly browsing the code and just let things jump out at you. Don’t worry if you miss something.

### Connections based on names and comments

A first example of informal relationships between code components are comments, like a comment in one class saying something like “this class is similar to class X and we should consider merging them”. This is not a formal relationship between classes like inheritance but does give us more depth in understanding the code. Another one is similar naming. In our running example, `IsComplete()` and `IsValid()` share both a formal relationship because they have the same superclass, but they are also similar in name, and play a similar role on the call site.

### Connections based on parameters

Another more informal type of relationship that can exist between components and requires some manual inspection are the relationships that can occur at the parameter level. Functional components can be connected when they use the same parameters frequently, which Fowler calls a Data Clump. Even if functions are not formally connected by being grouped in a class, they can still be similar because they operate on the same type of objects. For example, when your code base contains a Canvas object that you write information to, multiple classes can have methods that use that Canvas as a parameter without calling each other. While these methods lack a formal connection, they do have a relationship, one that is hard to determine without manual inspection of the method declarations.

### Exercise 2.3: Examine parameters

Examine the parameters of the functions of methods in your code base from Exercise 2-1. If you have done exercise 2.2 you can simply copy all function lines. Now list the parameters that occur in these functions as column headers and mark cells with an X when a function uses a certain parameter.

| Component || program\_text || skulpt || URL || … |
| --- | --- | --- | --- | --- |
| *parse (F)* || *X* ||   ||   ||   |
| --- | --- | --- | --- | --- |
| *execute (F)* || *X* || *X* || *X* ||   |

This analysis might help you find patterns of methods that are linked because they work on the same parameters, and thus will share a number of Xs in a column.

You might want to note some of these parameter connections in the table of exercise 2.2, especially if there is a strong overlap in parameters and a similarity between functionality.

Finally, when you are doing any kind of analysis manually, it can be tempting to dive deep into the code, but it is better in this phase to try to limit yourself to finding the dependencies. Later chapters cover other exercises looking at names, code quality, and other aspects of the code.

## A top to bottom read

Now that we have formed a first basic understanding of the code base and its components, it is a good time for a time to bottom read, without relying on IDE features. While IDEs can help illuminate dependencies, reading the entire code snippet from the top to the bottom can also be very useful, because you are then building a more complete model of the code. But it is important to timebox this activity or you end up going into a rabbit hole and getting frustrated by a lack of understanding. Use, for example, 10 minutes and read the code in that time. Keep your diagram handy and note down any new information.

# Entry points

Now that you have gained some initial understanding of the different components and their relationships to each other, it is time to make a plan for reading the code. You’ll do that firstly by locating an *entry point.* An entry point is a point where you will start reading.

There are many different ways in which you can read code, which I discuss later in this book, in Part Z. For now, let’s focus on reading by following the execution flow of the code.

## Finding entry points

While it is not always clear where unfamiliar code starts, often, you can guess a few places in the code where you expect the execution to start, like a main() method in a C like language, code guarded by an if \_\_name\_\_ == “\_\_main\_\_”: in Python, or a method related to the /index route in a web application. Finding generic entry points can be done with text search, if you have a good guess of what the name of the starting method will be. For example, it can be main in many C programs, a controller in an MVC app, or a default route like /index.html for web apps. Unit tests can also serve as great entry points, but will will dive deeper into reading test code specifically later in the book.

However, sometimes you will lack a good guess of where the execution starts. In such a case, you can use a table like the one you constructed in Exercise 2.2 to help find another entry point. First, ignore the object and singular components and just focus on the rows and columns of functional components. Now look at the functional components that have an empty or almost empty column, and also have a full row. These are functional components that are not, or hardly called but do call a lot of other functions and as such can be interesting places to start an exploration.

## Reading from the entry point

Now that you have identified an entry point, you can start to build a mental model of the remainder of the code. You can again use the IDE to help you understand code dependencies by placing a breakpoint on the entry point and stepping into each dependent code block examining the **slice** of code that you are interested in. A slice is a part of the code that relates to a certain function or variable and is exactly that part of the code that this variable is used in or that this function leads into.

If you want to step through a slice of code, in most IDEs you can place a breakpoint such that the code will stop when the execution reaches the breakpoint. You can do so by clicking in the gutter next to the code, which will create a breakpoint indicated by a red dot as shown in Figure 2-6.

![](assets/structural_examination_672754_09.png)

Once you have placed the breakpoint, you can follow the execution path using the IDE to step into each of the called functions. Again, your instincts might be to dive into each method in detail and read all the inner workings. But at this level of reading, the concrete goal is to scratch the surface of the code and not go too much deeper, so as not to tax your working memory. Don’t worry, that phase will come later!

A good strategy at this point is to try to contextualize the relationships that you have, summarizing the why of the relationship in one or two sentences.

### Exercise 2.4

Follow the call stack of your chosen entry point and read from there. Fill the table with the relationship types between the functions. Describe the role of the dependency in one or two sentences.

| Component || parse (F) || execute (F) || validate\_tree(F) |
| --- | --- | --- | --- |
| *parse (F)* ||   ||   || *calls the validate\_tree()n function to determine if the parse tree can be processed* |
| --- | --- | --- | --- |
| *execute (F)* || *calls the parse() function to obtain the transpiled code which then can be executed* ||   ||   |
| *validate\_tree(F)* ||   ||   ||   |

You can find the dependencies manually or using the IDE. While using the IDE can feel easier, stepping through the code can be useful but is also likely to be overwhelming. If the IDE will step into a piece of code that is located far away from the current line, you will have to reorient yourself in the codebase again, and you will easily lose track. This is especially true if there are pieces of code in which multiple execution paths come together, like when a function gathers information from several other methods and combines them. For example, the function print\_ask\_args below first checks the variables in all arguments (and throws if that is not possible) and then transforms each of the arguments in the next line.

```
def print_ask_args(self, args):
   args = self.check_var_usage(args)
   transformed_args = [self.process_variable_for_fstring(a) for a in args]
   return ''.join(transformed_args)
```

In such complex cases, manual reading aided by search might be less demanding and thus more effective than “hopping” through the code using the IDE using breakpoints.

In addition to making notes in a dependency table (see exercise 2.3), visualizations as Unified Modeling Language (UML) class diagrams like the one in figure 2-X can help support this type of code reading as well. In a class diagram we described the classes in a system as boxes containing their fields and methods. For example in figure 2-X, `Client`, `Receiver`, `Invoker`, `Command` and `ConcreteCommand` are classes and `execute()` is a method of `Command` and `ConcreteCommand`.

If you are looking for specific information, it is especially important to keep an eye on your history, and determine if what you are reading is helping you to answer concrete questions about the code.

![](assets/structural_examination_672754_10.png)

## Different kinds of entry points

Your prior knowledge of a code base will impact what entry point to use, as well as the activities you are performing. For example, when you are looking at a codebase that is quite unfamiliar to you, starting at a generic entry point like a main method can be helpful because it helps you build a mental model of what happens in the code in the order that it will happen at runtime too.

But in a more familiar code base, it is more likely that you will start with a context-specific entry point, and which type depends on the activity. For example, while debugging, a failing test will be a natural entry point to start your reading process from. Later in this book, in chapter X we will dive into the process of code reading when debugging is depth. While optimizing, a line flagged by the profiler is a good place to start. Later chapters of this book, particularly in Part 2, will dive into different types of reading scenarios with different types of entry points.

# After the read

During the structural examination described in this chapter, you have gathered a lot of relevant information about the structure of the code. If you have applied these exercises not only to the example code snippet in this chapter, but also to a code base that you are actually reading, there are a lot of things you can do with your newly gained understanding of the code base.

In many cases, you could commit some of the information you gather back into the code base. This information might help you if you decide to explore the code in more depth, for example using exercises later in this book, but it might also help newcomers after you find their way in the code base. In this section we outline some ways in which the gathered information can be useful.

## Expand documentation

While writing documentation might not be your first priority, however, research shows that actively processing information you have just learned is a great way to store that information in your long time memory. So trying to write a little write up of your findings is going to be valuable to you, and it might also be valuable to add to the project.

A useful exercise at this point can be to deliberately simplify the results of previous exercises. The results of exercises 2.1 and 2.2 can serve as good starting points for written documentation. Don’t just commit every single table with every single entry, but actively think how to summarize it. This is both valuable for you, and for the project; committing all results will likely be overwhelming for future readers. The simplifying/summarizing will further consolidate your understanding.

## Improve comments

Some information that you have found while reading can also be committed to the source code itself. The brief text snippets resulting from exercises 2.3 could, if this information was not yet present in the code, be placed right before a method call.

## Discuss code quality

Sometimes while examining the code from a structural perspective, you will find parts of the code that have code smells, specifically structural code smells. Here are some to look out for.

###### Note

*Code smells are generally separated into two categories: structural and linguistic*

Structural code smells are the code smells that come from Fowler’s original book, and refer to the way code is organized. Examples of such smells are *Message chains* in which methods call other methods, which in turn call other methods etc, *long parameter lists* or *large classes*.

The other type of code smells are linguistic code smells as defined by Venera Arnaoudova. Linguistic code smells refer to names of variables or classes that do not aptly describe what they do. That can be vague names such as “fetch” or names that omit part of the functionality, such as a conversion method that converts but also stores information about the conversion in the database.

Size
: 

Looking at the sizes of objects alone can already reveal bloated classes, data classes or very small classes. The analysis of size you performed in Exercise 2.1 can point you to this particular code smell.

These findings can serve as a great conversation starter with your team to talk about code history and code quality. Here are some questions to consider based on this analysis:  

*   Is there a reason these classes are large, small or only contain data?
    
*   Is this a historical reason or is there a good justification for the size of the classes?
    

Smells in relationships
: 

Other code smells pertaining to object components are inappropriate intimacy and feature envy, which both point to two or more classes that have too much interaction to be useful. You can detect these code smells by looking at the table that you created in exercise 2.2 and examining if there exists a number of classes that often work on each other’s fields (inappropriate intimacy) or one that very often class fields or classes of another (feature envy). Again, there might be good reasons for this design, but it is something to discuss, for example:  

*   Do we understand why these classes are intimately related?
    
*   Are there alternative ways in which the classes could be organized?
    

Smells in functional components
: 

The smells above concerned object components, but there are also code smells of functional components that can be found with structural analysis. For example, the results of exercise 2.2 is a message chain. Start at an entry point in the table and follow the functional components it calls, and then the functional components that that component calls, etc. If you have to make a large number of “hops” in the table, that indicates the presence of a message chain. Now, message chains are not necessarily bad, however, if you find one you can discuss whether:

*   Some of the methods on the path are so small they could be merged?
    
*   There are specific scenarios in the code that call long chains that could be solved differently, for example with caching or early returns?
    

Another smell which can be found with structural analysis is dead code. Are there any methods that are not called at all? These might represent dead code. Be sure to also examine alternative ways in which dead code might be used, such as calls from the test suite for example, or from other parts of the code base. This is especially relevant in multi-lingual code bases in which static source code analysis might not always find dependencies.

## Refactor

When you find code that has code smells using structural analysis, sometimes you will want to improve the code. If you decide to do a refactor, the analysis that you performed so far can also guide the refactorings.

Large class
: 

If you find large classes in the size analysis, these classes could be split up. A relationship table like the one you created in exercise 2.2 can help you find groups of methods that call each other. If two or three subgroups exist in these methods, the large class could be split into those subgroups without causing too many dependencies between the new classes.

Small class
: 

On the other hand, if you find small classes in your analysis, you might want to merge them into another class. The relationship table like the one you created in exercise 2.2 can help you decide which class would be a good home for the small number of fields and methods in the small class. Ideally this would be a class with which this small class shares the most fields and methods.

Data class
: 

For a data class (a class that has a lot of fields but no or hardly any methods) a similar analysis can be made; this class could be merged with a class that often accessed the fields of the data class. If there is no such class, maybe the code

Long parameter list
: 

In some cases, you will find methods or functions with long parameter lists. These can be found in exercise 2.3 as lines with a lot of Xs. In the case of methods on a class, these long parameter lists might be shortened by moving some of the parameters to be fields on the class. In functions or methods, you might group some of the parameters into larger objects. For example, a function that has x, y and z as parameters might be refactored into a function that uses just the point data type, which then contains x, y and z fields.

One call functions
: 

In exercise 2.3 you might have identified methods that are only called in one place. This is not necessarily a smell as defined by Fowler, however such organization might point to premature generalization where an abstraction is introduced that is not used (yet). While this might be worth it in some cases, it will also cause the editor to “hop” when reading and debugging, causing more cognitive load because your brain has to reorient itself. These methods might be inlined reducing that cognitive load, at the expense of making the method in which the method or function is inlined longer.

Dead code
: 

If you found dead code in examining the relationships in the code, you might want to remove that dead code when you have verified it is indeed no longer needed.

# Summary

Code can be read in different dimensions: structure, domain, concepts, context, and collaboration. This chapter explored the structural dimension and looked at how variables, functions and objects in code can relate to each other, and how that information can be used while reading code. The following chapter explores domain.

^([1](#ch01.html#id134-marker)) See “Eye Movements in Code Reading: Relaxing the Linear Order” by Teresa Busjahn et al. (2015), https://ieeexplore.ieee.org/document/7181454.

^([2](#ch01.html#id136-marker)) See for example: https://medium.com/nerd-for-tech/how-to-generate-uml-diagrams-from-your-existing-code-814d27bd1537

^([3](#ch01.html#id137-marker)) https://www.sciencedirect.com/science/article/pii/S1045926X02902375&sa=D&source=docs&ust=1658912480042497&usg=AOvVaw3mvAJmKBMgrgVt4HM9Ez13

# Chapter 2. Understanding the Business Domain of Your Code in More Depth

The previous chapter examined relationships in code, without diving into the content of the code in depth. This chapter will do the reverse and examine the content of the code without looking at the way it is connected. We will specifically look at terms and concepts within the business domain of the code, in other words, the real-world problem that the code is solving.

When you are reading unfamiliar information such as unfamiliar code, your brain will be engaging in a lot of different activities at the same time. You are trying to *understand* the structure, how it all relates to each other using your working memory. In the previous chapter we have learned techniques to support your working memory in examining relationships.

But you will also be *remembering* information that you have already stored in your long-term memory. Doing these things in parallel is very energy consuming and will make it harder to understand code. It is therefore important to concentrate on the domain concepts in an unfamiliar code snippet and verify there is no confusion there, before you engage in more complex activities like debugging or reviewing.

This chapter will not be useful for all code reading purposes. If you are working with a code base of which the domain is very familiar to you, for example because you have been working in a certain field, like insurance or pensions for a long time, the domain concepts will not be overwhelming for your brain. But if you are just getting used to a new code base for a new customer or if you are trying to get to know a new open source project that uses new terms, examining and learning domain concepts from code will greatly ease further engagement with the code.

Therefore, this chapter solely examines the business domain concepts present in the code. With domain here we refer to the business domain or problem of the code. The programming domain will be covered in Chapter 4. Examining the domain concepts is useful because if you miss a thorough understanding of what words in the code mean precisely, it will be hard to understand the code as a whole. This relates to your long-term memory, as discussed in Chapter 2: if you do not have some prior understanding, remembering and analyzing code will be a lot harder than needed.

Variable and method or function names can be great sources of information about the domain aspects of code, since they will very often contain words used in the domain. In his book Patterns of Enterprise Application Architecture, Martin Fowler describes a domain model as having two components: behavior and data.^([1](#ch02.html#id139)) Words that occur in variables often point to either data or behavior: verbs will often refer to behaviour, while nouns refer to objects. So in a sense, in this chapter you will be looking at objects and their functionality, but you will gather the information from names rather than from the structure of the code.

The exercises and techniques in this chapter thus focus only on the domain aspects of code, helping you answer questions like: What are the central domain concepts in this code base and how do they relate to each other?

At the end of the chapter, you will know X techniques to be applied to more deeply understand the domain of a codebase:  

*   List all variable names
    
*   Identify different parts of variable names
    
*   Look at name molds
    

# Running example

As in the previous chapter, we will use one example code snippet in this chapter. Don’t worry if it looks entirely overwhelming! We will go through it step by step.

## Example code: Bracket colorization in the Monaco Editor

```
/*---------------------------------------------------------------------------------------------
*  Copyright (c) Microsoft Corporation. All rights reserved.
*  Licensed under the MIT License. See License.txt in the project root for license information.
*--------------------------------------------------------------------------------------------*/
import { Emitter } from '../../../../base/common/event.js';
import { Disposable } from '../../../../base/common/lifecycle.js';
import { Range } from '../../core/range.js';
import { editorBracketHighlightingForeground1, editorBracketHighlightingForeground2, editorBracketHighlightingForeground3, editorBracketHighlightingForeground4, editorBracketHighlightingForeground5, editorBracketHighlightingForeground6, editorBracketHighlightingUnexpectedBracketForeground } from '../../core/editorColorRegistry.js';
import { registerThemingParticipant } from '../../../../platform/theme/common/themeService.js';
export class ColorizedBracketPairsDecorationProvider extends Disposable {
   constructor(textModel) {
       super();
       this.textModel = textModel;
       this.colorProvider = new ColorProvider();
       this.onDidChangeEmitter = new Emitter();
       this.onDidChange = this.onDidChangeEmitter.event;
       this.colorizationOptions = textModel.getOptions().bracketPairColorizationOptions;
       this._register(textModel.bracketPairs.onDidChange(e => {
           this.onDidChangeEmitter.fire();
       }));
   }
   //#region TextModel events
   handleDidChangeOptions(e) {
       this.colorizationOptions = this.textModel.getOptions().bracketPairColorizationOptions;
   }
   //#endregion
   getDecorationsInRange(range, ownerId, filterOutValidation) {
       if (ownerId === undefined) {
           return [];
       }
       if (!this.colorizationOptions.enabled) {
           return [];
       }
       const result = new Array();
       const bracketsInRange = this.textModel.bracketPairs.getBracketsInRange(range);
       for (const bracket of bracketsInRange) {
           result.push({
               id: `bracket${bracket.range.toString()}-${bracket.nestingLevel}`,
               options: {
                   description: 'BracketPairColorization',
                   inlineClassName: this.colorProvider.getInlineClassName(bracket, this.colorizationOptions.independentColorPoolPerBracketType),
               },
               ownerId: 0,
               range: bracket.range,
           });
       }
       return result;
   }
   getAllDecorations(ownerId, filterOutValidation) {
       if (ownerId === undefined) {
           return [];
       }
       if (!this.colorizationOptions.enabled) {
           return [];
       }
       return this.getDecorationsInRange(new Range(1, 1, this.textModel.getLineCount(), 1), ownerId, filterOutValidation);
   }
}
class ColorProvider {
   constructor() {
       this.unexpectedClosingBracketClassName = 'unexpected-closing-bracket';
   }
   getInlineClassName(bracket, independentColorPoolPerBracketType) {
       if (bracket.isInvalid) {
           return this.unexpectedClosingBracketClassName;
       }
       return this.getInlineClassNameOfLevel(independentColorPoolPerBracketType ? bracket.nestingLevelOfEqualBracketType : bracket.nestingLevel);
   }
   getInlineClassNameOfLevel(level) {
       // To support a dynamic amount of colors up to 6 colors,
       // we use a number that is a lcm of all numbers from 1 to 6.
       return `bracket-highlighting-${level % 30}`;
   }
}
registerThemingParticipant((theme, collector) => {
   const colors = [
       editorBracketHighlightingForeground1,
       editorBracketHighlightingForeground2,
       editorBracketHighlightingForeground3,
       editorBracketHighlightingForeground4,
       editorBracketHighlightingForeground5,
       editorBracketHighlightingForeground6
   ];
   const colorProvider = new ColorProvider();
   collector.addRule(`.monaco-editor .${colorProvider.unexpectedClosingBracketClassName} { color: ${theme.getColor(editorBracketHighlightingUnexpectedBracketForeground)}; }`);
   const colorValues = colors
       .map(c => theme.getColor(c))
       .filter((c) => !!c)
       .filter(c => !c.isTransparent());
   for (let level = 0; level < 30; level++) {
       const color = colorValues[level % colorValues.length];
       collector.addRule(`.monaco-editor .${colorProvider.getInlineClassNameOfLevel(level)} { color: ${color}; }`);
   }
});
```

## Reading the example snippet

This chapter will contain specific exercises to help you engage with the example snippet, but of course, you might want to get a sense of the code before reading on. A great way to do that is to now use the exercises from the previous chapter, to understand the structure of this code a bit more before we dive into the domain.

While it is relatively easy to figure out more about the domain of the code using Google, I would recommend you to not do that at this po

## Context

The example code we will study this is code from Monaco,the editor in Visual Studio code.  
  
If you want to examine the code in the browser or in the IDE, you can find it here:

[https://github.com/hoathaivan/nextjs-monaco-ls/blob/d31e9482265eddb70459e0a253b85d7c94bcce80/public/monaco-editor/esm/vs/editor/common/model/bracketPairsTextModelPart/colorizedBracketPairsDecorationProvider.js](https://github.com/hoathaivan/nextjs-monaco-ls/blob/d31e9482265eddb70459e0a253b85d7c94bcce80/public/monaco-editor/esm/vs/editor/common/model/bracketPairsTextModelPart/colorizedBracketPairsDecorationProvider.js)

# Learning from variable names

In the previous chapter, you folded all the code so you could focus on just the structural elements of the code. In this first technique to understand the code’s domain, you’ll do the reverse: you’ll just list all domain elements and disregard the remainder of the code including the structural components. While we can read code in an IDE, they are ultimately created mainly as code *writing* tools, and as such do not typically contain features that are only useful in code reading, such as the option to create a list all the different identifier names used in a file or project. We will therefore need to use a bit of manual effort.

## Gathering domain insights from variable names

To explore all domain concepts, one method is to list all variables in use in a file that you are examining. Ultimately, you might be interested in all names in an entire code base, but let’s start with a smaller scope for now. In Chapter 5, we will explore techniques to explore the surroundings of a code base including documentation and repositories.  
  
A first step in learning about domain concepts in a code base is to write all variable names down in a text file or markdown file, but it can also be a piece of paper. While doing this exercise you will want to fully focus on variable names. It will be very tempting to browse the code a bit more, and of course, there is nothing wrong with reading some code around the variables, but try to keep your eyes on the goal, and especially do not let yourself be discouraged by the code itself.

### Exercise 3.1 List all names in the codebase

For this exercise, simply list all names in the codebase. Names here are all the things that the programmer (not the language designer) chose, including variables, parameters, classes and functions/methods. Keep track of the type of each variable to make further processing easier.

Focus on the names only and don’t spend all too much time on this, we compile this list to work with it further later in the chapter.

| Name || Type |
| --- | --- |
| *ColorizedBracketPairsDecorationProviders* || *Class* |
| --- | --- |
| *bracketsInRange* || *local* |
| *getInlineClassName()* || *Method* |
|   ||   |
|   ||   |
|   ||   |

Once you have listed all variable names, you can start to dive into the domain more. As we have seen in Exercise 3.1, variable names are very often compounded of a lot of different parts, like `independentColorPoolPerBracketType`.

Variable names often contain separate parts such as **quantifiers**, and **programming concepts** but also **domain concepts**.

**Quantifiers** are parts of names that refer to quantities, like count, number\_of, maximum, or filtered. Sometimes these quantifiers also include with is being quantified over, like maximum\_refund\_per\_month.

**Programming concepts** also occur within names, for example from a name like m\_object, object can refer to a specific concept in a language. Other parts of names that refer to concepts are list, hash, table, string, or number. In this chapter, we will not yet focus on concepts now, that is the topic of chapter 4.

**Domain concepts** refer to the domain, like customer, import, parser, code, or language. Some parts of variable names point to concepts, often in the form of nouns, like customer or code, but they can also represent *domain behavior* in the form of verbs like import, or parse.

Picking apart the names, you can get an introductory sense of what concepts and behavior play a role in the code base.

# Extracting domain concepts automatically

In addition to the manual approach, you can also use tools to extract domain concepts. While text analysis tools are typically meant for text, they could be applied to code too, for example: [https://pypi.org/project/wordcloud/](https://pypi.org/project/wordcloud/)

  
Once you have gathered all names in our example code snippet (or in your own file or module that you are currently investigating) our next step is to determine what concepts in the code are even part of the domain. Because if we want to understand the domain better, we first need to know what concepts are in the domain. We will start by splitting the variable names into different words, and then determining which words in names are domain concepts, which is not as straightforward as you might think.

Firstly, sometimes even finding the parts is not easy. For example, consider the name getInlineClassName(). Are we getting the name of an Inline Class? Or are we getting an Inline Name of a Class? In other words, is Inline an attribute of Class or of Name? You might need some further reading of the code to even know!

And, even if you can find all parts, what a domain concepts is is not always clear. For example, the name ColorizedBracketPairsDecorationProvider can be split up into five words: Colorized, Bracket, Pairs, Decoration and Provider.

However, without further knowledge of what this code does, it is hard to know what is a programming concept and what is a domain concept: Decoration and Provider can refer to programming concepts, Pairs could be a datatype, and even Brackets is something that occurs in code.

We will therefore potentially have to gather more knowledge about what the code does to make some of the decisions for Exercise 3.2. See what you can understand, and don’t overthink it, it you don’t know, feel free to place concepts in the Unclear column

### Exercise 3.2 Distinguish different parts within the names

From the list of variables from exercise 3.1, extract the separate words within the variable names, and divide them into quantifiers, programming concepts and domain concepts. If you don’t know, just put the concept in the Unclear column.  

| Name || Quantifiers || Programming || Domain || Unclear |
| --- | --- | --- | --- | --- |
| *ColorizedBracketPairsDecorationProvider* ||   || *Provider* || *Colorized*  
*Bracket Pair* || *Decoration* |
| --- | --- | --- | --- | --- |
|   ||   ||   ||   ||   |
|   ||   ||   ||   ||   |
|   ||   ||   ||   ||   |
|   ||   ||   ||   ||   |
|   ||   ||   ||   ||   |

If you have access to domain experts (which is not always the case if you are reading code from an open source project), this is a good point to discuss the list of concepts. Great questions to ask are:

*   Are there any synonyms in the list of domain concepts that might not be obvious?
    
*   Are there any central domain concepts or behaviours that this list misses?
    

# Determining relationships between Domain Concepts

Once you have formed an initial idea of the central domain concepts, we are going to explore how the concepts fit together to form a more coherent picture of what the domain entails. We will work with the domain concepts that we have gathered so far in Exercise 3.1 We will dive into what type these concepts are, using the terminology of Chapter 2.

In Chapter 2 we have distinguished objects and functions, and that distinction is relevant in the domain dimension too. In a domain, we also have objects, like Decorations, that can be used in functions like getDecorationsInRange(). When examining the domain, one more distinction is useful, between objects and object attributes. Objects like Bracket Pairs can have attributes like Colorized because in this domain a pair of brackets can get a certain color. So Colorized it not a domain concept in itself but a modifier used with Bracket Pairs.

The types we have gathered in Exercise 3.1 can also help in determining the component types of the domain concepts. Functions tend to contain more generic words, especially verbs such as execute, read, load or create. However function names often also reveal domain-specific behavior such as buying, selling, placing an order or favoriting an item. And function names are not limited to behavior. Function names can also reveal domain concepts; for example, a function name such as getDecorationsInRange() tells us that there is such a thing as a range that decorations fall into.

## Mapping concepts into components

List all names and their types from Exercise 3.1, and all concepts in the names from Exercise 3.2 in the gray columns. These go in the first 3 gray columns of this table, and are not new information. Now fill out the final column, dividing the concepts into Object or Function components, or Attributes.

### Exercise 3.3: Concepts and components

| Name || Type (3.1) || Concept (3.2) || Component |
| --- | --- | --- | --- |
| *ColorizedBracketPairsDecorationProvider* || *Class* || *Bracket Pair* || *Object* |
| --- | --- | --- | --- |
|   ||   || *Colorized* || *Attribute (of Bracket Pair)* |
|   ||   || *Decoration* || *Attribute (of Bracket Pair)* |
|   ||   ||   ||   |
|   ||   ||   ||   |
|   ||   ||   ||   |

## Finding relationships between concepts

While a flat list of concepts can be very useful to gain insight into the core concepts in a code base, visualization can be a richer way to store knowledge, which is also more in line with how your brain processes information. Rather than flat lists or hierarchies, your brain stores information more like networks, so making these links explicit helps to gain better insights and to remember information better.

### Exercise 3.4: Concept relations and map

Combine the list of concepts with that of behaviour. Which ones belong together?

 
| Concept →Operation ↓ || *program* || *tree* ||           ||           |
| --- | --- | --- | --- | --- |
| *check if valid* || *x* ||   ||   ||   |
| *process* ||   || *x* ||   ||   |
|   ||   ||   ||   ||   |
|   ||   ||   ||   ||   |
|   ||   ||   ||   ||   |

# Expanding knowledge

You have now looked at names occurring in both variables and functions or methods and gathered two domain categories from the names: concepts and behaviour. Before you summarize your findings, let’s focus on these two categories separately. The aim of the next step in reading is to try to define the concepts and behaviours as precisely as possible, based on your prior knowledge of code and domain, and, if possible, on documentation and knowledge of others.

## Gathering more knowledge about concepts

Let’s focus firstly on the concepts present in the code. Start with the list of concepts and decide if you can explain in your own words what the domain concepts mean.

If you can’t, reading more of the code might not be the best next step. Instead, you might want to start by gathering more information about the concepts from other sources by asking teammates or other contributors or reading comments and documentation.

It can be especially valuable to also investigate whether you suspect that synonyms are used in the code. While writing this exercise, I realized that in the Hedy code base we sometimes say *parse tree*, but also use *tree* in other places. While that might be very clear to the code team, using synonyms can be confusing for newcomers. And using synonyms can even impact experienced developers negatively, for example by hampering searchability, since looking for “parse tree” will not reveal locations of “tree” in the code.

### Exercise 3.5 Describe concepts

Now is a good time to reexamine the concepts that you have gathered from the names in Exercise 3.2. Do you understand enough about the domain to describe the meaning of each concept? If not, describe this in the meaning column so you can go back to this list later when you have gathered more information.

| Concept || Meaning |
| --- | --- |
| *Bracket* || *One bracket, can be unexpected if a closing bracket* |
| --- | --- |
| *Bracket Pair* || *Two brackets is code belonging together, should get the same color in the editor* |
| *…* ||   |
|   ||   |
|   ||   |

## Enrich domain understanding

Once you have investigated the code and made an effort to understand the domain concepts from the code itself, it can be very valuable to enrich your knowledge in two ways.

Firstly, you can direct your attention outside of the codebase. Depending on your context, this might be a deep dive into the documentation trying to obtain more information about the concepts that you have found in an open source project, or a session with an experienced colleague within a company. In both cases, you are looking to gather more depth in your understanding of the concepts that you have just learned about. A great exercise is to double check your findings from exercises 3.2 and 3.4. Can you verify that the meaning and synonyms from exercise 3.6a are correct, and that the datatype from exercise 3.6b are correct?

Secondly, after expanding your knowledge of all domain concepts with information outside of the code base, a second way to deepen understanding is to reread the code. Now is a great moment to let go of the small focus on the names in the code, and read the code as a whole again. Because your knowledge is now expanded, you will certainly pick up on new things in the code that did not occur to you before.

# After reading

During the domain examination described in this chapter, you have again gathered a lot of relevant information about the code, this time about its domain concepts and domain behaviour. As with the structural information from chapter 2, you have probably gathered some information you might want to commit back to the code base, either to help future newcomers understand the code better, or to guide your own reading process when diving deeper with exercises later in this book. Here are some ways in which this information can be useful.

## Expand documentation

Some of the information about domain concepts and behaviour can serve as excellent documentation to add to a readme file. In particular, you have explored a lot of important concepts in the code, and these might not live somewhere in a documentation file yet. In a sense, you might have been starting to document the Ubiquitous Language of the project.

# Ubiquitous Language

Eric Evans coined the term *Ubiquitous Language* in his Domain Driven Design book. With the ubiquitous language, Evan denotes a language that developers and users of software systems share. Often this language only partially emerges naturally and parts of it will need to be deliberately designed. Evans states that the Ubiquitous Language should be based on the Domain Model used in the software since natural language concepts tend to be imprecise.

The information that you have gathered so far could be converted into documentation. The lists of concepts that you created in exercises 3.6a and 3.6b can serve as starting points for a list of domain concepts. From there, you can expand, for example by:  

*   Ensuring that all abbreviations or acronyms in variable or function names are either replaced by full words, or if people prefer keeping them, documented
    
*   Describing domain objects that felt similar to you but turned out to be different. If they are not exactly the same but there are reasons that they look similar, such as similar names or similar behaviour, other people might be confused too. Documenting how these concepts differ subtly can be very useful
    
*   Discussing these lists with users. Do they recognize and understand all concepts? What concepts are missing and are they maybe missing or represented in other ways?
    
*   If you work a context where DDD is practiced and a Ubiquitous Language is used, you can check whether the list of your concepts is similar to the documentation of the Ubiquitous Language that already exists.
    

Furthermore, you will likely find some names that do not exactly describe the functionality of code, such as analyze(), convert(), or process(). While reading the code, you might have found a deeper understanding of those words, based on examining the call trace of the function, variable names in the function or comments. You might be able to commit to the documentation, specifically the docstrings of these functions to aid future readers.

## Discuss code quality

While examining the code from the domain perspective, you might find parts of the code that have code smells, specifically linguistic code smells, which are code smells relating to the naming of objects, functions, methods or variables. Your domain findings can serve as a great conversation starter with your team to talk about naming quality.

Similar objects
: 

Your domain analysis might point to objects that seem very similar. Apart from documenting the differences, as explained above, you might also want to dive deeper into the reasons that the code is like it is, for example by discussing:

*   Are these domain concepts truly synonyms or are they different, and if so, how?
    
*   Are there historical reasons for the similarities? Sometimes when a new product or process is introduced, it comes with its own objects since the author might not be aware of places in the code where it could fit.
    

Linguistic code smells
: 

Chapter 2 discussed two different code smells.: structural smells as described by Fowler, and linguistic code smells as defined by Arnaoudova. Since this chapter focuses on domain concepts, it makes sense to evaluate the quality of the names in terms of smells. You can focus on specific types of linguistic smells, which Arnaudova distinguished between in her work, three for functions/methods and three for identifiers.  

*   Methods that do more than they say
    
*   Methods that say more than they do
    
*   Methods that do the opposite of what they say  
    
*   Identifiers whose names say that they contain more than what the entity contains
    
*   Identifiers whose names say that they contain less than what the entity contains
    
*   Identifiers whose names say the opposite of what the entity contains
    

Identifying these code smells is most easily done by starting with the tables you made for exercises 3.1 and 3.3 and comparing the two sources where you can learn about types. Firstly, the types you can gather from the names (such as find\_id, which indicates an id, maybe a string or integer will be returned) versus the names the function definitions define (such as int find\_id()). Of course this will be a lot different in static languages versus in dynamic ones. Exercise 3.6 can help you to compare these two sources.

### Exercise 3.6 Examine types of identifiers and functions/methods

For this exercise, examine your lists of identifiers and function/methods in the codebase that you created in exercises 3.1 and 3.3. For each of the identifiers and function/methods identify the type expected based on the name and the type determined by reading the code or gathering information from other sources. If you have done exercise 2.3 where you describe dependency between methods, that table might also support your understanding of the role and output value of methods.

For now, try to focus on the output values of the code, for example by looking at the locations of the return keyword or the final lines. At this stage of code reading, you are specifically interested in the naming dimensions, not yet in deeper reading, to avoid overloading your working memory.

| Identifier name || Type expected based on name || Type gathered from reading code |
| --- | --- | --- |
| *keywords\_requiring\_indentation* ||   ||   |
| --- | --- | --- |
| *imported\_product* ||   ||   |
| *…* ||   ||   |
| *Function or method name* ||   ||   |
| *process\_input\_string* ||   ||   |
| *check\_program\_size\_is\_valid* ||   ||   |
| *…* ||   ||   |

# Check for linguistic smells automatically

Are you curious about whether your codebase suffers from linguistic antipatterns? Is your codebase written in Java? In that case you can evaluate smells automatically with the Linguistic Anti-Pattern Detector (LAPD) that Arnaoudova created based on her research. LAPD is available as an extension of Eclipse Checkstyle plugin.^([2](#ch02.html#id140))

Name molds
: 

Name molds are patterns in which elements in a variable name are typically combined (see further breakout box) and form are a great dimension for examining code quality. Using many different name molds in a code base makes the code unnecessarily hard to read. If you want to examine the name molds in your code base, you have already done some partial work in exercise 3.2 where you listed the names and their word/domain parts. You can now expand this table in the next exercise to detect different molds.

### Exercise 3.7 Examine name molds

Examine the names of the identifiers in the code base and determine the order in which the elements appear, as illustrated by the examples. I distinguish verbs, nouns, adjectives, and quantifiers here, but you might want to add your own categories where that makes sense, such as acronyms (EBIT) or filters (monthly, per customer).

How many different orders of words can you find? It is also interesting to discuss patterns that arise. Are the orders or words similar locally or globally, i.e. does one method have a lot of one mold while another method uses a different one?

| Variable name || Elements || Order |
| --- | --- | --- |
| *keywords\_requiring\_indentation* || *keyword, requiring, indentation* || *noun, verb, noun* |
| --- | --- | --- |
| *imported\_product* || *imported, product* || *adjective, noun* |
| *minimum\_distance* || *minimum, distance* || *quantifier, noun* |
|   ||   ||   |
|   ||   ||   |
|   ||   ||   |

###### Note

Name molds^([3](#ch02.html#id141)) are patterns in which elements in a variable name are typically combined. For example, when a name was needed for the maximal benefits someone can

receive per month, you could choose max\_benefit, or max\_benefit\_per\_month, or max\_benefit\_num. Each of these contains the same domain concepts, but expressed in different ways. Feitelson ran an experiment with 47 developers which showed that the probability that two developers select the same name is low: the median probability in the study was only 7%! And diverse variable names put more strain on your working memory since you will have to search for the relevant parts in different places each time.

## Refactor

If you want to use the findings of this chapter to improve your code base, there are a number of refactorings that you can consider, partly also based on the quality issues from section 3.4.2.

Similar sounding objects
: 

In some cases, similar sounding objects may warrant a code change. Here are a couple questions to ask:

*   If they are genuinely different, can you rename one to minimize overlap and confusion?
    
*   If they are the same, can they be merged into one object sharing some of the similarities?
    

Vague-sounding names
: 

You will likely find some names that do not exactly describe the functionality of code, such as analyze, convert. What could be a more precise name for vague names like process()?

Smelly names
: 

If you identified names which do not fully match their implementation in exercise 3.9, you might want to rename the method or identifiers to be more precise. You may also want to discuss their implementation with someone more familiar with the code base, to determine why the name does not match the implementation. In some cases a change to the implementation might be better than a change to the name. For example, when you have a method called return name\_and\_id which only returns an id, which is then used in several places to later fetch a name, it might be better to actually return the name and the id and refactor the code to centralize fetching the name.

# Summary

Code can be read in different dimensions: structure, domain, concepts, context, and collaboration. This chapter explored the domain dimension and looked at how variables, functions and objects are named, what you can learn from the names and how names can be used to understand and improve code quality. The following chapter explores programming concepts.

^([1](#ch02.html#id139-marker)) https://martinfowler.com/books/eaa.html

^([2](#ch02.html#id140-marker)) http://www.veneraarnaoudova.ca/linguistic-anti-pattern-detector-lapd/

^([3](#ch02.html#id141-marker)) Dror G. Fietelson et al., [“How Developers Choose Names”](https://www.cs.huji.ac.il/w~feit/papers/Names22TSE.pdf)

# Chapter 3. Conceptual Examination

In the previous chapters, you examined relationships in code, without diving into the content of the code in-depth, and you studied the variable names in isolation without looking at the structure, giving you two different lenses through which to read code.

This chapter will bring both techniques together as you examine the programming concepts present in the code. Concepts often manifest in two ways; they are either clearly written into the names, for example in a name like customerArray, or they can be determined from the structure of the code, for example in a code snippet like this:

```
foreach (customer in customers)
{
    customer.free_account();
}
```

You can also find information about programming concepts by looking at types and imports, or by looking at specific syntactic elements that are connected to concepts, like \* for pointers and . for objects.

Deeply understanding the programming concepts used in a piece of code is useful because it will help you mentally process the code better, and will help you to understand what prior knowledge you might be missing.

The exercises and techniques in this chapter focus on the conceptual aspects of code, helping you answer questions like: What are the important programming concepts in this code base, and how do they relate to the domain and to the structure of the code?

# Running example

As in previous chapters, I provide one example code snippet to help you work through the concepts presented in this chapter: for this chapter, you are learning how to uncover concepts embedded in the code. Don’t worry if it looks entirely overwhelming! By the end of the chapter, you will be able to make sense of it…

## Reading the example snippet

Later sections of the chapter include exercises to complete with the example snippet, but you might want to get a sense of the code before reading on. A great way to start is to use the exercises from previous chapters, to understand the structure and domain of the code.

## Example code: Tree templates in Lark (Python)

```
from typing import Union, Optional, Mapping

from lark import Tree, Transformer
from lark.exceptions import MissingVariableError

Branch = Union[Tree[str], str]
TreeOrCode = Union[Tree[str], str]
MatchResult = Dict[str, Tree]
_TEMPLATE_MARKER = '$'


class TemplateConf:
   #Template Configuration Allows customization for different uses of Template

   def __init__(self, parse=None):
       self._parse = parse

   def test_var(self, var: Union[Tree, str]) -> Optional[str]:
       """Given a tree node, if it is a template variable return its name. Otherwise, return None.

       This method may be overridden for customization

       Parameters:
           var: Tree | str - The tree node to test
       """
       if isinstance(var, str) and var.startswith('$'):
           return var.lstrip('$')

       if isinstance(var, Tree) and var.data == 'var' and len(var.children) > 0:
           first_child = var.children[0]
           if isinstance(first_child, str) and first_child.startswith('$'):
               return first_child.lstrip('$')

       return None

   def _get_tree(self, template: TreeOrCode):
       if isinstance(template, str):
           assert self._parse
           template = self._parse(template)

       assert isinstance(template, Tree)
       return template

   def __call__(self, template):
       return Template(template, conf=self)

   def _match_tree_template(self, template, tree):
       template_var = self.test_var(template)
       if template_var:
           return {template_var: tree}

       if isinstance(template, str):
           if template == tree:
               return {}
           return None

       assert isinstance(template, Tree), template

       if template.data == tree.data and len(template.children) == len(tree.children):
           res = {}
           for t1, t2 in zip(template.children, tree.children):
               matches = self._match_tree_template(t1, t2)
               if matches is None:
                   return None

               res.update(matches)
           return res


class Template:
   """Represents a tree templates, tied to a specific configuration

   A tree template is a tree that contains nodes that are template variables.
   Those variables will match any tree.
   (future versions may support annotations on the variables, to allow more complex templates)
   """

   def __init__(self, tree: Tree, conf = TemplateConf()):
       self.conf = conf
       self.tree = conf._get_tree(tree)

   def match(self, tree: TreeOrCode):
       """Match a tree template to a tree.

       A tree template without variables will only match ``tree`` if it is equal to the template.

       Parameters:
           tree (Tree): The tree to match to the template

       Returns:
           Optional[Dict[str, Tree]]: If match is found, returns a dictionary mapping
               template variable names to their matching tree nodes.
               If no match was found, returns None.
       """
       tree = self.conf._get_tree(tree)
       return self.conf._match_tree_template(self.tree, tree)

   def search(self, tree: TreeOrCode):
       """Search for all occurances of the tree template inside ``tree``.
       """
       tree = self.conf._get_tree(tree)
       for subtree in tree.iter_subtrees():
           res = self.match(subtree)
           if res:
               yield subtree, res

   def apply_vars(self, vars: Mapping[str, Tree]):
       """Apply vars to the template tree
       """
       return _ReplaceVars(self.conf, vars).transform(self.tree)


def translate(t1: Template, t2: Template, tree: TreeOrCode):
   """Search tree and translate each occurrence of t1 into t2.
   """
   tree = t1.conf._get_tree(tree)      # ensure it's a tree, parse if necessary and possible
   for subtree, vars in t1.search(tree):
       res = t2.apply_vars(vars)
       subtree.set(res.data, res.children)
   return tree


class TemplateTranslator:
   """Utility class for translating a collection of patterns
   """

   def __init__(self, translations: Mapping[TreeOrCode, TreeOrCode]):
       assert all( isinstance(k, Template) and isinstance(v, Template) for k, v in translations.items() )
       self.translations = translations

   def translate(self, tree: Tree):
       for k, v in self.translations.items():
           tree = translate(k, v, tree)
       return tree
```

## Context

The code snippet above is code from a Python parser framework called Lark. If you want to examine the code in the browser or in the IDE, you can find it here: [https://github.com/lark-parser/lark/blob/953171821ed307f700fddf27f3fcb9483346bd46/lark/tree\_templates.py](https://github.com/lark-parser/lark/blob/953171821ed307f700fddf27f3fcb9483346bd46/lark/tree_templates.py).

If you have done a domain exercise, you have probably noticed that the domain of this code is mainly a programming domain: parsing and templating.

# Learning from names and operations

When a code base is well organized, names can give you a solid understanding of what code is doing. A name like password**Hashed**  indicates clearly that a hash is being performed on a password. Often though, recognizing concepts from names requires an understanding of the domain. For example, consider the first few lines of the Lark code snippet. Let’s look at the variable Branch at the top of this code snippet:

```
from typing import Union, Optional, Mapping
from lark import Tree, Transformer
from lark.exceptions import MissingVariableError
Branch = Union[Tree[str], str]
TreeOrCode = Union[Tree[str], str]
MatchResult = Dict[str, Tree]
_TEMPLATE_MARKER = '$'
```

The name Branch in itself can refer to several concepts known in programming, and for many developers the first *branch* they will likely think of is a branch in a version control system like GitHub. But *branch* can also be a domain concept, indicating a certain instance of a shop (the Detroit branch) If you know something about parsing and are somewhat familiar with the Lark library, you may be able to deduce that the branch that is meant here is the name of subtree of a parse tree. Now try out [Exercise 4.1](#ch03.html#exercise_4_1_examine_concepts_in_all_variable_name).

## Exercise 4.1 Examine concepts in all variable names in the codebase

For this exercise, write down the variables found in the Lark code snippet. Which programming concepts come to mind based on the names?

| Variable name || Programming concept(s) |
| --- | --- |
| *Branch* || *Trees, Parsing* |
| --- | --- |
| *TreeOrCode* || *Trees, Parsing* |
|   ||   |
|   ||   |
|   ||   |

## Learning from behavior

Variable names can be a source of conceptual information. But, names are not the only place in the code where you can learn about concepts that are present in a piece of code. You can also identify concepts from the operations performed with or on variables. For example, if you see a line of code such as settings.next, you can deduce that settings is a type over which you can iterate, giving rise to the hypothesis that setting will be an iterator or a stream.

And behaviour that you can learn from is more expansive than just naming. Syntax can also be a rich source of understanding. Many syntactic elements are connected to concepts, for example, a variable that is used with a dot notation, like var.children indicates that var is something like and an object or class with fields that you can access. children\[0\] means that children is a list or array in most programming languages. If you are familiar with the programming language, of course these things are not hard to recognize. But it has been my experience reading code myself and reading code with others in code reading workshops that in a setting where cognitive load is really high, like reading an unknown code base, even in a known language, you have to pay deliberate attention to these hints or they are easily missed.

Furthermore, these syntactic concepts are usually very programming language dependent, so reading them with full attention in a code base in a language you are less familiar with is even harder . For example, in C++, if you see a type with an asterisk, like mobj\_t\* thing and field acces with thing->flags it is sensible to conclude that the variable thing is a pointer. But this requires knowledge of C like languages, and if you are familiar with . style access, retrieving the memory that -> also indicates field access can be harder than you think, even if you know it well! (see the next sidebar).

This again underlines how important it is to have a solid understanding of a programming language’s syntax when you read code. One symbol like an asterisk is easily missed but radically changes your view on the type of the variable and thus on the concepts in the code base.

## Exercise 4.2 Examine concepts in all variable names in the codebase

For this exercise, write down the operations that are applied to the variables in the codebase. Which programming concepts can you find in the operations?

| Variable name || Operations connected to the name || Programming concept(s) |
| --- | --- | --- |
| *.update()* || *res.update(matches)* || *dictionary* |
| --- | --- | --- |
| *\[0\]* || *return \_get\_template\_name(var.children\[0\])* || *list* |
|   ||   ||   |
|   ||   ||   |
|   ||   ||   |
|   ||   ||   |

# Using roles of variables to gain a deeper understanding

Variable names can give you a great impression of the concepts in the code base, but there is even more you can learn about concepts from variables alone. A great way to gain a deeper understanding of what concepts a piece of code uses is examining it using the **roles of variables** framework.

Roles of variables, as described by Jorma Sajaniemi at the University of Eastern Finland, is a framework that gives precise names to the different ways that variables are used in code.

For example a variable i in a loop like for i in range(classes): is not just a variable, or even an integer, it has a more specific role and that is of traversing the elements of the range one by one. Even without seeing the body of the loop, you can say with some certainty that the i will be used to index the list classes. If you see another integer variable called max\_class\_size, it will have different connotations for most programmers, even not having seen anything else. Such a variable is not likely to be used to index a list but will likely hold a value representing the highest number of students in a class.

While most programmers will know and understand these variables do different things, roles of variables gives us a precise vocabulary to consider the differences. For example, i in the above example is a **stepper**, a variable stepping through a list of values that is fixed, while max\_class\_size is a **most-wanted holder**, a variable that stores a value that has been searched for.

Sajaniemi^([1](#ch03.html#id144)) describes 11 different roles that cover most of the programs that you will encounter day to day. His research has even shown that the 11 roles cover 99% of programs that novices will write. Table 4.X describes the different roles that variables can play in a program.

| Role || Description || Example || How to recognize this role |
| --- | --- | --- | --- |
| Fixed value || A variable whose value does not change after initialization plays the role of a fixed value. || Examples of fixed-value variables include mathematical constants like pi, or data read from a file or database.  
 || In some languages constants can be expressed (and an error will be thrown if the value is changed) in other languages the naming convention might give a hint a constant value is meant, such as uppercase in Python. But a constant might also simply be a variable that is initialized once and then only assigned to. |
| --- | --- | --- | --- |
| Stepper || When iterating in a loop, there is always a variable stepping through a list of values. That is the role of the stepper, whose value can be predicted as soon as the interaction starts. || A variable i that iterates over a for loop is the canonical stepper, but more complicated steppers are also possible, such as an inner loop variable j start starts at i each iteration. || Is often an integer i  
or j or m or n that is used in conjunction with a loop. |
| Flag || A variable used to indicate that something has happened or is the case. || Typical examples are is\_set, is\_available, or is\_error. Flags are often Booleans, but they can be integers or even strings. || Flags are often recognizable from the variable name that indicates a state, like is\_\_\_ or has\_\_\_\_. Flags are also often used in conditions, either directly (if is\_set) or with an equality check (if value == “set” or 1) in the case of a non-Boolean flag. |
| Walker || A walker also traverses a data structure, similar to a stepper. The difference lies in the way the data structure is traversed. A stepper always iterates over a list of values that are known beforehand. A walker, on the other hand, is a variable that traverses a data structure in a way that is unknown before the loop starts. || Depending on the programming language, walkers can be pointers or integer indices. Walkers can traverse lists, for example in a search, but more often traverse data structures like a stack or a tree.  
 || Walker can be used in loops when we do not know the precise set of values that they will iterate over, such as in a search, or can be connected with data structures like a linked list, where it is used to find the position where a new element should be added. |
| Most recent holder || A variable that holds the most-recent value encountered in going through a series of values is a most recent holder. || For example, it might store the latest line read from a file, i.e. line = file.readline(), or a copy of the array element last referenced by a stepper element = list\[i\]. || Most recent holders can only occur within a loop body, and they will be updated in each iteration of the loop. |
| Most wanted holder || Often when you are iterating over a list of values, you are doing that to search for a certain value. The variable that holds the result of this search, or the best value found so far, is what we call a most wanted holder.  
 || Examples of a most wanted holder are a variable that stores a minimum value, a maximum value, or the first value meeting a certain condition. || Like most recent holders, most wanted holders can also only occur within a loop body, but they will only be updated under certain conditions, when a value is found matching the search better (i.e. a value higher than the currently stored maximum) |
| Gatherer || A gatherer is a variable that collects data and aggregates it into one value. || This can be a variable that starts at zero and collects values while iterating through a loop, like this:  
sum = 0  
for i in range(list):  
sum += list\[i\]  
  
Its value can, however, also be calculated directly in functional languages or languages that encompass certain functional aspects, i.e. functional\_total = sum(list). || Gatherers look quite differently based on the programming language in question. In imperative pieces of code, they can only occur in conjunction with a loop, while in functional languages they can be expressed with pattern matching also. |
| Container || A container is any data structure that holds multiple elements that can be added and removed. || Examples of containers are lists, arrays, stacks, and trees. || In many cases we can recognize containers syntactically, since they are initialized with special symbols, like customers = \[\] or symbols = {} |
| Follower || Some algorithms require you to keep track of a previous or subsequent value. A variable in this role is called a follower and is always coupled to another variable. || Examples of follower variables are a pointer that points to a previous element in a linked list when traversing the list, or the lower index in a binary search. ||   |
| Organizer || Often data has to be processed in some way. For example, in some languages, you cannot access individual characters in a string without converting the string to a character array first. Or you retrieved a collection of elements but you also want to store a sorted version of that list to filter in various ways later on, and sorting once is more effective than sorting each time. These are examples of organizers, which are variables that are only used for rearranging or storing values differently. Often, organizers are temporary variables. || Examples are sorting or filtering variables, for example in pandas:  
  
filtered **\=** df**\[**df**\[**“variable”**\]** **\==** “A”**\]**  
 || Organizers are always connected to other variables whose values they reshape or transform. |
| Temporary || Temporary variables are variables that are used only briefly and are often called temp or t. These variables may be used to swap data or to store the result of a computation that is used multiple times in a method or function. || temp, foo, bar, thing || Temporary variables are often recognized based on their names that might have temp at the beginning or the end, but this is not always the case. Another way to recognize temporary variables is to notice that they are only used in a few lines of code that are located closely together. |

## Exercise 4.3 Examine roles of all variable names in the codebase

For this exercise, examine your list of variables in the codebase again and try to determine their roles. Once you have found all roles, you can fill out the final column of the table documenting the programming concepts related to the variables?

| Variable name || Role || Related variables || Programming concept(s) |
| --- | --- | --- | --- |
| *template in  
  
template = self.\_parse(template)* || *organizers* || *template* || *parsing* |
| --- | --- | --- | --- |
| *res in:*  
  
*res.update(matches)*  
 || *gatherer* || *matches* || *Tree templates* |
|   ||   ||   ||   |
|   ||   ||   ||   |
|   ||   ||   ||   |
|   ||   ||   ||   |

# Understanding concepts using dependencies on frameworks and libraries

To fully understand the concepts that play a role in a piece of code, it is also important to pay attention to what concepts are imported from frameworks and libraries. For example, consider this import statements from the Lark code snippet in Python you examined before:

```
from typing import Union, Optional, Mapping, Dict, Tuple, Iterator
from lark import Tree, Transformer
from lark.exceptions import MissingVariableError
```

The first line of code immediately provides some insights into the programming concepts that are being used: container types such as Tuple and Mapping, and (functional) data structures like Union and Optional. The import line of code also tells you, if you have some prior knowledge of Python and this library, that you are importing typed versions of build-in types like Dictionary and Tuple so Python can give type hints.

The second line tells you that Trees are being used and transformed, and the final line tells you that from all exceptions that Lark supports, only the MissingVariableError is imported. So if you want to dive deeper into reading this specific code base, it would be really interesting to find out what other exceptions exist and why only this one is needed here.

It can also be very helpful to examine where in the code snippet the imports are used. For example in the lark code snippet, the imported Tree Transformer is only used in the \_ReplaceVars class. This helps you to see that that class is the only transformer in this file, and thus plays a large role in the code snippet.

## Exercise 4.4 Examine dependencies

For this exercise, examine the dependencies and (optionally) also read these dependencies. What can you learn about the code from the imported concepts? What places in the code use these concepts?

| Imported concept || Imported from || Programming concept(s) || Used in |
| --- | --- | --- | --- |
| *Transformer* || *lark* || *(parse)tree transformation* || *\_ReplaceVars class* |
| --- | --- | --- | --- |
|   ||   ||   ||   |
|   ||   ||   ||   |
|   ||   ||   ||   |
|   ||   ||   ||   |
|   ||   ||   ||   |

# After reading

Like in other chapters, there are a number of things you can do after examining the programming concepts in the code.

## Expand conceptual documentation

Some of the information about programming concepts and behaviour can serve as excellent documentation to add to a readme file. While the core maintainers of the project will probably know most of the programming concepts intimately, explaining basics in the project documentation can be a very valuable addition for beginners in a project.

## Document roles of variables

Another contribution you could make after examining the roles of variables in the code base is to add that information to the code base. The table you made in Exercises 4.3 can be a great start for that. Depending on the style of the code base, roles of variables could be encoded into variables names, added as comments or documented outside of the code base. As a concrete first step, clearly marking all temporary variables (f.e. With a temp\_ prefix) can make it a lot easier ot perform refactorings in the future.

## Expand dependency documentation

Exercise 4.4 has helped us to understand the dependencies in the code base …more to come.

# Summary

Code can be read in different dimensions: structure, domain, concepts, context, and collaboration. Like chapter 4, this chapter looked at how variables, functions and objects are named, but from the perspective of programming concepts. In addition to names, operations on code, both as user defined functions like .parse(0 and in syntax, such as \[0\] can also help us understand the programming concepts present in a code base.

The next chapter will explore how to understand the context of a code snippet in more depth, including comments and documentation.

^([1](#ch03.html#id144-marker)) https://www.ppig.org/files/2005-PPIG-17th-sajaniemi.pdf

# Chapter 4. Context Examination

Chapters 5-X dive into five different dimensions of code reading: structure, domain, concepts, context, and collaboration.

In the previous chapters, you examined code from three different perspectives: structure, domain and programming concepts. Hopefully by now you will feel more confident when reading unfamiliar code. However, in practice you will probably won’t read small code snippets only. Snippets are part of a code base and as such have a lot of *context*, which is what we will examine in more depth in this chapter.

# Running example

As in previous chapters, we will use one example code snippet in this chapter. In this chapter, you are learning how to read documents that surround the actual code itself.

Again don’t worry if the code is overwhelming, we will go over it slowly and also even when reading it in depth, some parts might remain unclear to you and that is ok, because it represents that way that you will also read code in daily practice. Even in a code base you work in daily, there will be parts that feel less familiar to you because others wrote the code or because you wrote it a long time ago. Therefore it is totally ok to read parts of code you do not understand entirely and practicing it in this book is a great way to experience the feeling of not getting it all.

So don’t worry if it looks entirely overwhelming! By the end of the chapter, you will be able to make sense of it…

## Reading the example code

This chapter will contain specific exercises to do with the example snippet, but of course, you might want to get a sense of the code before reading on. A great way to do that is to now use the exercises from previous chapters, to understand the structure, domain and programming concepts present in the code.

## Example code: Warnings in CoronaWarnApp (Kotlin)

```
class TraceLocationWarnTanViewModel @AssistedInject constructor(
   @Assisted private val traceLocationWarnDuration: TraceLocationWarnDuration,
   private val organizerSubmissionRepository: OrganizerSubmissionRepository,
   dispatcherProvider: DispatcherProvider,
) : CWAViewModel() {
   private val currentTan = MutableStateFlow(Tan(""))
   val registrationError = SingleLiveEvent<OrganizerSubmissionException>()
   val registrationState = MutableLiveData(ApiRequestState.IDLE)

   val state = currentTan.map { currentTan ->
       UIState(
           isTanValid = currentTan.isTanValid,
           isTanValidFormat = currentTan.isTanValidFormat,
           areCharactersCorrect = currentTan.areCharactersValid,
           isCorrectLength = currentTan.isCorrectLength
       )
   }.asLiveData(context = dispatcherProvider.Default)

   fun onTanChanged(tan: String) {
       currentTan.value = Tan(tan)
   }

   fun startTanSubmission() {
       val teletan = currentTan.value
       if (!teletan.isTanValid) {
           Timber.w("Tried to set invalid teletan: %s", teletan)
           return
       }

       Timber.d("traceLocationWarnDuration=%s", traceLocationWarnDuration)
       val payload = OrganizerSubmissionPayload(
           traceLocation = traceLocationWarnDuration.traceLocation,
           startDate = traceLocationWarnDuration.startDate,
           endDate = traceLocationWarnDuration.endDate,
           tan = teletan.value
       )

       launch {
           try {
               registrationState.postValue(ApiRequestState.STARTED)
               organizerSubmissionRepository.submit(payload)
               registrationState.postValue(ApiRequestState.SUCCESS)
           } catch (err: OrganizerSubmissionException) {
               Timber.d(err, "Organizer submission failed")
               registrationState.postValue(ApiRequestState.FAILED)
               registrationError.postValue(err)
           } catch (err: Exception) {
               Timber.d(err, "Unknown organizer submission Exception")
               registrationState.postValue(ApiRequestState.FAILED)
               err.report(ExceptionCategory.INTERNAL)
           }
       }
   }

   
data class UIState(
       val isTanValid: Boolean = false,
       val areCharactersCorrect: Boolean = false,
       val isTanValidFormat: Boolean = false,
       val isCorrectLength: Boolean = false
   )

   @AssistedFactory
   interface Factory : CWAViewModelFactory<TraceLocationWarnTanViewModel> {
       fun create(
           traceLocationWarnDuration: TraceLocationWarnDuration
       ): TraceLocationWarnTanViewModel
   }
}
```

## Context

The code snippet above stems from an app the German government created to detect potential covid infections. If you want to examine the code in the browser or in the IDE, you can find it here: [https://github.com/corona-warn-app/cwa-app-android/blob/127889b390ef00228a619afb7573c5da724a1a6e/Corona-Warn-App/src/main/java/de/rki/coronawarnapp/ui/presencetracing/organizer/warn/tan/TraceLocationWarnTanViewModel.kt](https://github.com/corona-warn-app/cwa-app-android/blob/127889b390ef00228a619afb7573c5da724a1a6e/Corona-Warn-App/src/main/java/de/rki/coronawarnapp/ui/presencetracing/organizer/warn/tan/TraceLocationWarnTanViewModel.kt)

# Gathering information from the code

In previous chapters, I presented you with code snippets that were, given enough time and energy understandable from the snippet alone. In this example however, assuming you are not a domain expert in the topic, it is extremely hard to understand this code base. Not that the code is so complex, but there are domain concepts present that you will not know about: tan and teletan.

For code snippets such as these, inevitably you will have to direct your search outside of the code base, looking in documentation or talking to experts. However, just browsing docs, or asking an expert “can you tell me more about this code?” is not very likely to give you results quickly. Therefore, you first want to gather information from the code base, so that you can start your search with clear questions.

Section X of this chapter explores the surroundings of this code base, using the repository and Google to find more information about these unfamiliar domain concepts. Before doing that, however, you will learn what you can gain from the code snippet to better prepare your search.

## Reading plain text strings

Plain text strings in a code base can sometimes reveal information not yet uncovered by sources you have already seen, like structure of the code, variable, function and class names. Whether it concerns strings used in the UI of an application or debugging information, strings can deepen your understanding but have not been explored in a separate strategy in this book so far. Time to change that!

### Exercise 5.1 Examine string texts related to tan

For this exercise, write down a list of string texts found in the CovidWarnApp code snippet and include notes about what unfamiliar concepts these texts address.

| Text || Information gathered |
| --- | --- |
| *Organizer submission failed* || *A tan has an organizer that might fail* |
| --- | --- |
|   ||   |
|   ||   |
|   ||   |
|   ||   |

## Reading code comments

Sadly this code snippet lacks comments entirely, but in other cases, code comments can also be useful in gathering more questions about domain concepts.

For example, consider this code snippet from the same code base:

```
syntax = "proto3";
package de.rki.coronawarnapp.server.protocols;
// ExposureKeyPackage v0.1
message SignedPayload {
   bytes payload = 1; // serialized Payload message
   bytes certificateChain = 2; // X509 certificate chain
   bytes signature = 3; // signature of payload
}
```

While “serialized Payload message” and “signature of payload” do not necessarily give additional information not present in the code, the X509 comment gives a hint on how to continue your search outside of the code base. You might now be wondering what X509 is, if you are not familiar with it already.

# Examining code snippet surroundings

Now that you have found a bit of ground in what you are examining, you can try to find information outside of the codebase. Before you do so it is smart to create a search plan, for the questions you want to answer. I have spent many an afternoon randomly browsing pages not really keeping track of whether I was making progress at all. While that might be useful if you want to get some grounding in a larger new domain or programming language, if you simply want to understand some code with a clear purpose (use the app, fix a bug) you will probably not be so interested in everything.

## Create a clear search plan

The first thing you will want to do is create a clear search plan, consisting of a list of things you want to know while searching for more information. If you have browsed the code snippet above, with or without using the exercises of previous chapters, these are the things you will probably still be wondering about:  

*   What is a tan?
    
*   What is a teletan?
    
*   What is the role of an organizer?
    
*   How do you know whether a tan is valid?
    
*   When does a tan change?
    

I have often browsed code bases without a clear search plan, and it is very easy to waste a lot of time thinking “hmmm this is interesting” and “awww what would that mean?” without a lot of progress to show for in the end. Try to keep track of your plan and assess after each search whether you are making meaningful progress, even when that can be hard to determine! The examples in the remainder of the chapter focus on the meanings of tan and teletan as questions for the search plan but feel free to go wider if you want.

## Use a generic search engine

Google, of course, can be a good first step in finding information but sadly, depending on your search history of course, tan and teletan do not really give many results:

![](assets/context_examination_487104_01.png)

The same is true for organizer which is a very generic word with several meanings (from an office supply, to a person planning a wedding). Depending on the domain, generic searching might not result in too many useful leads.

Furthermore, it is difficult to even identify the domain concept embedded in a language you’re unfamiliar with.. Had I not told you that tan is a domain concept, it is hard to distill from a name like TraceLocationWarnTanViewModel what parts here are domain concepts! Is it tracing a location? Is a “warn tan” a form of tan, contrary maybe to an error tan? Is the tan warning or is it being warned? From code alone, this will be close to impossible to find out, so you will need to widen your scope beyond the code.

## Use a code search engine

In addition to generic search engines, many coding platforms also allow search within a repository, as shown in Figure 5.2, or findable online via: [https://github.com/corona-warn-app/cwa-app-android/search?q=teletan](https://github.com/corona-warn-app/cwa-app-android/search?q=teletan)

![](assets/context_examination_487104_02.png)

When searching for a term, you can see on the left hand side of the search results that multiple types of results are found. Let’s go over the types of results and what you can learn from them step by step. I highly encourage you to go through the results on your own, if possible!

Code
: 

The first category of results is code. In some cases, you can dive into the code itself and use the techniques from earlier chapters to learn more about concepts from them. However, the goal of this chapter is to widen your search outside of the code base. The nice thing about code search in GitHub is that if you click code on the left-hand side, you will be shown only code results and you can quickly browse all instances of teletan to help gain a notion of the operations used together with it. This will allow you to examine, like in exercise 3.6, what the different operations are that are being applied to the teletan. You can, for example, see that a teletan can be valid or invalid, can be censored and, from a test, you can learn that a teletan allows you to “get registration token”.

Commits
: 

A GitHub search provides robust sources beyond just the code, which you can also search for in the IDE. Clicking commit on the left hand side will show you all commits that match the search term.

This again allows you to expand your knowledge about what this concept entails; you can see that teletans should not be visible in error reports (0ff23e7), that there is such a thing as a “raw” teletan (1628fe), and that it was extended from 7 to 10 characters (b31fd1d) and that the input can be validated (4dc73f1). In this way, without even cloning or forking the project you can easily gain some insight into what happens in the code base.

![](assets/context_examination_487104_03.png)

Issues
: 

Similarly, clicking issues reveals all issues, which often are connected to commits but not always. For example, issue 77 ([https://github.com/corona-warn-app/cwa-app-android/issues/77](https://github.com/corona-warn-app/cwa-app-android/issues/77)) refers to teletan with a clear definition: *“The submission of a positive COVID-19 result is a* security*/privacy sensitive task and involves scanning a QR-code or entering the* *teleTAN. The data involved by this should be* *protected.“* When I was reading the code base myself, the description in this issue was the most helpful in understanding teletan, and contained information I did not find in the code or in the comments or documentation!

# Document your findings!

This is a great time to revisit your search plan, have you answered the relevant questions? It is also very valuable to write down what information you have found where, especially when you are not doing all the searching in one go but plan to revisit your work later. Writing down what information was helping to answer what question can greatly ease later searching.

## Examining Project Docs

In addition to code, the search in Figure 5.2 shows a markdown and an svg file. While you might not be looking for this information under “code” search, there are often documentation files shown here too.  It can be interesting to look at Markdown files shown in code search because these will often contain project documentation, as is true in this case.

![](assets/context_examination_487104_04.png)

There is also an svg file (an image) showing an architecture diagram with more information.

![](assets/context_examination_487104_05.png)

While the diagram doesn’t necessarily help to understand the precise meaning of teletan, it does show the project as a whole in a bit more depth.

Now that you have found a lot of relevant information surrounding the code, it is time to consolidate the information. Research shows that actively trying to remember information is the best way to retain that information in your long term memory (see note below)

# Elaboration

In Chapter 4 I noted the difference between storage strength and retrieval strength. While it is hard to improve storage strength, retrieval strength can be easily increased by trying to remember information. Simply copying information is by far not as powerful as trying to remember it before you look it up!

### Exercise 5.2 Summarize concepts

Write down the concepts that were unfamiliar to you at the start of your examination of the code snippet. Based on the information gathered within strings and within the search of non-code documents in the repository, how would you define the relevant concepts in this code base?

| Concept || In your own words |
| --- | --- |
| *tan* ||   |
| --- | --- |
| *teletan* ||   |
| *organizer* ||   |
| *registration* ||   |
| *…* ||   |

# Enrich understanding

In previous chapters, you gathered lists of domain and programming concepts from the code, and in this chapter, you explored glossaries or definitions lists from code, documentation and issues and pull requests. Especially when gathering information from various sources such as code and documentation and comments and issues, it can be hard to keep track of what you read where. This will be important later if you want to dive deeper into a certain source. So if you have not done so yet, write down some notes about what documents helped you learn what concepts.

## Create a concept map

In Chapter 3 you created a concept map based on what you gathered from the code. As I explained then, your brain also stores concepts more like a map than like a list. Therefore it can be very useful to again create a concept map of the concepts and their relationships. Enrich the map with the sources of information so it will help you to revisit or refine information when needed.

# After reading

In this chapter you have learned techniques to more deeply understand the context of a code base. If you have applied the exercises and techniques of this chapter to a code base you are working with, you can again share the information you have learned within the code base itself. You might be surprised how little connection there is between the documentation and the code in many projects. I can tell you that while writing this book, the documentation in my own open-source repository has improved significantly after following my own advice!

## Improve the glossary with a beginner’s eye

While I was examining the SWA Corona Warn App, a domain I was not knowledgeable in at all, it took me quite a while to deeply understand the concepts and make the relevant connections into the code. If you are reading another code base in a similar way, using similar techniques, I can guarantee you that it would be very useful to the project maintainers if you would share what you have learned in the documentation. When you are an expert, you suffer from what we call the Curse of Expertise. When you know or understand something, it becomes almost inconceivable to you that someone might not know the things you know! So improving project documentation with fresh eyes can be very valuable.

## Discuss documentation quality

An easier way to contribute might be to help maintainers understand the quality of the current documentation. For example, sharing the list of questions that remained after reading the code and initial documentation gives relevant pointers toward where documentation could be improved. Also sharing the sources of relevant information can be very helpful for maintainers. In our example repository I am sure the maintainers would like to know how much I learned from the PR documenting the teletan meaning, and move some of that to the documentation.

## Expand code comments

In other chapters you gathered information from the code, and then added it to the documentation, but in this chapter, you read documentation outside of the code, which you could commit back into the code base. For example, you could take information from an existing glossary or definition list and add it to the code.

In our example snippet, the addition of one line of comment would have made it a lot easier to understand what is happening.

The original code read:

```
val teletan = currentTan.value
       if (!teletan.isTanValid) {
           Timber.w("Tried to set invalid teletan: %s", teletan)
           return
       }
```

But you could expand it like this:

```
val teletan = currentTan.value
// The server returns a TeleTan based on the received TAN value 
       if (!teletan.isTanValid) {
           Timber.w("Tried to set invalid teletan: %s", teletan)
           return
       }
```

# Summary

Code can be read in different dimensions, including looking at the structure, the domain and the programming concepts. This chapter explored the context of a code snippet.

# Chapter 5. Reading for code reviews

Code reviews are a well established practice in the software industry these days. A *code review* is a quality assurance step in the software development lifecycle. A lot of companies put them right before merging code changes back to the trunk, also referred to as the main or master branch, when using a version control like git. It often has a blocking quality when implemented like this, meaning that the code changes cannot be merged back without a code review being performed.

The main activity of a reviewer during a code review is reading. Just as with code reading, nobody is taught how to do a proper code review. You learn it along the way. In this chapter, I will walk you through my approach to a code review and highlight some tips and tricks on finding issues that can tamper with the future readability of code.

# How to do a code review?

Although it is a well established practice, a code review is not a well defined practice in our industry. It can be an individual activity where reviewers give feedback asynchronously. It can also be a pairing activity where the author of the code walks the reviewer(s) through the implementation while discussing and changing the feedback immediately.

Every company I (Gien) have worked for, did their code reviews differently. When there were testers on the team, manual testing was not part of the review the software developer did. When there were no testers, testing was the main reason code reviews existed. Some companies had a standardized way of doing code reviews, but not all of them. When there was no formal structure for the code review, each code review I requested was done differently, depending on the person picking up the request.

It also depended on the *continuous integration (CI)* practices of the company. Some companies had automated quality assurance as part of the CI pipeline, others didn’t. I worked for companies where automated testing was limited or absent, and in those companies the main reason for code reviews was also testing.

For me, the purpose of a code review is twofold: to find improvements in the code and to distribute knowledge in the team. Everything else is out of scope. Here are some things to keep in mind when structuring a code review:

*   A code review is not a control mechanism to make sure developers follow the code conventions.
    
*   It is not a technique to educate “junior” developers on the team.
    
*   Code reviews are a bad way to add testing to your development lifecycle.
    
*   Refactoring the code to implement those improvements does not have to be part of a code review.
    

If you want quality assurance, you shouldn’t implement elaborate code reviews as a blocking step in your CI pipeline. Code reviews itself should be an ongoing process that can be done at any time: before, during or after you make code changes. You should automate code conventions, bug detection and testing. This is sometimes referred to as continuous code quality.

Whether you are fixing a bug, adding a new feature or refactoring code, you should create the merge request (MR) the moment you start to make a code review possible for your team members. You should add enough information to your MR so that your team members can understand what you will be changing and how you will change it.

# Automating parts of the code review

Starting out with a language or figuring out how to use new features in a new version of it, can be a challenge. This is why most programming languages offer recommendations for code conventions. The guidelines are basically a statement of their creators on how to use the language best. For example, Microsoft offers best practices for naming conventions, code layouts, comments, security, and general language guidelines.

For example, we already mentioned in chapter 2 that Python classes are written in snake\_case. That is an example of a naming convention. C# has a different naming conversion for classes, PascalCase, where you use an uppercase letter for every word in your class. “Leave a blank line between two methods” is an example of a code layout convention. You can have a look at all of them here: [https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)

You might wonder why I am talking about code conventions in a chapter about code reviews. Since you have to read code that was written by somebody else during a code review, it is easier if some things are agreed upon beforehand such as naming and the layout of files. Opening a file and finding a completely different way of naming or layouting can be very confusing. You want to focus on reading the code, but instead you have to take the time to get used to the layout of the file. You want to remove as many barriers as possible to increase readability.

But! What are the conventions you should follow in a team? This has been the topic of many heated discussions in some of my previous teams. Every programmer has their own preference. Every programmer thinks their preference is right, and all the other ones are wrong. I too thought like that for a very long time and I must confess I still get lured into this discussion trap sometimes. In general, my advice is to use the guidelines that your language offers you and automate the discussion away.

## Code formatters

A *code formatter* is a tool that formats your code into a specific layout. Most IDE’s have built-in code formatters and shortcuts that can format your code according to a set of specifications in your settings. In more general code editors like Visual Studio Code, you have to install a plug-in for your language.

There are a few ways that you can automate code formatting. You can enable formatting when saving a file, as shown in [Figure 5-6](#ch05.html#fig_6_5_add_a_comment_to_the_line_that_is_incorrect_acco).

![](assets/reading_for_code_reviews_309317_01.png)

Unfortunately, not every IDE offers auto-formatting on save. What you can do then is format on committing changes to your version control. Git offers *Hooks*, a way of automatically executing scripts before or after certain git commands are executed. One of those Hooks is the “pre-commit hook”. You can add anything to this hook, but a common practice is to add a code formatter here.

If you use an automated code formatter, code reviews are no longer a control mechanism for checking code layout conventions. The reviewer will also be able to focus on what is important: finding improvements and understanding the code.

## Linters

*Linters* go a bit further than code formatters. They analyze your code against a set of rules to find bugs, detect code smells and vulnerabilities. They also find style violations for you, but unlike a code formatter, they don’t fix those automatically. When you start looking for a linter, you will find that it gets a bit fuzzy though. Some linters can automatically correct style violations and code smells, others cannot. What they can analyze also differs. SonarQube, for example, can analyze multiple programming languages, configuration files for Docker, even your SQL statements. Pylint on the other hand is specifically developed to analyze Python.

Same as with code formatters, you can install (some) of them in your IDE or use the pre-commit hook to run them.

# Analyzing the dimensions

During a code review, you analyze all four dimensions of code reading for understandability: structure, business domain, programming concepts, and context. Together, they need to form a clear, and cohesive picture.

Depending on my familiarity with the part of the codebase that changed, I like to start my code reviews differently. Let’s go through an illustrative example I created for this book. I picked Marten, a .NET library that allows PostgreSQL to function as a document database or an event store. The open source project can be found here: [https://github.com/JasperFx/marten](https://github.com/JasperFx/marten). I copied over one of their merge requests I will be evaluating: [https://github.com/codereadinginpractice/marten/pull/1](https://github.com/codereadinginpractice/marten/pull/1)

If you want to start contributing to an open source project, but you are uncertain how to do that, doing a code review is a good way to start.

## Getting a feel for the merge request

First, I get a general feel for the MR. I have a look at conversations, commits and quickly check the collapsed view of files changed, as shown in [Figure 5-6](#ch05.html#fig_6_5_add_a_comment_to_the_line_that_is_incorrect_acco).2. I can see here that it is a small MR with mostly added lines of code, because the color blue is dominant in this overview.

![](assets/reading_for_code_reviews_309317_02.png)

If I look at the sidebar, I can gain even more information. Next to each file name, there is a symbol to indicate what happened to the file ([Figure 5-6](#ch05.html#fig_6_5_add_a_comment_to_the_line_that_is_incorrect_acco).3). I can easily see that 4 new files were added, and 3 files were changed. No files were removed in this MR.

![](assets/reading_for_code_reviews_309317_03.png)

You can also gain a lot of information from looking at displayed information in the MR. As shown in [Figure 5-6](#ch05.html#fig_6_5_add_a_comment_to_the_line_that_is_incorrect_acco), you can get an indication on how much has changed between the two files. In this example there are 3 changes in the file docs/.vitepress/config.ts. There are more additions to this file then there are removals. You can see this by looking at the color indications next to the number of changes: blue means lines of code were added to the right file that is not in the left file.. Red indicates lines of code were removed from the left file. Two squares are blue and one is red, so there were more lines of code added to config.ts than there were removed.

![](assets/reading_for_code_reviews_309317_04.png)

Let’s take an even deeper dive. Look for the numbers between two @@ underneath the file name. This gives you information about what is displayed from each file. We can read -173,7 +173, 8 as followed:

*   The minus sign indicates that lines of code that appeared in the left file, are removed from the right file.
    
*   The number 173 is the line number of the first line of code that is displayed in the diff view.
    
*   The 7 means that there are 7 lines of code displayed for the left file.
    
*   The plus sign indicates that lines of code were added to the right file, that are not in the left file.
    
*   The right file starts displaying also at line 173, but it is displaying 8 lines of code.
    

Once you are finished with reviewing the differences, you can mark the file as viewed. I like this feature because reviewing changes can be very exhausting and cannot always be completed in one sitting. This MR is pretty small, only 7 files were changed and one of those is the gitignore file so I can review it in one go. However, I have had review requests that changed over 15 files and that is too much to do at once. I review larger merge requests in smaller chunks and use the reviewed check to keep track of what I already did.

I also try to understand what the purpose of the code changes is. Is this merge request:

*   Fixing a bug?
    
*   Adding a new feature?
    
*   Changing or expanding an existing feature?
    
*   Refactoring existing code?
    
*   …?
    

This might sound normal to you, but I have encountered merge requests where it took some effort to figure out what exactly was happening. Generally, people should be separating these types of code changes and creating a merge request for each of them, but this is not always the case. The MR from the example adds a new feature to the codebase, as shown in [Figure 5-6](#ch05.html#fig_6_5_add_a_comment_to_the_line_that_is_incorrect_acco).

![](assets/reading_for_code_reviews_309317_05.png)

From the description alone you can already find domain concepts that you need to understand in order to do your code review. I already mentioned async daemon. As an exercise, find other domain concepts and add them to the Name column in Exercise 11.1. You don’t have any information yet on Type, Concept or Component so let’s leave those blank for now.

### Exercise 11.1: Concepts

| Name || Type || Concept || Component |
| --- | --- | --- | --- |
| *Async Daemon* ||   ||   ||   |
| --- | --- | --- | --- |
|   ||   ||   ||   |
|   ||   ||   ||   |
|   ||   ||   ||   |
|   ||   ||   ||   |
|   ||   ||   ||   |

## Examining the structure

After a thorough examination of the MR, It is time to dig into the files. You can use what you learned in chapter 2 to understand the structure of the new version of the code.

### Exercise 11.2: List all components

| Object components  |
| --- |
| Component name  || Lines of Code  || Size (small/medium/large)  |
| --- | --- | --- |
| *AsyncDaemonHealthCheckExtensions*  || *97*  || *small*  |
|    ||    ||    |
|    ||    ||    |
|    ||    ||    |
| Functional components  |
| Method/function name  || Lines of Code  || Size (small/medium/large)  |
|    ||    ||    |
|    ||    ||    |
|    ||    ||    |
| Singular components  |
| **Variable name**  ||    ||    |
|    ||    ||    |
|    ||    ||    |

## Understanding the domain concepts

Now that you have a clear understanding of the structure of the MR and the files, it is time to go back to the domain concepts.

Let’s go back to the conversation tab of the MR, where you found the domain concept called async daemon and health check. From the text you can deduct that async daemon was an existing concept, where the functionality of a health check was implemented for. You wrote these concepts down but did nothing with them. Let’s now use the documentation available to find the meaning of those words and record them in Exercise 11.3. This includes the new documentation that was added in this MR. You should also read the comments, if there are any, to see what other domain concepts are there.

### Exercise 11.3: Add all the domain concepts to the list and search for their meaning in the documentation.

| Name || Meaning |
| --- | --- |
| *Async Daemon* || *Asynchronous projection processing engine* |
| --- | --- |
| *Health check* ||   |
|   ||   |
|   ||   |
|   ||   |
|   ||   |

Once you understand the meaning of old and new domain language, you can look at the tests to see how these concepts function. First examine the names of the tests, and give your own formulation as to what you think the test is doing. Take `` ` ```should_add_settings_to_services``` ` `` as a name. You can’t easily deduct what exactly this is testing, even though you know the meaning of most of the domain concepts. Is it: Using the health check for the async daemon extension should add settings to services? Which services should have these settings now?

You also have to analyze if the test does what the name suggests. If you look at the test implementation of ‘`should_be_healthy_with_one_projections_lagging`’ you see that the assert statement does not match the name of the test. The health check should return an unhealthy status according to the code:  `result.Status.ShouldBe(HealthStatus.Unhealthy)`*;* This is something to add as a comment in the MR as shown in [Figure 5-6](#ch05.html#fig_6_5_add_a_comment_to_the_line_that_is_incorrect_acco).

![](assets/reading_for_code_reviews_309317_06.png)

Let’s look at another domain concept. *`HealthCheckResult`* is a domain concept from the library that is being used in the code. I prefer to protect my code from leakages like this by introducing my own domain concept for this and translating from theirs to mine. It is also a very generic name, I could make it something more specific like *‘`AsyncDaemonHealthStatus`’*, which tells me a lot more about the return value of this method than the original name.

I am also noticing an overuse here of Unhealthy. Three things can happen: the Async daemon is healthy, unhealthy or some kind of error occurs whilst trying to determine. In that last case, I have no idea what the health status is of the Async Daemon, yet they state that it is unhealthy. I would try to find a better way to do that.

### Exercise 11.4: Investigate domain concept *AsyncDaemonHealthCheckSettings*.

Perform an analysis of Async… similar to the one I did for the HealthCheckResult. Think about the following questions:

*   Is the code more readable because this domain concept is present?
    
*   Is this hiding another domain concept?
    
*   Is there a better name available for this?
    

## Converting improvements to tasks

If I want to go for non-blocking code reviews, as I mentioned in section 11.1, I can’t just leave my comments and wait until the author of the code agrees with me or not. Because it is non-blocking, it also does not have to be the author that implements these either. Depending on how long it would take to implement my improvements I will either create well defined tasks to pick up later, or implement them immediately. Once I have done that, I am done with my review!

# Summary

When performing a code review, you need to use all four dimensions of code reading. The main purpose of a code review is to find improvements and share knowledge in the team. At the end of the code review, you will have a list of improvements. Depending on how long an improvement takes to implement, you can either implement them after merging back the branch or generate tasks out of them to pick up at a later time.

# About the Authors

**Felienne Hermans** is associate professor at Leiden Institute of Advanced Computer Science at Leiden University, where she heads the PERL research group, focused on programming education. She also works at the Vrije Universiteit Amsterdam, where she teaches prospective computer science teachers.

Felienne is the creator of the Hedy programming language, and was one of the founders of the Joy of Coding conference. Since 2016, she has been a host at SE radio, one of the most popular software engineering podcasts on the web. Felienne is the author of *The Programmer’s Brain* (Manning 2021) which helps programmers understand how their brains work and how to use it more effectively. In 2021, Felienne was awarded the Dutch Prize for ICT research.

Felienne is a member of the board of I&I, the Dutch association of high-school computer science teachers, and of TC39, the committee that designs JavaScript.

**Gien Verschatse** is an experienced consultant and software engineer that specialises in domain modelling and software architecture. She’s fluent in both object-oriented and functional programming, mostly in .NET. As a Domain-Driven Design practitioner, she always looks to bridge the gaps between experts, users, and engineers. As a side interest, she’s researching the science of decision-making strategies, to help teams improve how they make technical and organisational decisions. She shares her knowledge by speaking and teaching at international conferences.

And when she is not doing all that, you’ll find her on the sofa, reading a book and sipping coffee.