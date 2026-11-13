
# Cloud Providers Comparison

## IBM Cloud

Overall, IBM Cloud can certainly host your application and scale it up and down. It is, however, less documented and available than AWS or Google, mostly due to its enterprise focus.

## Microsoft Azure

Microsoft Azure has been a popular newcomer to the cloud scene. Naturally, Azure is focused on Microsoft products such as `.NET`. It does, however, offer general computing resources as well. Since it provides Docker hosting, we can also host our Vapor applications there. Azure also provides autoscaling services, along with monitoring.

Just like AWS, Google, and DigitalOcean, it also offers a managed Kubernetes service, which can be used for autoscaling as well.

## Oracle Cloud

Similar to IBM, Oracle had enterprise customers as its focus. It offers Kubernetes and Docker hosting, just like the others, and so autoscaling and monitoring are mostly a matter of configuration.

All in all, almost every competitive cloud provider will offer autoscaling capabilities, as well as the related monitoring services. The choice of whom to choose comes mainly down to preference and budget. The prices vary, and you should consider the long-term effect. But overall, all providers are stable and reliable enough to host and scale your application.

# Summary

In this chapter, we looked carefully at scaling and monitoring microservices. We started by looking at what monitoring and autoscaling are conceptually, and how they are generally implemented.

Then, we looked into how autoscaling is done with AWS, Google Cloud, and DigitalOcean. We then explored additional cloud providers. By now, you should have a solid understanding of how the different cloud providers operate, and what benefits they offer.

When you launch your next API using Vapor, you should be well equipped to have your services autoscale up and down according to traffic. Especially in projects where the demand is not foreseeable, this becomes invaluable.

# Questions

To make sure you understand this chapter well, go through the following questions:

1.  Which types of scaling are there?

2.  How is autoscaling done in AWS ECS?

3.  Why is Google Cloud's App Engine convenient when it comes to scaling?

# Assessment Answers

Here, you can see the questions and answers to all chapters:

## Chapter 1: Introduction to Microservices

### Questions

1.  What is the definition of a microservice?

2.  List three reasons why microservices can be helpful.

3.  List three reasons not to choose microservices.

4.  List four differences between a monolith and microservices.

### Answers

1.  A microservice is a small business unit operating as independently as possible.

2.  Three reasons:
    1.  Independently scalable
    2.  Independently maintainable
    3.  Decreased complexity

3.  Three reasons:
    1.  Too small for a project (only one microservice).
    2.  Not enough time or resources available to convert all legacy code.
    3.  Increased initial work.

4.  Four differences:
    1.  A monolith has one code base; each microservice has its own.
    2.  Monoliths usually operate with only one database; each microservice has its own database.
    3.  Monoliths are hard to scale as the entire service must be scaled, while microservices can be scaled individually.
    4.  Monoliths force you to use the same programming language for additions. Microservice additions can have different programming languages.

## Chapter 2: Understanding Server-Side Swift

### Questions

1.  Why is it helpful that Swift is a native language?

2.  What is the preferred operating system for server-side Swift applications at the moment?

3.  What is `SwiftNIO`?

4.  How does Docker help?

5.  Name three of the added benefits of Swift 5.

### Answers

1.  Because it is faster, more secure, and easy to deploy.

2.  Ubuntu.

3.  A network and streaming library, written in and for Swift.

4.  Docker provides flexibility when hosting Swift applications.

5.  Three benefits:
    1.  Extensions
    2.  Generic functions
    3.  Protocols

## Chapter 3: Getting Started with the Vapor Framework

### Questions

1.  Add a new route to your FirstApp application that interacts with a new controller.

2.  Add a new model to your application and integrate it into your controllers.

3.  Build a simple pagination app in which the requests can contain parameters such as `sortBy` and `offset`.

### Answers

1.  See GitHub, `Chapter2/FirstAppExtended`.

2.  See GitHub, `Chapter2/FirstAppExtended`.

3.  See GitHub, `Chapter2/FirstAppExtended`.

## Chapter 4: Planning an Online Store Application

### Questions

1.  How do you verify that a user has access to a function of a microservice?

2.  What is a good lifespan for an access token?

3.  Why is JSON a good choice for the inputs and outputs of microservices?

### Answers

1.  A `JSON Web Token` (`JWT`) middleware (`SimpleJWTMiddleware`) verifies that only permitted users can access a function of a microservice.

2.  This depends on the case. Some cases warrant 60 seconds, while others can have a lifespan of multiple hours or even days.

3.  Because it is universally readable and lots of libraries support it well in many programming languages and environments.

## Chapter 5: Creating Your First Microservice

### Questions

1.  Why have we included `SimpleJWTMiddleware`?

2.  How would you most likely run your microservices, and what server environment would you use?

3.  Should you define models for your services altogether, or individually per service?

### Answers

1.  Because it provides us with a basic middleware that takes care of authentication.

2.  In Docker, with Ubuntu.

3.  To keep services as independent as possible, they should be defined individually. You might decide to share a common library, but that is not the case typically.

## Chapter 6: Application Structure and Database Design

### Questions

1.  Why is it essential to use different repositories for Git and Docker for each service?

2.  How do your services receive credentials for the database and JWT?

3.  How can you run multiple Vapor applications at once?

4.  Advanced: Push your Docker image into your Docker repository.

### Answers

1.  We need to ensure the integrity of our services. Combining repositories can be dangerous as it invites code to get mixed.

2.  Through environment variables.

3.  By defining different ports for each service.

4.  See [Chapter 15](#0ebe9276-6cfd-4219-9aa6-c6378525eac4.xhtml), *Deploying Microservices in the Cloud*, for an example.

## Chapter 7: Writing the User Service

### Questions

1.  Extend the user service so that new users need to confirm their email first.

2.  Save the last login time to a user's model.

3.  Complete the address model by adding attributes such as `Country`, `LAT`, `LNG`, `State`, and so on.

4.  Advanced: Write admin functions to allow an admin user (you have to define what that is) to edit another user's addresses and profile.

### Answers

See GitHub for a full example.

## Chapter 8: Testing Microservices

### Questions

1.  What is the fundamental difference between unit tests and functional tests?

2.  How can unit tests and functional tests work together?

3.  Write Postman tests for all endpoints of the user service.

4.  Write more sophisticated tests when verifying the responses (for example, check that the token contains the correct information).

5.  Write unit tests verifying that the JWT functions in the user service are working correctly.

### Answers

1.  Unit tests are for individual functions. Functional tests are undertaken to test the overall functionality of a service.

2.  While unit tests ensure that foundational functions operate properly, functional tests will ensure that functions are working together as intended.

3.  See GitHub.

4.  See GitHub.

5.  See GitHub.

## Chapter 9: Product Management Service

### Questions

1.  Why do we not need to sign JWT in this service?

2.  Add a new filter that only returns a product within a given price range.
