Another aspect of performance testing is to test the load of the system your application is running on, which can be quite useful for IoT applications. IoT devices are generally constrained in some way - memory, CPU, storage, etc - whereas cloud services can scale as much as we need them to. It stands to reason then, that our first IoT applications - coming up in Chapter 2 - will set the stage for monitoring each device’s performance individually.

Since performance testing often goes hand-in-hand with both integration and unit testing, we’ll continue to use Maven and specialized Unit Tests for this as well, along with open source tools where needed.

> **NOTE**
> 
> There are many performance testing tools available, and you can also write your own. While system-to-system and communications protocol performance testing is completely optional for the purposes of this book, I’ll cover it in more detail beginning in Part III, Chapter 5, where I’ll introduce Locust ⁸, a tool you can use in your local environment to test scalability for your solution. Locust allows for extensions that enable you to test many different protocols and connection paradigms, which will be important for device-to-device communications testing between your GDA and CDA.

# Testing Tips for the Exercises in this Book