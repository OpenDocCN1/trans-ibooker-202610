
Compare `arc42` to a cabinet with drawers: the drawers are clearly marked with labels indicating the content of each drawer. `arc42` contains 12 such drawers (a few more than you see in the diagram that follows). The meaning of these `arc42` drawers is easy to understand:

![Figure 1.2: arc42 drawers ](image/B15546_01_02.jpg.jpg)

#### Figure 1.2: arc42 drawers

`arc42` offers you a simple and clear structure to document and communicate your (complex!) system. Starting with the goals and requirements of your system and how its embedded into its environment, you can provide the important stakeholders of your system with adequate information about the architecture.

`arc42` is optimized for understandability and adequacy. It guides you to explain any kind of architecture information or decision in an understandable and reproducible context.

Individuals and organizations using `arc42` especially like two things about it:

1. The understandability of the documentation, resulting from its standardized structure (the drawers)
2. The manageable amount of effort needed to create such documentation - we call it "painless documentation"

## Why arc42?

You're kidding, aren't you? Ever heard of Douglas Adams, the (very British) and already deceased sci-fi writer? His novel *Hitchhikers Guide to The Galaxy* calls `arc42` the:

*Answer to the Ultimate Question of Life, The Universe, and Everything* (https://en.wikipedia.org/wiki/Phrases_from_The_Hitchhiker's_Guide_to_the_Galaxy#Answer_to_the_Ultimate_Question_of_Life.2C_the_Universe.2C_and_Everything_.2842.29)

`arc42` aims to provide the answer to *everything* related to your software architecture. (Yes, I know it's a little pretentious, but we couldn't think of a better name back in 2005.)

## Where to Get Additional Information

With the latest release (V7) of `arc42` in January 2017, the project now has extensive online help and FAQs (frequently asked questions) available:

* The documentation site docs.arc42.org ([https://docs.arc42.org](https://docs.arc42.org)) contains the full documentation plus many (more than 100) tips for efficient and effective usage.
* The FAQ site faq.arc42.org ([https://faq.arc42.org](https://faq.arc42.org)) answers more than 100 typical questions.

## I.2 Why This Book?

### Note

Examples are often better suited to show how things can work than lengthy explanations.

`arc42` users have often asked for examples to complement the (quite extensive) conceptual documentation of the template, which was, unfortunately, only available in German for several years.

There were a few approaches to illustrating how `arc42` can be used in real-world applications, but those were (and still are) scattered around numerous sources, and are not carefully curated.

After an incredibly successful (again, German-only) experiment to publish one single example as a (very skinny) 40-page booklet, we decided to publish a collection of examples on a modern publishing platform – so we can quickly react to user feedback and add further samples without any hassle.

## I.3 What This Book is Not

In this book, we focus on examples for `arc42` to document and communicate software architectures. We only give a brief introduction to `arc42` (if you'd like to know more, see *Appendix B* for details).

This book is *not* an introduction to:

* Software architecture
* Architecture and design patterns
* Modeling, especially `UML` (`Unified Modeling Language`)
* Chinese cooking (but you probably didn't expect that here)

## I.4 An Overview of Examples

We have encountered exciting software and system architectures during our professional lives. Several of those would have made instructive, interesting, and highly relevant examples – too bad they were declared confidential by their respective owners. Our nondisclosure agreements stated that we're not allowed to even mention clients' names without violating those contracts.

Therefore, it has been quite difficult to find practical examples that we can freely write and talk about.

Now, let's get an overview of the examples that are waiting in the rest of this book.

### HTML Sanity Checking

The HTML Sanity Checker is a tiny open source system for checking HTML files for semantic errors such as broken cross-references, missing images, unused images, and similar stuff.

Dr. Gernot wrote the initial version of this system himself when working on a rather large (100+ pages) documentation based upon the `AsciiDoctor` ([https://asciidoctor.org](https://asciidoctor.org)) markup language.

`HtmlSanityCheck` (or `HtmlSC` for short) is described in *Chapter 2,* *HTML Sanity Checking*, of this book.

### Mass Market Customer Relationship Management

Gosh, what an awkward name – so, let's shorten it to `MaMa-CRM`: mass market refers to consumers of several kinds of enterprises, such as health insurance, mobile telecommunications, or large real-estate companies.

We are reasonably sure you know these 86 × 54 mm plastic cards (experts call them ISO/IEC 7810 – [https://en.wikipedia.org/wiki/ISO/IEC_7810](https://en.wikipedia.org/wiki/ISO/IEC_7810)). Chances are, you even own several of these cards. If they contain your photo, the chances are high that they were produced by a system such as `MaMa-CRM`.

`MaMa-CRM` takes the burden of (usually paper-based) customer contacts from organizations working in such mass markets. It was initially built by an independent mid-sized data center to support the launch of the German (government-enforced) *e-Health Card* and was later used to support campaigns such as telephone billing, electrical-power metering, and similar stuff.

The architecture of `MaMa-CRM` is covered in *Chapter 3, Mass Market Customer Relationship Management.*

### biking2

`biking2` or "*Michis mileage*" is a web-based application for tracking biking-related activities. It allows the user to track the mileage covered, collect GPS tracks of routes, convert them to different formats, track the location of the user, and publish pictures of bike tours.

The architecture of `biking2` is covered in *Chapter 4, biking2*.

### DokChess – a Chess Engine

`DokChess` is a fully functional chess engine. The architecture of `DokChess` is covered in *Chapter 5, DokChess*.

### docToolchain

`docToolchain` ([https://doctoolchain.github.io/docToolchain](https://doctoolchain.github.io/docToolchain)) is a heavily used and highly practical implementation of the `docs-as-code` ([https://docs-as-co.de](https://docs-as-co.de)) approach. The basic idea is to facilitate the creation and maintenance of technical documentation.

The architecture of `docToolchain` is covered in *Chapter 6, docToolChain*.

### MiniMenu

`MiniMenu` surely has some of the *leanest* architecture documentation you will ever encounter. It captures some design and implementation decisions for a tiny MacOS menu bar application.

The main goal is to show that `arc42` even works for very small systems. Skip to *Chapter 7, Mac OS Menubar Application*, to see for yourself.

## I.5 A table of arc42 Topics

This table will help you to quickly find the `arc42` topics you're looking for. For each section of `arc42`, you'll find references to each of the book's examples. The digits after the links denote the size of the section in the respective example. We have marked the most extensive or complete examples in bold so that you can easily recognize them:

![Figure 1.3: Table of arc42 topics](image/B15546_01_03.jpg)

#### Figure 1.3: Table of arc42 topics

# Chapter 2

## II - HTML Sanity Checking

By **Dr. Gernot Starke**

The system that's documented in this lesson is a small open source tool that's hosted on GitHub ([https://github.com/aim42/htmlSanityCheck](https://github.com/aim42/htmlSanityCheck)).
