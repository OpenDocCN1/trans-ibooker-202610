
## NOTE

This element signifies a general note.

## WARNING

This element indicates a warning or caution.

## Using Code Examples

This book is here to help you get your job done. In general, if example code is offered with this book, you may use it in your programs and documentation. You do not need to contact us for permission unless you’re reproducing a significant portion of the code. For example, writing a program that uses several chunks of code from this book does not require permission. Selling or distributing examples from O’Reilly books does require permission. Answering a question by citing this book and quoting example code does not require permission. Incorporating a significant amount of example code from this book into your product’s documentation does require permission.

We appreciate, but generally do not require, attribution. An attribution usually includes the title, author, publisher, and ISBN. For example: “*Observability Engineering* by Charity Majors, Liz Fong-Jones, and George Miranda (O’Reilly). Copyright 2022 Some Hound Technology Inc, 978-1-492-07644-5.”

If you feel your use of code examples falls outside fair use or the permission given above, feel free to contact us at [permissions@oreilly.com](mailto:permissions@oreilly.com).

## O’Reilly Online Learning

> ### NOTE
> 
> For more than 40 years, [O’Reilly Media](https://oreilly.com) has provided technology and business training, knowledge, and insight to help companies succeed.

Our unique network of experts and innovators share their knowledge and expertise through books, articles, and our online learning platform. O’Reilly’s online learning platform gives you on-demand access to live training courses, in-depth learning paths, interactive coding environments, and a vast collection of text and video from O’Reilly and 200+ other publishers. For more information, visit [https://oreilly.com](https://oreilly.com).

## How to Contact Us

Please address comments and questions concerning this book to the publisher:

- O’Reilly Media, Inc.
- 1005 Gravenstein Highway North
- Sebastopol, CA 95472
- 800-998-9938 (in the United States or Canada)
- 707-829-0515 (international or local)
- 707-829-0104 (fax)

We have a web page for this book, where we list errata, examples, and any additional information. You can access this page at [https://oreil.ly/observability-engineering](https://oreil.ly/observability-engineering).

Email [bookquestions@oreilly.com](mailto:bookquestions@oreilly.com) to comment or ask technical questions about this book.

For news and information about our books and courses, visit [https://oreilly.com](https://oreilly.com).

Find us on LinkedIn: [https://linkedin.com/company/oreilly-media](https://linkedin.com/company/oreilly-media)

Follow us on Twitter: [https://twitter.com/oreillymedia](https://twitter.com/oreillymedia)

Watch us on YouTube: [https://youtube.com/oreillymedia](https://youtube.com/oreillymedia)

## Acknowledgments

This book would not have been possible without the support of executive sponsors at Honeycomb: many thanks to Christine Yen, Deirdre Mahon, and Jo Ann Sanders. Nor would this book have been possible without the domestic sponsors who put up with many odd hours, lost weekends, sleepless weeknights, and cranky partners: many thanks to Elly Fong-Jones, Rebekah Howard, and Dino Miranda. Without all of them, we would probably still be trying to find the time to fully develop and tie together the many ideas expressed in this book.

We’d especially like to thank additional contributors who have made the content in this book much stronger by sharing their varied perspectives and expertise. [Chapter 16, “Efficient Data Storage”](#), was made possible by Ian Wilkes (author of Honeycomb’s Retriever engine, the basis of the case study), and Joan Smith for reviewing references to external literature. [Chapter 14, “Observability and the Software Supply Chain”](#), was authored by Frank Chen, and [Chapter 18, “Telemetry Management with Pipelines”](#), was authored by Suman Karumuri, and Ryan Katkov—all of whom we thank for sharing their knowledge and the lessons they’ve learned from managing incredibly large-scale applications with observability at Slack. Many thanks to Rachel (pie) Perkins for contributions to several early chapters in this book. And thanks to the many bees at Honeycomb who, over the years, have helped us explore what’s achievable with observability.

Finally, many thanks to our many external reviewers: Sarah Wells, Abby Bangser, Mads Hartmann, Jess Males, Robert Quinlivan, John Feminella, Cindy Sridharan, Ben Siegelman, and Daniel Spoonhower. We’ve revised

our takes, incorporated broader viewpoints, and revisited concepts throughout the authoring process to ensure that we’re reflecting an inclusive state of the art in the world of observability. Although we (the authors of this book) all work for Honeycomb, our goal has always been to write an objective and inclusive book detailing how observability works in practice, regardless of specific tool choices. We thank our reviewers for keeping us honest and helping us develop a stronger and all-encompassing narrative.

## Part I. The Path to Observability

This section defines concepts that are referenced throughout the rest of this book. You will learn what observability is, how to identify an observable system, and why observability-based debugging techniques are better suited for managing modern software systems than monitoring-based debugging techniques.

Chapter 1 examines the roots of the term “observability,” shows how that concept has been adapted for use in software systems, and provides concrete questions you can ask yourself to determine if you have an observable system.

Chapter 2 looks at the practices engineers use to triage and locate sources of issues using traditional monitoring methods. Those methods are then contrasted with methods used in observability-based systems. This chapter describes these methods at a high level, but the technical and workflow implementations will become more concrete in Part II.

Chapter 3 is a case study written by coauthor Charity Majors and told from her perspective. This chapter brings concepts from the first two chapters into a practical case study illustrating when and why the shift toward observability becomes absolutely necessary.

Chapter 4 illustrates how and why industry trends have helped popularize the need for observability and how that fits into emerging spaces, like the cloud native ecosystem.

and large, the debugging practices developed decades ago with nascent monitoring tools are still the same as those used by many engineering teams today—even though the systems they manage are infinitely more complex. Observability tools were born out of sheer necessity, when traditional tools and debugging methods simply were not up to the task of quickly finding deeply hidden and elusive problems.

This chapter will help you understand what “observability” means, how to determine if a software system is observable, why observability is necessary, and how observability is used to find problems in ways that are not possible with other approaches.

## The Mathematical Definition of Observability

The term “observability” was coined by engineer Rudolf E. Kálmán in 1960. Since then, it has grown to mean many different things in different communities. Let’s explore the landscape before turning to our own definition for modern software systems.

In his 1960 paper, Kálmán introduced a characterization he called *observability* to describe mathematical control systems.¹ In [control theory](#), observability is defined as a measure of how well internal states of a [system](#) can be inferred from knowledge of its external outputs.

This definition of observability would have you study observability and controllability as mathematical duals, along with sensors, linear algebra equations, and formal methods. This traditional definition of observability is the realm of mechanical engineers and those who manage physical systems with a specific end state in mind.

If you are looking for a mathematical and process engineering oriented textbook, you’ve come to the wrong place. Those books definitely exist, and any mechanical engineer or control systems engineer will inform you (usually passionately and at great length) that observability has a formal meaning in traditional systems engineering terminology.
