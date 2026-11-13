
# Why Choose Microservices

It makes a lot of sense to use microservices not only when your specifications may change, but also in cases where your requirements are highly defined. Microservices are easily maintained; each service can be updated, upgraded, and serviced individually without influencing the other services. This is particularly important with bigger applications as it reduces the risk and impact of potential downtime.

If you are serving a lot of users, you want to avoid downtime as much as possible. Because microservices are independent, you can replace them in time without going offline. You will explore deployments later on in more detail, but most cloud service providers have procedures in place that will keep your application up 100% of the time while switching out the services.

Additionally, you and your team can focus your efforts on the services that need it most. While one service is being heavily updated, the other services can stay the way they were. I have been in a project that had legacy microservices running for over two years because it worked reliably and effort was put into developing new features instead of updating the old ones. You should not neglect updating all parts of your system though, but microservices allow you to keep old infrastructure longer than with a monolith.

In conclusion, microservices allow you to work on them while other services stay as they are and therefore give you the ability to focus on what matters most.

# Scalability

Another element to consider is that you want to be able to grow and scale your application easily. Microservices are arguably the easiest form of application for a server application to grow. Remember, we have individual and independent services running. It means, in principle, that no problem should have the same service run in many instances; for example, you could have 100 servers running the same service. Alternatively, it could also run as only one service, based on your needs.

Let's look into the following three distinct advantages microservices have when it comes to scalability:

* Auto-scaling
* Flexible cost
* Service-oriented availability

## Auto-scaling

You can have your hosting provider automatically scale (up and down) according to your needs. Predefined and customizable rules allow you to define and to scale up and down. If you have an upcoming marketing gig (such as a TV promotion), your backend will automatically scale according to your demand without you having to adjust anything manually.

## Flexible cost

This goes hand-in-hand with auto-scaling. You may pay for the infrastructure depending on your needs. In an ideal world, you would pay nothing for no users and increasingly more as you have more users. In reality, this rarely happens just like that; however, you do end up saving money as you don't need to buy more hardware than you need in most cases.

## Service-oriented availability

Often, not every service is used equally. In the preceding example, the user service only performs authentication and registration. Once the user is authorized, this service is mostly irrelevant to the app. You may decide to have the email processing service be a lot more available than your user service.

Concretely, this may look like five servers running the product manager application while only one server runs the user and the order manager. As your app grows and changes, you can adjust this setup precisely to what you need.

So, we can conclude that microservices allow your application to grow effortlessly and you have many options for how you want your entire backend application to run.

# Mixed Stacks

Sometimes, you may find yourself in a situation where a part of your tech stack is using other technologies. You may want to write your microservices with Vapor and Swift but have a good bit of infrastructure in PHP. You can utilize microservices to keep some of the old parts while already developing new services. Your setup will then look like the following diagram:

![](assets/44b2fe6c-a1c3-495d-a002-870661e3670e.png)

In this case, your user request will be routed *either* to the (old) PHP monolithic Media & File Management service or to your new Vapor services. Your old monolith is actually turned into a microservice itself, though it most likely covers more functionality than you want it to have. But deciding to go with microservices allows you to incorporate software that you may depend on even if it is written in an entirely different language.

If you are wondering how the preceding example would look like in real life, most microservices are operating out of *containers* (for example, `Docker` containers). These containers operate as mini-servers and are completely isolated. So, even with just one actual hardware server, you can implement the preceding scenario with a local load balancer such as `nginx`.

Hence, microservices allow you to integrate older parts of your infrastructure while already using your newer parts.

# Understanding Why Not to Choose Microservices

You have seen several reasons to choose microservices. Now, let's look at reasons why not to choose microservices. As with everything, the strengths of microservices are also their weakness. While this book is about writing microservices, you should not think that microservices are the answer to every problem. There are many examples of why microservices are actually the wrong choice for certain problems. Here is a list of some of the reasons why not to choose microservices.

## Initially More Work

The first and most obvious reason why you would not want to choose microservices is that it is usually a bit more work initially. To set up the entire system and connect the services and get them all up and running the way you need to often requires more work than with a monolithic application.

It starts by setting everything up and building a working environment. A monolithic application will require you to run the application only. So, once you start your main application, you are good to go.

A microservices application, however, requires a setup in which multiple applications are running at the same time. Alternatively, you could design your application to receive test data from static files; this, however, does not replace testing with the actual services. You can archive such a setup easily in a deployment situation; cloud providers are doing an excellent job in helping us here. However, when you start developing, it can be a good amount of work to set everything up. In the next chapter, you will set up your own workspace.

Additionally, microservices demand planning. You cannot perform some changes as quickly as you would in a monolithic application. If you are only building a simple proof-of-concept for your backend, you might be better off starting as a monolith. If you need something implemented that is critical in timing, microservices might not be your best option.

So, a good thing to keep in mind when working with microservices is that microservices demand some planning and often take longer to start.

## Increased Complexity

When developing monolithic applications, you usually have everything in the same code base. As long as a project is small, the lack of complexity makes it easier for you to maintain it. Additional features and changes can be implemented with the same stack, language, and frameworks you have been using so far.

Microservices, on the other hand, can be a bit more complex (or messy even). Adding a new feature is not always trivial and may require changes in numerous services. The services themselves may also have different stacks. You may have a legacy service running PHP combined with a Swift Vapor service for another part of the app. Adding a feature requires you to work in PHP as well as Swift.

If you know your exact scope for a project and don't expect any changes, there is no reason not to implement it like a monolithic application.
