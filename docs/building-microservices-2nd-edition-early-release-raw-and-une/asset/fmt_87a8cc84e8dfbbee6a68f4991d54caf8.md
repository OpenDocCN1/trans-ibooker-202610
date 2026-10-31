
^([4](#ch02.xhtml#idm46534877695976-marker)) In my book, Monolith To Microservices, I attributed this to Larry Constantine himself. While the statement neatly sums up much of Constantine’s work in this space, the quote should really be attributed to Albert Endres and Dieter Rombach from their book “A Handbook of Software and Systems Engineering”.

^([5](#ch02.xhtml#idm46534877685576-marker)) Constantine, Larry and Edward Yourdon, Structured Design, Yourdon Press

^([6](#ch02.xhtml#idm46534877684712-marker)) Page-Jones, Meilir, Practical Guide to Structured Systems Design (Yourdon Press Computing)

^([7](#ch02.xhtml#idm46534878148648-marker)) This concept is similar to the Domain Application Protocol which defines the rules by which components interact in a REST-based system.

^([8](#ch02.xhtml#idm46534878125080-marker)) Pass through coupling is my name for what was originally described as Tramp coupling by M. Page-Jones: The Practical Guide to Structured Systems Design. I chose to use a different term here due to the fact that I found the original term somewhat problematic, and not terribly meaningful to a wider audience

^([9](#ch02.xhtml#idm46534877682408-marker)) OK, more than once or twice. A **lot** more than once or twice…

^([10](#ch02.xhtml#idm46534877600232-marker)) Eric Evans, *Domain-Driven Design: Tackling Complexity in the Heart of Software* (Boston: Addison-Wesley, 2004).

^([11](#ch02.xhtml#idm46534877578344-marker)) I know some people object to the use of templated URIs in REST systems, and I understand why - I just want to keep things simple for this example

^([12](#ch02.xhtml#idm46534877917224-marker)) I mean no disrespect if this is you - I’ve done this myself more than once

^([13](#ch02.xhtml#idm46534877908952-marker)) I mean, why not yellow? It’s the most common colour!

^([14](#ch02.xhtml#idm46534877904104-marker)) Event Storming, Alberto Brandolini, Leanpub (work in progress) [*https://leanpub.com/introducing\_eventstorming*](https://leanpub.com/introducing_eventstorming)

# Chapter 3. Microservice Communication Styles
Getting communication between microservices right is problematic for many, in great part due to the fact that I feel that people gravitate towards a chosen technological approach without first considering the different types of communication you might want. In this chapter, I’ll try and tease apart the different styles of communication, to help you understand the pros and cons of each, and also help you understand which approach will best fit your problem space.
We’ll be looking at synchronous blocking and asynchronous non-blocking communication mechanisms, as well as comparing request-response collaboration with event-driven collaboration.
By the end of this chapter you should be much better prepared to understand the different options available to you, and will have a foundational knowledge that will help when we start looking at more detailed implementation concerns in the following chapters.

## From In-Process To Inter-Process
OK, let’s get the easy stuff out of the way first - or at least what I *hope* is the easy stuff. Namely, calls *between* different processes across a network (inter-process) are **very** different to calls *within* a single process (in-process). At one level, we can ignore this distinction. It’s easy, for example, to think of one object making a method call on another object, then just map this interaction to two microservices communicating via a network. Putting aside the fact that microservices aren’t just objects, this thinking can get us into a lot of trouble.
Let’s look at some of these differences now, and how they might change how you think about the interactions between your microservices.

### Performance
The performance of an in-process call and an inter-process call is fundamentally different. When I make an in-process call, the underlying compiler and runtime can carry out a whole host of optimizations to reduce the impact of the call, including inlining the invocation so it’s as though there was never a call in the first place. No such optimizations are possible with inter-process calls. Packets have to be sent. Expect the overhead of an inter-process call to be significant compared to the overhead of an in-process call. The former is very measurable - just round-tripping a single packet in a data centre is measured in milliseconds - whereas the overhead of making a method call is something you don’t need to worry about.
This can often lead you to want to rethink APIs. An API that makes sense in-process may not make sense in inter-process situations. I can make 1000 calls across an API boundary in-process without concern. Do I want to make 1000 network calls between two microservices? Perhaps not.
When I pass a parameter into a method, the data structure I pass in typically doesn’t move - what’s more likely is that I pass around a pointer to a memory location. Passing in an object or data structure to another method doesn’t necessitate more memory to be allocated in order to copy the data.
When making calls between microservices over a network on the other hand, the data actually has to be serialized into some form that can be transmitted over a network. The data then needs to be sent, and deserialized at the other end. We therefore may need to be more mindful about the size of payloads being sent between processes. When was the last time you were aware of how big a data structure was that you were passing around inside a process? The reality is that you likely didn’t need to know - now, you do. This might lead you to reduce the amount of data being sent or received (perhaps not a bad thing if we think about information hiding), pick more efficient serialization mechanisms, or even offload data to a file system and pass around pointers to that data instead.
These differences may not cause you issues straight away, but you certainly need to be aware of them. I’ve seen a lot of attempts to hide from the developer the fact that a network call is even taking place. Our desire to create abstractions to hide detail is a big part of what allows us to do more things more efficiently, but sometimes we create abstractions that hide too much. A developer needs to be aware if they are doing something that will result in a network call, otherwise do not be surprised if you end up with some nasty performance bottlenecks further down the line.

### Changing Interfaces
When we consider changes to an interface inside a process, the act of rolling out the change is straightforward. Both the code implementing the interface, and the code calling the interface, are all packaged together in the same process. In fact if I change a method signature using an IDE with refactoring capability, often the IDE itself will automatically refactor calls to this changing method. Rolling out such a change can be done in an atomic fashion - both sides of the interface are packaged together in a single process.
With communication between microservices, however, the microservice exposing an interface, and the consuming microservices using that interface, are separately deployable microservices. When making a backwards incompatible change to a microservice interface, we either need to do a lock-step deployment with consumers, making sure they are updated to use the new interface, or else find some way to phase the rollout of the new microservice contract. We’ll explore this concept in more detail later in this chapter.

### Error handling
Within a process, if I call a method, the nature of the errors tends to be pretty straightforward. Simplistically, the errors are either expected and easy to handle, or else they are catastrophic to the point where we just propagate the error up the call stack. Errors, on the whole, are deterministic.
