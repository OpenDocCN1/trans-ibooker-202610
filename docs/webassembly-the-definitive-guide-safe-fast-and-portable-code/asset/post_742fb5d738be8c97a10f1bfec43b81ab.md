
There is no single use case that makes WebAssembly make sense. We have talked about reusing existing libraries and writing low-level code in portable ways using Rust or C for performance reasons.

If you have mostly been a JavaScript developer all of your professional life, that is going to be a tall order to suddenly learn a language like that. AssemblyScript brings a slightly strange but still much more familiar language that you largely already know. It allows you to compile it down to potentially higher-performance code. That is a very nice additional use case indeed.

- 1 Also known as old.
- 2 Seriously, this is a classic album.
- 3 Transpilation is the process of translating one source codebase into another codebase as opposed to a binary executable (compilation). You can read more about it here: https://en.wikipedia.org/wiki/Source-to-source_compiler
- 4 There are several great tutorials and walkthroughs to learn TypeScript through their website: https://www.typescriptlang.org
- 5 This example and others were inspired by the website https://wasmbyexample.dev/home.en-us.html by Aaron Turner (@torch2424) who is a talented, generous developer and a badass DJ too. Check him out.
- 6 This proposal requires the completion of several other proposals first as indicated in Chapter 12 but you can read more about it here: https://github.com/WebAssembly/gc/blob/master/proposals/gc/Overview.md.
- 7 You can find more details about what is provided with the AssemblyScript Garbage Collector support here: https://www.assemblyscript.org/garbage-collection.html#runtime-interface
- 8 You can find more details about the AssemblyScript Standard Library here: https://www.assemblyscript.org/stdlib/globals.html
- 9 While it is commonly believed that π are squared, it isn’t true. π are round. Cornbread are squared.

## Chapter 15. Applied WebAssembly: In the Cloud and on the Edge

### A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 15th chapter of the final book.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the author at wasmbook@bosatsu.net.

> I’ve looked at clouds from both sides now
> From up and down and still somehow
> It’s cloud illusions I recall
> I really don’t know clouds at all
> 
> —Joni Mitchell

Given the popularity and ubiquity of cloud computing initiatives these days, it may seem odd to start this chapter with a quotation about not understanding clouds. If we are being honest, I think that it is fair to say that a lot of people do not understand the full scope of using cloud computing. There is the obvious speed improvement and ease of spinning up instances in contrast to the often untenable scenario of procuring physical hardware systems. But everything in the cloud is just as silly of a strategy as nothing in the cloud.

The real driver of your thinking about cloud strategy should not just be about procurement and developer productivity even though those are important considerations. A more general idea is to consider the cost of computation. This is a more complex calculation than it may seem though both in terms of what I mean by “cost” and “computation”. In this chapter, I am going to focus on how where we run our computation is changing rapidly and why WebAssembly is going to be a key part of that.

## A Short, Personal Detour

Earlier in my career, I was one of the first employees at a company called Parabon¹. We built an Internet distributed computing platform to aggregate idle time on computers all over the world. Keep in mind this was pre-cloud. In addition to porting our engine with its Java-based core for safety to eight platforms, we also built the client APIs, the server infrastructure for scheduling the work and a half-dozen or more vertical applications to use it. The kind of jobs we ran included gene sequence comparisons that scaled linearly, genetic algorithm-based feature selection for exhaustive regression searches, and a Monte Carlo-based rendering system.

The whole point of this initiative was to find arbitrage opportunities in computational capacity. Organizations did not buy enough computing power for their peak needs and therefore there were ceilings on what they could tackle computationally. We gave them an elastic capability for one class of problem known as “embarrassingly parallel” problems². Customers could ask for as many or few computational resources as they needed to solve a particular problem. This gave them a knob to turn to incur marginal costs based on their needs.

When doing exhaustive searches, this allowed them to include more features than they normally had the resources to analyze. This was helpful in one case to a pharmaceutical company to find why a certain therapy only worked for some patients and not others. Another example of a good match was a NASA consultant who was able to speed up his simulations from taking nine months on equipment he had access to under normal circumstances to four to five hours on our platform. In order to achieve the portability across the various operating systems and the protection we promised to our computation providers, he had to port his code from Fortran to Java. While Fortran could run circles around the upstart language from Sun at the time in a direct competition, we would make it up with parallelization.

Even with the limitation of the embarrassingly parallel problem constraint, given where things were with broadband roll out at the time, data-intensive tasks such as the digital rendering were not good fits for our platform. The compute to data ratio was too low. It cost too much in terms of time and bandwidth to get the data where it needed to be. What we were fundamentally offering was a form of computational intermediation. Even at the time, I felt like more was needed to really provide the most value, but cloud computing had not emerged yet to give us a fuller vision of resource virtualization.

Another work experience that drove my thinking was my time in the video game industry. I worked on a game that was based on another game that had 40 million users so we were pretty sure we were going to need to scale up. Putting everything into the cloud would have been cost prohibitive, however, so we ran a mix of data center equipment that we purchased and set up and then used cloud services as an elastic spill over. Again, we were able to use the extra resources only when we needed them and did not have to pay for them to be running all of the time.

## Our Industry Evolves

With that background in place, when I talk about the “cost” of computation, I mean the time cost, the money cost, the opportunity cost of not having elastic supply, the power consumption cost and the latency cost. These are all costs that we are trying to minimize these days. Cloud computing is one part of the strategy, but so are architectural designs such as microservices and serverless functions. Apple’s M1 designs are another. There are two CPUs in the M1 chip, one that is more performance-oriented and one that is more power consumption-oriented. Researchers are analyzing sophisticated approaches for using machine-learning strategies to order LLVM optimizations to either maximize performance or minimize power consumption. The application code does not change, just its compiled form.

Where and how we engage computational elements to solve problems has become one of the most important aspects of a successful IT strategy. This includes advanced and custom hardware such as vectorizing CPU instructions³, graphics processing units (GPUs), Field Programmable Gate Arrays (FPGA)⁴, Application Specific Integrated Circuits (ASIC)⁵ and Tensor Processing Units (TPU)⁶. Each of these types of hardware has different capabilities, instructions, costs, and performance characteristics. Mapping workloads to these elements is going to be a big part of getting computation done in ways that minimize a range of costs.

Physical location is another element of cost, because latency is, as you know⁷, something we cannot discount. If you are familiar with the concept of cloud regions or the use of a content delivery network (CDN) to serve up static resources, you will understand the benefit of getting things closer to where your users are. Domain Name Service (DNS) trickery allows an endpoint to be selected dynamically based upon the geographic region a user is coming from.

Edge computing is an emerging service offering that takes these ideas a step further to mix together the ideas of distributed computing (a la Parabon), cloud hosting, and geographically distributed resources. The difference is that we are no longer simply talking about static resources, but executable code. The web architecture allows us to push things all the way into the browser. As you have seen, WebAssembly has a prominent role in this use case. Sometimes you would like to go in browser or on device for privacy reasons, low-latency user experiences, and more. At other times, you may not want to run proprietary code on the user’s device, but will still want to get close. This is the realm of Edge Computing. It is a continuum of deployment localities that spans centralized data centers, regional boundaries, service provider access point boundaries, on premise systems,
