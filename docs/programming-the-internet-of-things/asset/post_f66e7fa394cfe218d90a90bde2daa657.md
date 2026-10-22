
test to verify that data repository connection is both available and working properly before exercising the sorting unit test.

These kinds of dependencies can make integration testing pretty challenging with any environment, and even more so with the IoT, since we sometimes have to set up servers to run specialized protocols to test our stuff. Fortunately, Maven can help with this, too, since it supports plugins which make some of this much easier. For all the code that needs to integrate only within the Edge Tier, we’ll use Maven plus some specialized Unit Tests as part of our integration testing strategy.

Finally, performance tests are useful for testing how quickly, or efficiently, a system handles a variety of conditions. They can be used with both unit and integration tests when, for instance, response time or the number of supported *concurrent*, or simultaneous connections needs to be measured.

Let’s say there are many different systems that need to retrieve a list of data from your data repository, and each one wants that list of data sorted before your application returns it to them. Ignoring system design and database schema optimization for a moment, a series of performance tests can be used to time the responsiveness of each system’s request (from the initial request to the response), as well as the number of concurrent systems that can access your application before it no longer responds adequately.

Another aspect of performance testing is to test the load of the system your application is running on, which can be quite useful for IoT applications. IoT devices are generally constrained in some way - memory, CPU, storage, etc - whereas cloud services can scale as much as we need them to. It stands to reason then, that our first IoT applications - coming up in Chapter 2 - will set the stage for monitoring each device’s performance individually.

Since performance testing often goes hand-in-hand with both integration and unit testing, we’ll continue to use Maven and specialized Unit Tests for this as well, along with open source tools where needed.

> **NOTE**
> 
> There are many performance testing tools available, and you can also write your own. While system-to-system and communications protocol performance testing is completely optional for the purposes of this book, I’ll cover it in more detail beginning in Part III, Chapter 5, where I’ll introduce Locust ⁸, a tool you can use in your local environment to test scalability for your solution. Locust allows for extensions that enable you to test many different protocols and connection paradigms, which will be important for device-to-device communications testing between your GDA and CDA.

## Testing Tips for the Exercises in this Book

The sample code provided for each exercise in this book includes unit tests, which you can use to test the code you’ll write. These unit tests are provided as part of the connected-devices-java and connected-devices-python repositories you’ve already pulled into your new iot-gateway and iot-device (respectively) projects are key to ensuring your implementation works properly.

Some exercises also have integration tests that you can use as-is, or modify to suit your specific needs. I’ve also included some sample performance tests you can use to test how well some of your code performs when under load.

Your implementation of each exercise should pass each provided unit test with 100% success. You’re welcome to add more unit tests if you feel they’ll be helpful to verify the functionality you develop. The integration tests and performance tests provided will be helpful, but not required as part of each exercise.

Remember, tests are your friend. And, like a friend, they shouldn’t be ignored. They can surely be time consuming to write and maintain, but any good friendship takes investment. These tests - whether unit, integration, or performance - will help you validate your design and verify your functionality is working properly.

## Step III: Managing Your Workflow - Requirements, Source Code, and CI/CD

So you’ve figured out how you want to write your code and test it - great! But wouldn’t it be great if you could manage all your requirements, source code, and CI/CD pipelines. Let’s tackle this in our last step, which is all about managing your overall development process workflow. This includes requirements tracking, source code management, and CI/CD automation.

You’re probably sick of me saying that building IoT systems is hard, and that’s largely because of the nature of the Edge Tier (since we often have to deal with different types of devices, communication paradigms, operating environments, security constraints, and so on). Fortunately, there are many modern CI/CD tools that can be used to help navigate these troubled waters. Let’s look at some selection requirements for these tools, and then explore how to build out a CI/CD pipeline that will work for our needs.

Your IoT CI/CD pipeline should support secure authentication and authorization, scriptability from a Linux-like command line, integration with Git and containerization infrastructure, and the ability to run pipelines within my local environment as well as a cloud-hosted environment.

There are many online services that provide these features, some of which provide both free and paid service tiers. When you downloaded the source code for this book, you pulled it from my GitHub ⁹ repositories using Git’s clone feature. GitHub is an online service that supports overall developer workflow management, including source code control (using Git), CI/CD automation, and planning.

Each exercise will build, test, and deploy locally, but also assume your code is committed to an online repository using Git for source code management. You’re welcome to use the online service of your choice, of course. For this book, all examples and exercises will assume GitHub is being used. You can learn more about GitHub’s features and create a free account on their website (https://github.com/).

> **NOTE**
> 
> There are lots of great resources, tools and online services available that let you manage your development work and set up automated CI/CD pipelines. Read through this section, try things out, and then as you gain more experience, choose the tools and service that works best for you.

## Managing Requirements

Ah yes - requirements. What are we building, who cares, and how are we going to build it? Plans are good, are they not? And since they’re good, we should have a tool that embraces goodness, which includes: task prioritization, task tracking, team collaboration, and (maybe) integration with other tools.

Remember the code you cloned from my GitHub repository? Choose one - for example, constraineddeviceapp, and open its URL in a web browser. You’ll notice that it has some Issues (like, literally - there’s a column called ‘Issues’ on the main page). If you click on ‘Issues’, you’ll see all the tasks (requirements) that need to be implemented.

Each task contains the instructions and other details a developer (you) will need to write the code and make sure it works correctly.

So far, so good, right? These tasks - at least in GitHub - can be organized into a board, so that you can see all of the things that need to be implemented for a project. You’ve probably heard of Agile 10 project management processes such as Scrum and Kanban.

One of the workflow organizational approaches GitHub let’s you select for a project is a Kanban-based electronic board. Kanban organizes tasks as cards, which can then be placed into various columns within the board’s workflow depending on their status. This is the approach we’ll use in the book to track things that need to be done for each code repository.

NOTE
I’m managing all of the activities for this book within a Kanban board, too. Each card on the board represents a task I (or one of my team members) needs to complete. Cards only move to ‘Done’ after Sarah approves (thanks Sarah!)
