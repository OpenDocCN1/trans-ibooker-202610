
- Creating a console app for working with EF Core 409
- Using a sample relational database 409
- Using Microsoft SQL Server for Windows 410
  - Downloading and installing SQL Server 411
- Creating the Northwind sample database for SQL Server 412
- Managing the Northwind sample database with Server Explorer 413
- Using SQLite 414
  - Setting up SQLite for macOS 414
  - Setting up SQLite for Windows 414
  - Setting up SQLite for other OSes 414
- Creating the Northwind sample database for SQLite 415
- Managing the Northwind sample database with SQLiteStudio 415
- Setting up EF Core 417
  - Choosing an EF Core database provider 417
  - Connecting to a database 417
  - Defining the Northwind database context class 418
- Defining EF Core models 420
  - Using EF Core conventions to define the model 421
  - Using EF Core annotation attributes to define the model 421
  - Using the EF Core Fluent API to define the model 423
    - Understanding data seeding with the Fluent API 423
- Building an EF Core model for the Northwind tables 423
  - Defining the Category and Product entity classes 424
- Adding tables to the Northwind database context class 426
- Setting up the dotnet-ef tool 427
- Scaffolding models using an existing database 428
- Configuring preconvention models 432
- Querying EF Core models 433
  - Filtering included entities 435
    - Unicode characters in the Windows console 436
  - Filtering and sorting products 437
  - Getting the generated SQL 438
  - Logging EF Core using a custom logging provider 439
    - Filtering logs by provider-specific values 442
    - Logging with query tags 443
  - Pattern matching with Like 444
  - Defining global filters 445
- Loading patterns with EF Core 446
  - Eager loading entities 446
  - Enabling lazy loading 447
  - Explicit loading entities 448
- Manipulating data with EF Core 450
  - Inserting entities 450
  - Updating entities 452
  - Deleting entities 453
  - Pooling database contexts 454
### Working with transactions 454
### Controlling transactions using isolation levels 455
### Defining an explicit transaction 455
### Code First EF Core models 456
### Understanding migrations 463
### Practicing and exploring 464
### Exercise 10.1 – Test your knowledge 464
### Exercise 10.2 – Practice exporting data using different serialization formats 464
### Exercise 10.3 – Explore topics 464
### Exercise 10.4 – Explore NoSQL databases 465
### Summary 465
## Chapter 11: Querying and Manipulating Data Using LINQ 467
### Writing LINQ expressions 467
### What makes LINQ? 467
### Building LINQ expressions with the Enumerable class 468
### Understanding deferred execution 470
### Filtering entities with Where 471
### Targeting a named method 473
### Simplifying the code by removing the explicit delegate instantiation 474
### Targeting a lambda expression 474
### Sorting entities 475
### Sorting by a single property using OrderBy 475
### Sorting by a subsequent property using ThenBy 475
### Declaring a query using var or a specified type 476
### Filtering by type 476
### Working with sets and bags using LINQ 478
### Using LINQ with EF Core 480
### Building an EF Core model 480
### Filtering and sorting sequences 483
### Projecting sequences into new types 485
### Joining and grouping sequences 486
### Joining sequences 487
### Group-joining sequences 488
### Aggregating sequences 490
### Sweetening LINQ syntax with syntactic sugar 491
### Using multiple threads with parallel LINQ 492
### Creating an app that benefits from multiple threads 492
### Using Windows 494
### Using macOS 494
### For all operating systems 494
### Creating your own LINQ extension methods 495
### Trying the chainable extension method 498
### Trying the mode and median methods 498
### Working with LINQ to XML 499
### Generating XML using LINQ to XML 499
### Reading XML using LINQ to XML 500
### Practicing and exploring 501
### Exercise 11.1 – Test your knowledge 501
### Exercise 11.2 – Practice querying with LINQ 502
### Exercise 11.3 – Explore topics 503
### Summary 503
## Chapter 12: Improving Performance and Scalability Using Multitasking 505
### Understanding processes, threads, and tasks 505
### Monitoring performance and resource usage 506
### Evaluating the efficiency of types 506
### Monitoring performance and memory using diagnostics 507
### Useful members of the Stopwatch and Process types 508
### Implementing a Recorder class 508
### Measuring the efficiency of processing strings 510
### Monitoring performance and memory using Benchmark.NET 512
### Running tasks asynchronously 516
### Running multiple actions synchronously 516
### Running multiple actions asynchronously using tasks 518
### Starting tasks 518
### Waiting for tasks 519
### Using wait methods with tasks 519
### Continuing with another task 520
### Nested and child tasks 522
### Wrapping tasks around other objects 523
### Synchronizing access to shared resources 524
### Accessing a resource from multiple threads 525
### Applying a mutually exclusive lock to a conch 526
### Understanding the lock statement 527
### Avoiding deadlocks 528
### Synchronizing events 529
### Making CPU operations atomic 530
### Applying other types of synchronization 531
### Understanding async and await 532
### Improving responsiveness for console apps 532
### Improving responsiveness for GUI apps 533
### Improving scalability for web applications and web services 537
### Common types that support multitasking 537
### Using await in catch blocks 537
### Working with async streams 538
### Practicing and exploring 539
### Exercise 12.1 – Test your knowledge 539
### Exercise 12.2 – Explore topics 539
### Summary 539
## Chapter 13: Introducing Practical Applications of C# and .NET 541
### Understanding app models for C# and .NET 541
### Building websites using ASP.NET Core 542
### Building websites using a content management system 542
### Building web applications using SPA frameworks 543
### Building web and other services 544
### Building mobile and desktop apps 545
### Alternatives to .NET MAUI 545
### Understanding Uno Platform 545
### Understanding Avalonia 546
### New features in ASP.NET Core 546
### ASP.NET Core 1.0 546
### ASP.NET Core 1.1 546
### ASP.NET Core 2.0 546
### ASP.NET Core 2.1 547
### ASP.NET Core 2.2 547
### ASP.NET Core 3.0 548
### ASP.NET Core 3.1 548
### Blazor WebAssembly 3.2 548
### ASP.NET Core 5.0 548
### ASP.NET Core 6.0 548
### Building Windows-only desktop apps 549
### Understanding legacy Windows application platforms 549
### Understanding modern .NET support for legacy Windows platforms 550
### Structuring projects 550
### Structuring projects in a solution or workspace 551
### Using other project templates 552
### Installing additional template packs 552
### Building an entity data model for the Northwind database 553
### Creating a class library for entity models using SQLite 554
### Improving the class-to-table mapping 555
### Creating a class library for a Northwind database context 559
### Creating a class library for entity models using SQL Server 562
### Practicing and exploring 565
### Exercise 13.1 – Test your knowledge 565
### Exercise 13.2 – Explore topics 565
### Summary 565
## Chapter 14: Building Websites Using ASP.NET Core Razor Pages 567
### Understanding web development 567
### Understanding HTTP 567
### Understanding the components of a URL 568
### Assigning port numbers for projects in this book 569
### Using Google Chrome to make HTTP requests 569
### Understanding client-side web development technologies 572
### Understanding ASP.NET Core 572
### Classic ASP.NET versus modern ASP.NET Core 573
### Creating an empty ASP.NET Core project 574
### Testing and securing the website 576
### Enabling stronger security and redirect to a secure connection 579
