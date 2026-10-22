
To be able to route new tickets to the tenant’s support agents only during their working hours, WolfDesk allows the entry of each agent’s shift schedule.

Since WolfDesk provides its service with no minimal fee, it has to optimize its infrastructure in a way that minimizes the costs of onboarding a new tenant. To do that, WolfDesk leverages serverless computing, which allows it to elastically scale its compute resources based on the operations on active tickets.

## Conventions Used in This Book

The following typographical conventions are used in this book:

*Italic*

Indicates new terms, URLs, email addresses, filenames, and file extensions.

*Constant width*

Used for program listings, as well as within paragraphs to refer to program elements such as variable or function names, databases, data types, environment variables, statements, and keywords.

> NOTE
> 
> This element signifies a general note.

## Using Code Examples

Supplemental material (code examples, exercises, etc.) is available for download at https://learning-ddd.com.

All the code samples presented in the book are implemented in the C# language. Generally, the code samples you see in the chapters are excerpts demonstrating the discussed concepts.

Of course, the concepts and techniques discussed in the book are not limited to the C# language or to the object-oriented programming approach. Everything is relevant for other languages and other programming paradigms. As a result, feel free to implement the book’s samples in your favorite language and share them with me. I’ll be happy to add them to the book’s website.

If you have a technical question or a problem using the code examples, please email bookquestions@oreilly.com.

This book is here to help you get your job done. In general, if example code is offered with this book, you may use it in your programs and documentation. You do not need to contact us for permission unless you’re reproducing a significant portion of the code. For example, writing a program that uses several chunks of code from this book does not require permission. Selling or distributing examples from O’Reilly books does require permission. Answering a question by citing this book and quoting example code does not require permission. Incorporating a significant amount of example code from this book into your product’s documentation does require permission.

We appreciate, but generally do not require, attribution. An attribution usually includes the title, author, publisher, and ISBN. For example: “Learning Domain-Driven Design by Vlad Khonononov (O’Reilly). Copyright 2022 Vladislav Khononov, 978-1-098-10013-1.”

If you feel your use of code examples falls outside fair use or the permission given above, feel free to contact us at permissions@oreilly.com.

## O’Reilly Online Learning

### NOTE

For more than 40 years, *O’Reilly Media* has provided technology and business training, knowledge, and insight to help companies succeed.

Our unique network of experts and innovators share their knowledge and expertise through books, articles, and our online learning platform. O’Reilly’s online learning platform gives you on-demand access to live training courses, in-depth learning paths, interactive coding environments, and a vast collection of text and video from O’Reilly and 200+ other publishers. For more information, visit http://oreilly.com.

## How to Contact Us

Please address comments and questions concerning this book to the publisher:

O’Reilly Media, Inc.

1005 Gravenstein Highway North

Sebastopol, CA 95472

800-998-9938 (in the United States or Canada)

707-829-0515 (international or local)

707-829-0104 (fax)

We have a web page for this book, where we list errata, examples, and any additional information. You can access this page at https://oreil.ly/lddd.

Email bookquestions@oreilly.com to comment or ask technical questions about this book.

For news and information about our books and courses, visit http://oreilly.com.

Find us on Facebook: http://facebook.com/oreilly

Follow us on Twitter: http://twitter.com/oreillymedia

Watch us on YouTube: http://youtube.com/oreillymedia

## Acknowledgments

Originally, this book was titled “What Is Domain-Driven Design?” and was published as a report in 2019. Learning Domain-Driven Design would not have seen the light of day without the report, and I’m obliged to thank those who made “What Is Domain-Driven Design?” possible: Chris Guzikowski, Ryan Shaw, and Alicia Young.¹

This book also wouldn’t have been possible without O’Reilly’s Content Director and Diversity Talent Lead, Melissa Duffield, who championed the project and made it happen. Thank you, Melissa, for all your help!

Jill Leonard was the book’s development editor, project manager, and head coach. Jill’s role in this work cannot be overstated. Jill, thank you so much for all your hard work and help! Extra thanks for keeping me motivated, even when I considered changing my name and hiding in a foreign country.

A huge thanks to the production team for making the book not only writable but readable: Kristen Brown, Audrey Doyle, Kate Dullea, Robert Romano, and Katherine Tozer. For that matter, I want to thank the whole O’Reilly team for the great work you do. It’s a dream come true to be working with you!

Thanks to all the people I interviewed and consulted with: Zsofia Herendi, Scott Hirleman, Trond Hjorteland, Mark Lisker, Chris Richardson, Vaughn Vernon, and Ivan Zakrevsky. Thank you for your wisdom and for being there when I needed help!

Special thanks to the team of reviewers who read through the early drafts and helped me shape the final book: Julie Lerman, Ruth Malan, Diana Montalion, Andrew Padilla, Rodion Promyshlenikov, Viktor Pshenitsyn, Alexei Torunov, Nick Tune, Vasiliy Vasilyuk, and Rebecca Wirfs-Brock. Your support, feedback, and critique helped immensely. Thank you!

I also want to thank Kenny Baas-Schwegler, Alberto Brandolini, Eric Evans, Marco Heimeshoff, Paul Rayner, Mathias Verraes, and the rest of the amazing domain-driven design community. You know who you are. You are my teachers and mentors. Thank you for sharing your knowledge on social media, blogs, and conferences!

I’m most indebted to my dear wife, Vera, for always supporting me in my crazy projects and trying to guard me from things that could distract me from writing. I promise to finally declutter the basement. It is going to happen soon!

Finally, I want to dedicate this book to our beloved Galina Ivanovna Tyumentseva, who supported me so much in this project and whom we sadly lost during the writing of this book. We will always remember you.

#AdoptDontShop

## Introduction

Software engineering is hard. To be successful at it, we have to learn continuously, whether it’s trying new languages, exploring new technologies, or keeping up with new popular frameworks. However, learning a new JavaScript framework every week is not the hardest aspect of our job. Making sense of new business domains can be far more challenging.

Throughout our careers, it’s not uncommon for us to have to develop software for a diverse range of business domains: financial systems, medical software, online retailers, marketing, and many others. In a sense, that is what differentiates our job from most other professions. People working in other fields are often surprised when they find out how much learning is involved in software engineering, especially when changing workplaces.

Failure to grasp the business domain results in suboptimal implementation of the business software. Unfortunately, that’s quite common. According to studies, approximately 70% of software projects are not delivered on time, on budget, or according to the client’s requirements. In other words, the vast majority of software projects fail. This issue is so deep and widespread that we even have a term for it: software crisis.

The term *software crisis* was introduced all the way back in 1968.¹ One would assume that things would have improved in the intervening 50 years. During those years, numerous approaches, methodologies, and disciplines were introduced to make software engineering more effective: Agile Manifesto, extreme programming, test-driven development, high-level languages, DevOps, and others. Unfortunately, things didn’t change much. Projects are still failing quite often and the software crisis is still here.

Many studies have been conducted to investigate the reasons for the common project failures.² Although researchers have not been able to pinpoint a single cause, most of their findings share a common
