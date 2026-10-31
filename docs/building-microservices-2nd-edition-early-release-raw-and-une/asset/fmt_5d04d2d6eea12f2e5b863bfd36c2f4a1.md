
With this model, our microservice instance no longer maps to a single unit of deployment. Instead, our microservice is now more of a logical concept, which consists of multiple different functions which can theoretically be deployed independently from each other.

A few caveats here. Firstly, I would strongly urge you to maintain a coarser-grained external interface. To upstream consumers, they are still talking to the `Expenses` service - they are unaware that requests get mapped to smaller-scoped aggregates. This ensures that should you change your mind and want to recombine things, or even restructure the aggregate model, that you don’t impact upstream consumers.

The second issue relates to data. Should these aggregates continue to use a shared database? On this issue, I am somewhat relaxed. Assuming that the same team manages all these functions, and that conceptually it remains a single “service”, then I’d be ok with them still using the same database, as [Figure 7-20](#ch07.xhtml#multiple-functions-same-db) shows.

![](Images/multiple-functions-same-db.png)

Over time though, if the needs of each aggregate-function diverge, I’d be inclined to look to separate out their data usage, especially if you start to see coupling in the data tier impair your ability to change them easily. At this stage you could argue that these functions would now be microservices in their own right - although as I detailed above, there may be value in still representing them as a single microservice to upstream consumers.

![](Images/multiple-functions-multiple-dbs.png)

This mapping from a single microservice to multiple finer-grained deployable units warps our previous definition of a microservice somewhat. We normally consider a microservice as being an independently deployable unit - now one microservice is made up of *multiple different* independently deployable units. Conceptually, in this example, the microservice moves towards being more of a logical than a physical concept.

## Need Something Even More Fine-grained?

If you wanted to go even smaller, there is a temptation to break down your functions per aggregate into smaller pieces. I am much more cautious here. Aside from the explosion of functions this will likely create, it also violates one of the core principles of an aggregate - that we want to treat it as a single unit to ensure we can better manage the integrity of the aggregate itself.

I’ve previously entertained the idea of making each state transition of an aggregate its own function, but have backed out of this idea due to the problems associated with inconsistency. When you have different independently deployable things, each managing a different part of an overall state transition, ensuring things are done properly gets really quite difficult. It puts us into the space of Sagas which we discussed in [Chapter 3](#ch03.xhtml#integration-chapter). When implementing complex business processes, concepts like sagas are important and the work justifiable. I struggle to see the value in adding this complexity at the level of managing a single aggregate though that could be easily handled by a single function.

## The Way Forward

I remain convinced that the future for most developers is using a platform that hides much of the underlying detail from them. For many years, Heroku was the closest thing I could point to in terms of something that found the right balance, but now we have FAAS and the wider ecosystem of turnkey serverless offerings that chart a different path.

There are still issues to be ironed out with FAAS, but I feel that while the current crop of offerings still need to change to resolve the issues with them that this is the sort of platform that most developers will end up using. Not all applications will fit neatly into a FAAS ecosystem given the constraints, but for those that do people are already seeing the significant benefits. With more and more work going into Kubernetes-backed FAAS offerings, people who are unable to make direct use of the FAAS solutions provided by the main cloud providers will increasingly be able to take advantage of this new way of working.

So, while FAAS may not work for everything, it’s certainly something I urge people to explore. And for many of my clients who are looking at moving to cloud-based kubernetes solutions, I’ve been urging them to explore FAAS first as it may give them everything they need and while hiding significant complexity and offloading a lot of work.

# Which Deployment Option Is Right For You?

Yikes. So we have a lot of options, right? And I probably haven’t helped too much, by going out of my way to share loads of pros and cons for each approach. If you’ve got this far, you might be a bit bewildered regarding what you should do.

#### Tip

Well, before I go any further, I really hope that it goes without saying that if what you are currently doing works for you, then *keep doing it!* Don’t let fashion dictate your technical decisions.

If you think you do need to change how you deploy microservices, then let me try and distill down much of what we’ve already discussed and come up with some useful guidance.

Revisiting our principles of microservice deployment, one of the most important aspects we focused on was that of ensuring isolation of our microservices. Now just using that as a guiding principle, this might guide us towards using dedicated physical machines for each microservice instance! That of course would likely be very expensive, and as we’ve already discussed there are some very powerful tools that we wouldn’t be able to use if we went down this route.

Tradeoffs abound here. Balancing cost against ease of use, isolation, familiarity etc, it can become overwhelming. So let’s come back to a set of rules I like to call Sam’s Really Basic Rules Of Thumb For Working Out Where To Deploy Stuff:

1. If it ain’t broke, don’t fix it^[8](#ch07.xhtml#idm46534869612680).

2. Give up as much control as you feel happy with, then give away just a little bit more. If you can offload all your work to a good PAAS like Heroku (or FAAS platform), then do it and be happy. Do you really need to tinker with every last setting?

3. Containerising your microservices is not pain-free, but is a really good compromise around cost of isolation and has some fantastic benefits for local development, while still giving you a degree of control over what happens. Expect Kubernetes in your future.

Many people are going “Kubernetes or bust!” which I feel is unhelpful. If you’re on the public cloud, and your problem fits FAAS as a deployment model, do that instead and skip Kubernetes. Your developers will likely end up being much more productive. As we’ll discuss more in [Link to Come], don’t let the fear of lock-in keep you trapped in a mess of your own making.

Found an awesome PAAS like Heroku or Zeit, and have an application that fits the constraints of those platforms? Push all the work to the platform and spend more time working on your product. Both are pretty fantastic platforms with awesome usability from a developer point of view. Don’t your developers deserve to be happy after all?

For the rest of you, containerization is the way to go, which means we need to talk about Kubernetes.

# Kubernetes & Container Orchestration

As containers started gaining traction, many people started looking at solutions for how to manage containers across multiple machines. Docker had two attempts at this (with Docker Swarm and Docker Swarm Mode respectively), companies like Rancher and CoreOS came up with their own takes, and more general purpose platforms like Mesos were used to run containers alongside other sorts of workloads. Ultimately though, despite a lot of efforts on these products, Kubernetes has in the last couple of years come to dominate this space.

Before we speak to Kubernetes itself, we should discuss the need for a tool like it in the first place.
