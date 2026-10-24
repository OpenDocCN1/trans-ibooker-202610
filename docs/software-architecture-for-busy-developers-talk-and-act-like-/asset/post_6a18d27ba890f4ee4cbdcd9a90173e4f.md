
Chapter 6, Impact of the Cloud on the Software Architecture Practices, walks you through the most important aspects to grasp when dealing with the cloud and cloud-native applications.

Chapter 7, Architectural Trends and Global Summary, focuses on the most in-demand software architectures and summarizes what we have learned in this book.

## To get the most out of this book

Prior knowledge of software development is expected to have the best reading experience. Since the software architecture topic itself is technology-agnostic, you do not need language-specific skills nor language-specific tools. However, for the sake of demonstration, some examples are based on .NET, Azure, and Kubernetes. If you want to reproduce them in your own environment, you will need the following:

| Software | Operating system requirements |
| .NET Core or .NET 5 | Any operating system |
| An Azure subscription |  |
| Docker and Kubernetes |  |

Rest assured that you will be able to fully grasp the concepts discussed in this book even if you decide not to replay the examples by yourself.

If you are using the digital version of this book, we advise you to type the code yourself or access the code from the book's GitHub repository (a link is available in the next section). Doing so will help you avoid any potential errors related to the copying and pasting of code.

## Download the example code files

You can download the example code files for this book from GitHub at https://github.com/PacktPublishing/Software-Architecture-for-Busy-Developers. If there’s an update to the code, it will be updated in the GitHub repository.

We also have other code bundles from our rich catalog of books and videos available at https://github.com/PacktPublishing/. Check them out!

## Download the color images

We also provide a PDF file that has color images of the screenshots and diagrams used in this book. You can download it here: https://static.packt-cdn.com/downloads/9781801071598_ColorImages.pdf.

## Conventions used

There are a number of text conventions used throughout this book.

Code in text: Indicates code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles. Here is an example: "Let's say that we have a Rectangle base class with two separate SetWidth and SetHeight methods."

A block of code is set as follows:

```
Rectangle rect = new Square();
rect.setWidth(10);
rect.setHeight(5);
Assert.Equal(50, CalculateArea(rect));
```

Bold: Indicates a new term, an important word, or words that you see onscreen. For instance, words in menus or dialog boxes appear in bold. Here is an example: "I defined the MessageBroker as an ABB, and the three rectangles on the right are solutions that fulfil this need."

Tips or important notes
Appear like this.

## Get in touch

Feedback from our readers is always welcome.

General feedback: If you have questions about any aspect of this book, email us at customercare@packtpub.com and mention the book title in the subject of your message.

Errata: Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you have found a mistake in this book, we would be grateful if you would report this to us. Please visit www.packtpub.com/support/errata and fill in the form.

Piracy: If you come across any illegal copies of our works in any form on the internet, we would be grateful if you would provide us with the location address or website name. Please contact us at copyright@packt.com with a link to the material.

If you are interested in becoming an author: If there is a topic that you have expertise in and you are interested in either writing or contributing to a book, please visit authors.packtpub.com.

## Share Your Thoughts

Once you've read *Software Architecture for Busy Developers*, we'd love to hear your thoughts! Please click here to go straight to the Amazon review page for this book and share your feedback.

Your review is important to us and the tech community and will help us make sure we're delivering excellent quality content.

## Section 1:
Introduction

In this part, I'll introduce software architecture, what we are talking about and how it is reflected in the real world. I'll also explain what is in scope for the book and what's not.

This section comprises the following chapter:

- Chapter 1, Introducing Software Architecture

## 1
Introducing
Software
Architecture

In this chapter, I will introduce the subject of software architecture. My purpose is to help you understand my vision of software architecture and how I will tackle this topic throughout the book.

More specifically, this chapter covers the following topics:

- Software architecture in a nutshell
- A software architect's duties
- Introducing the different architecture disciplines
- Positioning software architecture within the global architecture landscape

By the end of the chapter, you should have a better grip on software architecture and a better idea of how this book will walk you through your software architecture journey.

## Software architecture in a nutshell

However rich the literature is on the topic, it's not so easy to find a common definition of software architecture. We as architects like to decouple things, so let's decouple the words **software** and **architecture**. Starting with **software**, we can give this broad definition: *computer programs*. Our second word, **architecture**, can be defined as *the art of designing buildings, houses, and the like*. If we extrapolate a bit, we could define software architecture as *the art of designing computer programs*. This definition sounds very broad.

Rest assured, we can evacuate **hardware** from the equation because it represents the machines themselves. Phew—this should make our task easier, although we are left with everything that runs on a piece of hardware, which remains extremely vast.

Searching for software architecture on Google reveals about 262,000 results, which is very impressive. So many results probably mean a lot of different definitions and a lack of a common understanding of what software architecture is all about. The same query on Google Images does not bring up a single diagram that could help up figure out what software architecture is.

Given the diversity of definitions, I will provide my own, although I don't claim to have the absolute truth. I will start by tying software architecture to the **Architecture Tradeoff Analysis Method (ATAM)**, which we will see in *Chapter 3, Understanding ATAM and the Software Quality Attributes*. In a nutshell, ATAM consists of evaluating different quality attributes—such as performance, availability, reliability, and so on—of a piece of software. Some of these attributes are code-related, whereas some are infrastructure- or security-related.

Although there is no single definition of software architecture, one thing is absolutely certain: *a software architect should be acquainted with ATAM*. Another thing that appears as an emerging consensus is that the role of a software architect is tightly coupled with the actual development of an application, and I share this viewpoint. This is how *Wikipedia* (https://en.wikipedia.org/wiki/Software_architect) defines software architecture, but for me, software architecture goes far beyond mere coding, and that is what you will find out while reading this book. Let's now look at what a software architect's job description might look like.

## A software architect's duties

Sometimes, a good job description helps understand the tasks and duties pertaining to a given position. Here again, looking for such descriptions on the web gives many different results, but this is what I think are the responsibilities of a software architect:

- **Addressing both functional requirements (FRs) and non-functional requirements (NFRs)**: As you know, FRs are the primary trigger to design a solution. Whether the solution/service is designed for the business or for the **Information Technology (IT)** department itself, you make your business case and then start the design work. NFRs (availability, security, **disaster recovery (DR)**, and so on) are not always expressed but are also particularly important and are often the most challenging part.

This book will help you address these requirements in a structured way. Addressing both FRs and NFRs is also the duty of a solution architect, so it can be debated whether this should fall under the duties of a software architect or not. I think that a *good* software architect is able to address FRs. An *excellent* software architect can address both while addressing FRs in a more in-depth way than a solution architect.

- **Providing technical standards, coding guidelines, and design patterns to developers**: Functional features are an integral part of the code base. It's a no-brainer that good design patterns usually improve the resulting quality of a solution. As a software architect, you must understand them and you must be able to sense which pattern is valuable in your own context. We will dive into design patterns in *Chapter 5, Design Patterns and Clean Architecture*, as well as in *Chapter 6, Impact of the Cloud on the Software Architecture Practice*, where we will explore cloud and cloud-native patterns.

- **Interacting with stakeholders to ensure developed applications land smoothly in the company's landscape**: A successful software architect understands multilateral concerns. They are able to interact with security, infrastructure, solution, and enterprise architects, as well as with developers.

This book will give you the essential keys to achieve **T-shaped skills**, which means being an expert in a given field (the base of the *T*) but also knowledgeable across disciplines (the top of the *T*). To reinforce your *T* top bar, we will explore some fundamentals of **enterprise architecture (EA)**, which is a common practice in large organizations, and we will touch on some infrastructure and security typical frameworks. However, I have to manage your expectations here, as the book will only *introduce* these topics.
