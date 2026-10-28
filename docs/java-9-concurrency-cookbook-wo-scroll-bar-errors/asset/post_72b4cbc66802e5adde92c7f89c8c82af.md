
+   How it works... 281
+   There's more... 282
+   See also 283
+   **Verifying conditions in the elements of a stream** 283
+   Getting ready 283
+   How to do it... 283
+   How it works... 285
+   There's more... 286
+   See also 287
+   **Reactive programming with reactive streams** 287
+   Getting ready 288
+   How to do it... 288
+   How it works... 292
+   There's more... 294
+   **Chapter 7: Concurrent Collections** 295
+   **Introduction** 295
+   **Using non-blocking thread-safe deques** 296
+   Getting ready 297
+   How to do it... 297
+   How it works... 300
+   There's more... 301
+   **Using blocking thread-safe deques** 302
+   Getting ready 302
+   How to do it... 302
+   How it works... 304
+   There's more... 305
+   See also 305
+   **Using blocking thread-safe queue ordered by priority** 305
+   Getting ready 306
+   How to do it... 306
+   How it works... 309
+   There's more... 310
+   See also 311
+   **Using thread-safe lists with delayed elements** 311
+   Getting ready 312
+   How to do it... 312
+   How it works... 315
+   There's more... 317
+   See also 317

## Using thread-safe navigable maps 317

Getting ready 318

How to do it... 318

How it works... 321

There's more... 322

See also 322

## Using thread-safe HashMaps 323

Getting ready 323

How to do it... 324

How it works... 327

There's more... 328

See also 329

## Using atomic variables 330

Getting ready 331

How to do it... 331

How it works... 335

There's more... 337

See also 337

## Using atomic arrays 337

Getting ready 338

How to do it... 338

How it works... 341

There's more... 342

See also 342

## Using the volatile keyword 342

Getting ready 343

How to do it... 343

How it works... 345

There's more... 346

See also 346

## Using variable handles 347

Getting ready 347

How to do it... 347

How it works... 349

There's more... 350

See also 351

## Chapter 8: Customizing Concurrency Classes 352

Introduction 353

Customizing the ThreadPoolExecutor class 353

+   Monitoring an Executor framework 440
+     Getting ready 440
+     How to do it... 440
+     How it works... 443
+     See also 444
+   Monitoring a fork/join pool 444
+     Getting ready 444
+     How to do it... 444
+     How it works... 448
+     See also 448
+   Monitoring a stream 449
+     Getting ready 449
+     How to do it... 449
+     How it works... 450
+     See also 451
+   Writing effective log messages 451
+     Getting ready 451
+     How to do it... 451
+     How it works... 456
+     There's more... 457
+     See also 457
+   Analyzing concurrent code with FindBugs 457
+     Getting ready 458
+     How to do it... 458
+     How it works... 461
+     There's more... 462
+     See also 463
+   Configuring Eclipse for debugging concurrency code 463
+     Getting ready 463
+     How to do it... 463
+     How it works... 464
+   Configuring NetBeans for debugging concurrency code 467
+     Getting ready 467
+     How to do it... 467
+     How it works... 471
+     There's more... 472
+     See also 473
+   Testing concurrency code with MultithreadedTC 473
+     Getting ready 473

Chapter 4, Thread Executors, explores the delegation of thread management to executors. They allow running, managing, and getting the results of concurrent tasks.

Chapter 5, Fork/Join Framework, covers the use of the Fork/Join framework. It’s a special kind of executor oriented to execute tasks that will be divided into smaller ones using the divide and conquer technique.

Chapter 6, Parallel and Reactive Streams, teaches you how to create streams and use all its intermediate and terminal operations to process big collections of data in a parallel and functional way. Streams were introduced in Java 8. Java 9 has included some new interfaces to implement reactive streams.

Chapter 7, Concurrent Collections, explains how to use some concurrent data structures provided by the Java language. These data structures must be used in concurrent programs to avoid the use of synchronized blocks of code in their implementation.

Chapter 8, Customizing Concurrency Classes, teaches you how to adapt some of the most useful classes of the Java concurrency API to your needs.

Chapter 9, Testing Concurrent Applications, covers how to obtain information about the status of some of the most useful structures of the Java 7 concurrency API. You will also learn how to use some free tools to debug concurrent applications, such as the Eclipse, NetBeans IDE, or FindBugs applications to detect possible bugs in your applications.

Chapter 10, Additional Information, explores the notions of synchronization, the executor, the Fork/Join framework, concurrent data structures, and the monitoring of concurrent objects, which were not included in the respective chapters.

Chapter 11, Concurrent Programming Design, provides some tips that every programmer should consider when they develop a concurrent application.

## What you need for this book

To follow this book, you need some basic knowledge of the Java programming language. You should know how to use an IDE, such as Eclipse or NetBeans, but this is not a necessary prerequisite.

## Customer support

Now that you are the proud owner of a Packt book, we have a number of things to help you to get the most from your purchase.

## Downloading the example code

You can download the example code files for this book from your account at http://www.packtpub.com. If you purchased this book elsewhere, you can visit http://www.packtpub.com/support and register to have the files e-mailed directly to you.

You can download the code files by following these steps:

- 1. Log in or register to our website using your e-mail address and password.
- 2. Hover the mouse pointer on the SUPPORT tab at the top.
- 3. Click on Code Downloads & Errata.
- 4. Enter the name of the book in the Search box.
- 5. Select the book for which you're looking to download the code files.
- 6. Choose from the drop-down menu where you purchased this book from.
- 7. Click on Code Download.

You can also download the code files by clicking on the Code Files button on the book's webpage at the Packt Publishing website. This page can be accessed by entering the book's name in the Search box. Please note that you need to be logged in to your Packt account.

Once the file is downloaded, please make sure that you unzip or extract the folder using the latest version of:

- WinRAR / 7-Zip for Windows
- Zipeg / iZip / UnRarX for Mac
- 7-Zip / PeaZip for Linux

The code bundle for the book is also hosted on GitHub at https://github.com/PacktPublishing/Java-9-Concurrency-Cookbook-Second-Edition. We also have other code bundles from our rich catalog of books and videos available at https://github.com/PacktPublishing/. Check them out!

## 1

## Thread Management

In this chapter, we will cover the following topics:

- - Creating, running, and setting the characteristics of a thread
- - Interrupting a thread
- - Controlling the interruption of a thread
- - Sleeping and resuming a thread
- - Waiting for the finalization of a thread
- - Creating and running a daemon thread
- - Processing uncontrolled exceptions in a thread
- - Using thread local variables
- - Grouping threads and processing uncontrolled exceptions in a group of threads
- - Creating threads through a factory

## Introduction

In the computer world, when we talk about concurrency, we refer to a series of independent and unrelated tasks that run simultaneously on a computer. This simultaneity can be real if the computer has more than one processor or a multi-core processor, or it can be apparent if the computer has only one core processor.
