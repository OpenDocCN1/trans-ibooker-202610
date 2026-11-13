
Let's now look at a related topic, generic functions.

# Using generic functions

A generic function is one that can take multiple types and work with them equally. Especially in server applications, where you deal with a lot of data, this becomes very handy. Instead of writing a function for each type of information, you can write it once and use it for every type.

Let's look at an example:

```swift
function saveModel<T:Model>(_ model: T, on app: Application) -> EventLoopFuture<T> {
    return model.save(on: app).transform(to: model)
}
```

You can see that the function is using a generic type `T` that we call `model` in the signature of the function. We are also defining that this type has to implement `Model`.

Now, whenever we want to save a class that conforms to `Model`, we can do so as follows:

```swift
let user = User()
return self.saveModel(user, on: app)
```

See how such a function could be very useful when doing operations on a number of types that are all conforming to the same superclass or protocol?

In this simple example, we are not saving much, but in projects where you might want to perform a good number of transformations before saving a model, generic functions can be life-saving.

Now we have looked at the perks of the Swift language specifically. Extensions, protocols, and generic functions should now be part of your portfolio when writing server-side Swift applications! Let's now look at when to be abstract and when to be concrete in your implementations.

# Using abstract and concrete implementations

When writing software, we are often faced with making decisions about how concrete our code is. For example, if you are tasked with writing a chat application, you need to have answers to the following questions:

*   Is the chat between two people or can it be more than two?
*   What kind of content is being exchanged? Text, image, video, audio, or a combination? Maybe all of it?
*   How long of a chat history should be stored?

You can probably easily spot further questions that can come up. In a well-planned and thoroughly thought-through project, there would be an answer to all these questions. However, in almost all the projects that I have seen, many of these questions are overlooked, and well-intended projects still face these questions. Some of these questions are also too technical for the people who tasked you with this. However, let's pretend for a moment you have all the answers from your project manager. You sense, though, that, in the future, some updates will come your way. You are basically faced with the following two options:

*   Option A: Write the app exactly matching the specifications.
*   Option B: Write the app to be as generic and abstract as possible to allow for changes later on.

How do those two options differ? Take a look at what it could mean.

**Option A:**

*   Only two people can chat.
*   It is only text and images.
*   Chat history is no longer than 3 days.

**Option B:**

*   As many people as needed can communicate with each other.
*   Every possible media format is supported.
*   Chat history is limitless.

You can probably tell that option B requires a lot more work, testing, and overall effort. Option A is what is requested. On a tight schedule and budget, the obvious choice would be A. It will do the job, but requires major restructuring when changes come down the line. In my experience, there is almost always some tension between what you, as the developer, envision it could be and what is requested.

As a general rule, try to stay as abstract as is reasonable without killing the timeline or budget. In the previous example, it makes no sense to go for the entirety of option B, but it probably does make sense to prepare the way for it. For example, you could implement it so that it would later be easier to have more than just two people chat with each other. You could also allow generic media, but only implement images at the moment. Especially with microservices, you *can* be abstract rather easily, so take advantage of it.

# Combining microservices

Off the back of the previous *Using abstract and concrete implementations* section, sometimes you may discover that you have been too generic and abstract. Adding microservices to your project does have a lot of benefits, but it also increases complexity and, depending on the dataflow, can introduce a lot of bugs. Especially when information is passed along from one service to another, a few things that can happen are as follows:

*   Information is duplicated in each service.
*   Each service verifies and validates inputs.
*   Each service requires hosting resources (the servers the service runs on).
*   Latency and overall slowness.

Let's take a look at the following example.

We have five microservices:

*   User management service
*   Address management service
*   Payment information service
*   Payment service
*   Order service

When a user submits an order and wants to pay for it with their credit card, the following needs to happen:

1.  The user management service needs to verify whether this user is still active and allowed to buy.
2.  The address management service needs to return the correct address and send it to the payment service as well as the order service.
3.  The payment information service needs to send the payment information to the payment service.
4.  The payment service needs to send confirmation to the order service.
5.  The order service needs to verify with each service and process the incoming data.
6.  The order service sends an email through the user management service.

There are six steps and in all but the first step, microservices are talking to each other. This is quite messy to debug and maintain. Instead, imagine we combined the user and address management services into one, and we combined the payment information service, payment service, and order service into one service. Now we only have two services:

*   User address management service
*   Order service

Our steps now look like the following:

1.  The user address management service can verify the user and provide the correct address for the order service.
2.  The order service takes the information, loads internally the payment information, and processes the order.
3.  The order service sends an email through the user management service.

That's only three steps now and only two services! This streamlines the dataflow. We also reduce the amount of data that needs to be exchanged between services, which reduces latency and makes things a bit faster.

Does this work every time? Of course not. In some cases, this is golden, in others, you want the opposite. In general, if more than two services need to talk to each other in a business logical step, you can probably reduce the dataflow. Not always by combining microservices, but that is a relatively easy way.

# Troubleshooting and debugging microservices

You should have a general idea of how to debug microservices now since we covered this well in [](#cdfd1f9b-2194-40cc-97dd-9c3d49613bfd.xhtml)Chapter 8, *Testing Microservices*. In this section, we are going to just look at a few core concepts that will tie this together with troubleshooting. The underlying question is usually: Why does it not do what I want it to do? In this section, you will learn how to find the answer.

Let's dive into the following topics:

*   Using `Xcode` for debugging: `Xcode` offers some convenient tools for debugging.
*   Using `lldb` for debugging: `lldb` is a great way to debug yourself in the command line.
*   Staging and live systems: Separating your development from production is essential for every project.

We are beginning with debugging using `Xcode`.
