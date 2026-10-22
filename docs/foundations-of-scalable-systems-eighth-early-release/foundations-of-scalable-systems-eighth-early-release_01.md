## Preface

### A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at vwilson@oreilly.com.

This book is built around the thesis that the ability of software systems to operate at scale is increasingly a driving system quality. As our world becomes more interconnected, this characteristic will only accelerate. Hence the goal of the book is to provide the reader with the core knowledge of distributed and concurrent systems. It will also introduce a collection of software architecture approaches and distributed technologies that can be used to build scalable systems.

### Why Scalability?

The pace of change in our world is daunting. Innovations appear daily, creating new capabilities for us all to interact with, conduct business, be entertained, end pandemics. The fuel for much of this innovation is software, written by veritable armies of developers in major internet companies, crack small teams in startups, and all shapes and sizes of teams in between.

Delivering software systems that are responsive to user needs is difficult enough, but it becomes an order of magnitude more difficult to do for

systems at scale. We all know of systems that fail suddenly when exposed to unexpected high loads - such situations are minimally bad publicity for organizations, and at worst can lose jobs and destroy companies.

Software is unlike physical systems in that it’s amorphous—its physical form (1’s and 0’s) bears no resemblance to its actual capabilities. We’d never expect to transform a small village of 500 people into a city of 10 million overnight. But we sometimes expect our software systems to suddenly handle 1000x the number of requests they were designed for. Not surprisingly, the outcomes are rarely pretty.

### Who This Book Is For

The major target audience for this book is software engineers and architects who have no or limited experience with distributed, concurrent systems. They need to deepen both their theoretical and practical design knowledge in order to meet the challenges of building larger scale, typically Internet-facing applications.

Much of the content of this book has been developed in the context of an advanced undergraduate/graduate course at Northeastern University. It has proven a very popular and effective approach for equipping students with the knowledge and skills needed to launch their careers with major Internet companies. Additional materials on the book web site are available to support educators who wish to use the book for their course.

### What You Will Learn

This book covers the landscape of concurrent and distributed systems through the lens of scalability. While it’s impossible to totally divorce scalability from other architectural qualities, scalability is the main focus of discussion. Of course, other qualities necessarily come in to play, with performance, availability and consistency regularly raising their heads.

Building distributed systems requires some fundamental understanding of distribution and concurrency - this knowledge is a recurrent theme throughout this book. It’s needed because at their core, there are two problems in distributed systems that make them complex, as I describe below.

First, although systems operate perfectly correctly nearly all the time, an individual part of the system may fail at any time. When a component fails (hardware crash, network down, bug in server), we have to employ techniques that enable the system as a whole to continue operations and recover from failures. And any distributed system will experience component failure, often in weird and mysterious and unanticipated ways.

Second, creating a scalable distributed system requires the coordination of multiple moving parts. Each component of the system needs to keep its part of the bargain and process requests as quickly as possible. If just one component causes requests to be delayed, the whole system may perform poorly and even eventually crash.

To deal with these problems there is a rich deep body of literature available to draw on. And luckily for us engineers, there’s a rich, extensive collection of technologies that are designed to help us build distributed systems that are tolerant to fail and scalable. These technologies embody theoretical approaches and complex algorithms that are incredibly hard to build correctly. Using these platform level, widely applicable technologies, our applications can stand on the shoulders of giants, enabling us to build sophisticated business solutions.

Specifically, readers of this book will learn:

- The fundamental characteristics of distributed systems, including state management, time coordination, concurrency, communications and coordination
- Architectural approaches and supporting technologies for building scalable, robust services
- How distributed databases operate and can be used to build scalable distributed systems
- Architectures and technologies such as Apache Kafka and Flink for building streaming, event-based systems




