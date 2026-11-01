
## Functional Programming in

MEAP

Marco Vermeulen
Rúnar Bjarnason
Paul Chiusano

![](img/255e7d4fd2ecc6516db2405b06be1018_1_0.png)

## MEAP Edition
Manning Early Access Program
Functional Programming with Kotlin
Version 8

Copyright 2020 Manning Publications

For more information on this and other Manning titles go to [manning.com](manning.com)

## welcome

Thanks for joining the early access program of *Functional Programming in Kotlin*. I hope you have lots of fun as you join me in working through this exciting, yet challenging book.

Why is it a challenging book? This book is a port of Manning’s Functional Programming in Scala into Kotlin. The original (known as “the Red Book”) has a huge following and is revered by many as the authoritative text about functional programming (FP) in the Scala community. In fact, the Red Book is less about Scala than what it is about good FP techniques and principles. It teaches functional programming from first principles, regardless of language.

Why is this book exciting? Kotlin is the new kid on the block, and many see it as their new language of choice on the JVM. Kotlin is gaining in popularity among backend developers, and not just with mobile (Android) developers. Kotlin also has a remarkable number of similarities to Scala; like Scala, it falls into the category of a general-purpose object functional language.

As a result of the increasing popularity of both FP and Kotlin, I decided to bring these two worlds together. Not much has been written about FP in Kotlin so far. This is the perfect opportunity to bring the success of Functional Programming in Scala to all the Kotlin developers who would love to learn functional programming.

The book closely follows the text of Functional Programming in Scala, but all code samples have been adapted and translated to idiomatic Kotlin code. At times we do deviate from the original text due to the languages being different, but the book remains as true to the original text as possible.

The first part of the book slowly eases into the topics by introducing some basic FP concepts, then explores some Kotlin features necessary to writing good FP code. We then explore how to work with data structures, perform error handling, use lazy evaluation, and work with pure functional state.

The second part deals with functional design, where we explore purely functional parallelism, property based testing, and building a parser combinator library.

The third part of the book deals with monoids, monads(!), applicatives, and traversable functors.

The final section deals with advanced topics such as external and local effects, dealing with mutable state, stream processing and incremental IO.

With that said, let’s jump right into it! I hope you enjoy this book as much as I am enjoying translating it for you. May it inspire you to write beautiful, pure functional code in every Kotlin project that you work on in the future.

If you have any questions, comments, or suggestions, please share them in Manning’s liveBook's Discussion Forum for my book.

—Marco Vermeulen

## brief contents

## PART 1: INTRODUCTION TO FUNCTIONAL PROGRAMMING

- 1 What is functional programming?
- 2 Getting started with functional programming in Kotlin
- 3 Functional data structures
- 4 Handling error without exceptions
- 5 Strictness and laziness
- 6 Purely functional state

## PART 2: FUNCTIONAL DESIGN AND COMBINATOR LIBRARIES

- 7 Purely functional parallelism
- 8 Property-based testing
- 9 Parser combinators

## PART 3: COMMON STRUCTURES IN FUNCTIONAL DESIGN

- 10 Monoids
- 11 Monads and functors
- 12 Applicative and traversable functors

## PART 4: EFFECTS AND I/O

- 13 External effects and I/O
- 14 Local effects and mutable state
- 15 Stream processing and incremental I/O

## APPENDIXES:

- A Exercise hints and tips
- B Exercise solutions
- C Expansive exercises
- D Higher-kinded types
- E Type classes

## What is functional programming?

![](img/255e7d4fd2ecc6516db2405b06be1018_4_0.png)

## This chapter covers:

- Understanding side effects and the problems they pose
- Achieving a functional solution by removing side effects
- Defining what a pure function is
- Proving referential transparency and purity using the substitution model

Most of us started programming using an *imperative style* of coding. What do we mean by this? It means that we give the computer a set of instructions or commands one after the other. As we do so, we are changing the system’s state with each step that we take. We are naturally drawn to this approach because of its initial simplicity. On the other hand, as programs grow in size and become more complicated, this seeming simplicity will lead to the very opposite; complexity arises and takes the place of what we initially intended to do. The end result is code that is not maintainable, difficult to test, hard to reason about and (possibly worst of all) full of bugs. The initial velocity that we were able to deliver features in slows down substantially until even a simple enhancement to our program becomes a slow and laborious task.

*Functional programming* is an alternative style to the *imperative* that addresses the problems mentioned above. In this chapter we will be looking at a simple example where a piece of imperative code with *side effects* (we’ll understand what that means shortly) is transformed into the functional style by a sequence of refactoring steps. The eradication of these side effects is one of the core concepts behind functional programming, and so is one of the highlights of this chapter. We will understand the dangers these effects pose and see how to extract them from our code, bringing us back to the safe place of simplicity where we departed from when we initially set out on our journey.

At this point, it’s also worth mentioning that this book is about functional programming using *Kotlin by example* to demonstrate the principles of this programming paradigm. Moreover, the focus is not on Kotlin as a language but rather on deriving the concepts used in functional programming. In fact, many of the constructs that we build are not even available in Kotlin but only in third party libraries such as Arrow ¹. This book teaches you functional programming from *first principles* that could be applied to many programming languages, not just to Kotlin.

Something else to keep in mind while reading this book is the *mathematical nature* of the functional programming that we will set out to learn. Many have written about the topic of functional programming, but the kind that we are describing in this book is a bit different. It relies heavily on the *type system* that statically typed languages such as Kotlin provide, and is often called *typed functional programming*. We will also make mention of *category theory*, a branch of mathematics that aligns itself very closely with this style of programming. Due to this mathematical slant, be prepared for words such as *algebra*, *proofs* and *laws*.

Along these lines, this is not a book of recipes or magic incantations. It won’t give you quick fixes or fast pragmatic solutions to your everyday problems as a programmer. Instead, it will teach you and equip you with foundational concepts and theory that you can apply to help you arrive at many pragmatic solutions of your own.

Functional programming (FP) is based on a simple premise with far-reaching implications: we construct our programs using only *pure functions*—in other words, functions that have no *side effects*. What are side effects? A function has a side effect if it does something other than simply return a result, for example:

- Modifying a variable beyond the scope of the block where the change occurs
- Modifying a data structure in place
- Setting a field on an object
- Throwing an exception or halting with an error
- Printing to the console or reading user input
- Reading from or writing to a file
- Drawing on the screen

We provide a more precise definition of side effects later in this chapter, but consider what programming would be like without the ability to do these things, or with significant restrictions on when and how these actions can occur. It may be difficult to imagine. How is it even possible to write useful programs at all? If we can’t reassign variables, how do we write simple programs like loops? What about working with data that changes, or handling errors without throwing exceptions? How can we write programs that must perform IO, like drawing to the screen or reading from a file?

The answer is that functional programming is a restriction on *how* we write programs, but not on *what* programs we can express. Over the course of this book, we’ll learn how to express the core of our programs without side effects, and that includes programs that perform IO, handle errors,
