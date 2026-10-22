
WARNING
This element indicates a warning or caution.

## Using Code Examples

Supplemental material (code examples, exercises, etc.) is available for download at https://github.com/oreillymedia/title_title.

If you have a technical question or a problem using the code examples, please send email to bookquestions@oreilly.com.

This book is here to help you get your job done. In general, if example code is offered with this book, you may use it in your programs and documentation. You do not need to contact us for permission unless you’re reproducing a significant portion of the code. For example, writing a program that uses several chunks of code from this book does not require permission. Selling or distributing examples from O’Reilly books does require permission. Answering a question by citing this book and quoting example code does not require permission. Incorporating a significant amount of example code from this book into your product’s documentation does require permission.

We appreciate, but generally do not require, attribution. An attribution usually includes the title, author, publisher, and

ISBN. For example: “*Programming the Internet of Things* by Andrew King (O'Reilly). Copyright 2021 Andrew King, 978-1-492-08141-8.”

If you feel your use of code examples falls outside fair use or the permission given above, feel free to contact us at permissions@oreilly.com.

## O’Reilly Online Learning

> **NOTE**
> 
> For more than 40 years, *O'Reilly Media* has provided technology and business training, knowledge, and insight to help companies succeed.

Our unique network of experts and innovators share their knowledge and expertise through books, articles, and our online learning platform. O’Reilly’s online learning platform gives you on-demand access to live training courses, in-depth learning paths, interactive coding environments, and a vast collection of text and video from O’Reilly and 200+ other publishers. For more information, visit http://oreilly.com.

## How to Contact Us

Please address comments and questions concerning this book to the publisher:

O'Reilly Media, Inc.

1005 Gravenstein Highway North

Sebastopol, CA 95472

800-998-9938 (in the United States or Canada)

707-829-0515 (international or local)

707-829-0104 (fax)

Email bookquestions@oreilly.com to comment or ask technical questions about this book.

For news and information about our books and courses, visit http://oreilly.com.

Find us on Facebook: http://facebook.com/oreilly

Follow us on Twitter: http://twitter.com/oreillymedia

Watch us on YouTube: http://www.youtube.com/oreillymedia

- 1 The latest UML specification can be found on the Object Management Group’s (OMG) website (https://www.omg.org/spec/UML/).
- 2 For further reading on Gordon Moore and Moore’s Law, see https://www.britannica.com/biography/Gordon-Moore
- 3 https://en.wikipedia.org/wiki/ARPANET
- 4 S. D. Antón, D. Fraunholz, C. Lipps, F. Pohl, M. Zimmermann and H. D. Schotten, “Two decades of SCADA exploitation: A brief history,”
2017 IEEE Conference on Application, Information and Network Security (AINS), Miri, 2017, pp. 98-104, doi: 10.1109/AINS.2017.8270432.
- 5 Details on the IETF’s RFC 768 can be found at https://tools.ietf.org/html/rfc768
- 6 For further reading on Tim Berners-Lee’s WWW proposal, please see https://www.w3.org/History/1989/proposal.html
- 7 See an explanation at https://www.livinginternet.com/i/ia_myths_toast.htm
- 8 See an explanation at https://en.wikipedia.org/wiki/Trojan_Room_coffee_pot
- 9 You can read more about Kevin Ashton’s “Internet of Things” presentation at https://www.rfidjournal.com/that-internet-of-things-thing
- 10 You can read a brief summary of the Arduino’s birth at https://www.computerhistory.org/timeline/2005/
- 11 More information about the Industrial Internet Consortium can be found on their website (https://www.iiconsortium.org/index.htm).

## Part I. Getting Started

## Chapter 1. Setting Up Your Environment

### A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 1st chapter of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the author at aking.em@gmail.com.

> *A path lies ahead,*
> 
> *Brambles and thorns, then it clears.*
> 
> *Almost there. Patience.*

**Fundamental concepts:** Identify a problem area to tackle and define an architecture as the baseline for your IoT solution; Setup an IoT-centric development environment that supports multiple deployment options.

You’ve likely gathered by now that the Internet of Things can be vast, unwieldy, and very difficult to tame. To plan a way forward, we’ll first want to identify a problem area to tackle and then create an architecture from which to design and build our IoT solution.

Let’s start with a few key questions to establish a baseline:
What problem are you trying to solve? Where does it start and end? Why does it require an IoT ecosystem? How will all these pieces work together to solve this problem? What outcome can you expect if everything works as designed? We’ll explore each of these in detail, and along the way construct an end-to-end, integrated IoT solution that meets our needs.

## What you’ll learn in this chapter

To help you really understand how an IoT system can and should be constructed, I’ll dig into some basic architectural concepts based on the questions above, and use this as the basis for each programming activity. From there, you’ll build a solution that addresses the problem layer by layer, adding more functionality as you work through each subsequent chapter.

It goes without saying, of course, that the right development tools will likely save you time and frustration, not to mention help you with testing, validation, and deployment. There are many excellent open source and commercial development tools and frameworks available to support you.

If you’ve been a developer for any length of time, I expect you have your own specific development environment preferences that best suit your programming style and approach. I certainly have mine, and while the examples I present will be based on my preferred set of tools, my goal in this chapter is not to
specify those you must use, but to help you ramp up on IoT development in a way that enables you to move out quickly and eventually choose your own for future development projects.

The concepts I present will be what matter most – the programming languages, tools (and their respective versions), and methods can be changed. These concepts represent some of the fundamentals of consistent software development: system design, coding, and testing. Let’s dig into each and get moving towards building your integrated IoT system.

## Designing Your System

Creating a problem statement is probably the most important part of this puzzle. Let’s try it by drafting something reasonably straight-forward, but sufficient to encompass a variety of interesting IoT challenges:

I want to understand the environment in my home and how it changes over time, and make adjustments to enhance comfort while saving money.

Seems simple enough, but this is a very broad goal. We can narrow it down by defining the key actions and objects in our problem statement. Our goal is to isolate the ‘what’, ‘why’, and ‘how’. Let’s first look at the ‘what’ and the ‘why’, and identify any action(s) that the design should consider as part of this process.

## Breaking Down The Problem

The exercises in this book will focus on building an IoT solution that can help you understand your home environment and respond appropriately. The assumption is that you’ll want to know what’s going on within your house (within reason), and take some sort of action if it’s warranted (that is, if the temperature is too hot, turn on the air conditioning). Simple, right?

This part of your design approach requires three steps:

- 1. Collect Data

Let’s define this in terms of what can be sensed, like temperature, humidity, etc. This is centered on the capture and transmission of *telemetry* (measurement data). The action, or rather, action category, will simply be named *data collection*
  - Temperature
  - Relative Humidity
  - Barometric Pressure
  - System Performance (utilization metrics for CPU, memory, storage)

- 2. Determine Relevant Changes

To decide which data is relevant, and whether or not a change in value is important, we need to not only collect data, but to store, and trend, time-series data on the items we can sense (like temperature, humidity,
