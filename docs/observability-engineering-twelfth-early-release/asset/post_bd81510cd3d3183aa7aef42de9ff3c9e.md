
However, when that same concept is adapted for use with squishier virtual software systems, it opens up a radically different way of interacting with and understanding the code you write.

## Applying Observability to Software Systems

Kálmán’s definition of observability can be applied to modern software systems. When adapting the concept of observability to software, we must also layer additional considerations that are specific to the software engineering domain.

For a software application to have observability, you must be able to do the following:

- Understand the inner workings of your application
- Understand any system state your application may have gotten itself into, even new ones you have never seen before and couldn’t have predicted
- Understand the inner workings and system state solely by observing and interrogating with external tools
- Understand the internal state *without* shipping any new custom code to handle it (because that implies you needed prior knowledge to explain it)

A good litmus test for determining whether those conditions are true is to ask yourself the following questions:

- Can you continually answer open-ended questions about the inner workings of your applications to explain any anomalies, without hitting investigative dead ends (i.e., the issue might be in a certain group of things, but you can’t break it down any further to confirm)?
- Can you understand what any particular user of your software may be experiencing at any given time?
- Can you quickly see any cross-section of system performance you care about, from top-level aggregate views, down to the single and exact user requests that may be contributing to any slowness (and anywhere in between)?
- Can you compare any arbitrary groups of user requests in ways that let you correctly identify which attributes are commonly shared by all users who are experiencing unexpected behavior in your application?
- Once you do find suspicious attributes within one individual user request, can you search across all user requests to identify similar behavioral patterns to confirm or rule out your suspicions?
- Can you identify which system user is generating the most load (and therefore slowing application performance the most), as well as the 2nd, 3rd, or 100th most load-generating users?
- Can you identify which of those most-load-generating users only recently started impacting performance?
- If the 142nd slowest user complained about performance speed, can you isolate their requests to understand why exactly things are slow for that specific user?
- If users complain about timeouts happening, but your graphs show that the 99th, 99.9th, even 99.99th percentile requests are fast, can you find the hidden timeouts?
- Can you answer questions like the preceding ones without first needing to predict that you might need to ask them someday (and therefore set up specific monitors in advance to aggregate the necessary data)?
- Can you answer questions like these about your applications even if you have never seen or debugged this particular issue before?
- Can you get answers to questions like the preceding ones quickly, so that you can iteratively ask a new question, and another, and another, until you get to the correct source of issues, without losing your train of thought (which typically means getting answers within seconds instead of minutes)?
- Can you answer questions like the preceding ones even if that particular issue has never happened before?
- Do the results of your debugging investigations often surprise you by revealing new, perplexing, and bizarre findings, or do you generally find only the issues you suspected that you might find?
- Can you quickly (within minutes) isolate any fault in your system, no matter how complex, deeply buried, or hidden within your stack?

Meeting all of the preceding criteria is a high bar for many software engineering organizations to clear. If you can clear that bar, you, no doubt, understand why observability has become such a popular topic for software engineering teams.

Put simply, our definition of “observability” for software systems is a measure of how well you can understand and explain any state your system can get into, no matter how novel or bizarre. You must be able to comparatively debug that bizarre or novel state across all dimensions of system state data, and combinations of dimensions, in an ad hoc iterative investigation, without being required to define or predict those debugging needs in advance. If you can understand any bizarre or novel state *without needing to ship new code*, you have observability.

We believe that adapting the traditional concept of observability for software systems in this way is a unique approach with additional nuances worth exploring. For modern software systems, observability is not about the data types or inputs, nor is it about mathematical equations. It is about how people interact with and try to understand their complex systems. Therefore, observability requires recognizing the interaction between both people and technology to understand how those complex systems work together.

If you accept that definition, many additional questions emerge that demand answers:

- How does one gather that data and assemble it for inspection?
- What are the technical requirements for processing that data?
- What team capabilities are necessary to benefit from that data?

We will get to these questions and more throughout the course of this book. For now, let’s put some additional context behind observability as it applies to software.

The application of observability to software systems has much in common with its control theory roots. However, it is far less mathematical and much more practical. In part, that’s because software engineering is a much younger and more rapidly evolving discipline than its more mature mechanical engineering predecessor. Production software systems are much less subject to formal proofs. That lack of rigor is, in part, a betrayal from the scars we, as an industry, have earned through operating the software code we write in production.

As engineers attempting to understand how to bridge the gap between theoretical practices encoded in clinical tests and the impacts of what happens when our code runs at scale, we did not go looking for a new term, definition, or functionality to describe how we got there. It was the circumstances of managing our systems and teams that led us to evolving our practices away from concepts, like monitoring, that simply no longer worked. As an industry, we need to move beyond the current gaps in tooling and terminology to get past the pain and suffering inflicted upon us by outages and a lack of more proactive solutions.

Observability is the solution to that gap. Our complex production software systems are a mess for a variety of both technical and social reasons. So it will take both social and technical solutions to dig us out of this hole. Observability alone is not the entire solution to all of our software engineering problems. But it does help you clearly see what’s happening in all the obscure corners of your software, where you are otherwise typically stumbling around in the dark and trying to understand things.

Imagine you wake up one Saturday morning with grand plans to make brunch for the entire extended family. You have multiple dishes in mind, including a complex recipe for cheese souffle, a list of everyone’s known allergens and food sensitivities, and a tight timeline—Grandma has to make it to the airport by noon. This is a nontrivial challenge on its own. Now imagine you can’t find your glasses (and that you’re as near-sighted as we are). When it comes to solving practical and time-sensitive problems in software engineering, observability is a darn good place to start.

## Mischaracterizations About Observability for Software

Before proceeding, we need to address another definition of “observability,” the definition being popularly promoted by software-as-a-service (SaaS) developer tool vendors. These vendors insist that “observability” has no special meaning whatsoever—that it is simply another synonym for “telemetry,” indistinguishable from “monitoring.” Proponents of this definition relegate observability to being another generic term for understanding how software operates. You will hear this contingent explain away observability as “three pillars” of things they can sell you that they already do today: metrics, logs, and traces.²

It is hard to decide which is worse about this definition: its redundancy (why exactly do we need another synonym for “telemetry”?) or its epistemic confusion (why assemble a list of one data type, one anti-data type slash mess of strings, and one…way of visualizing things in order by time?). Regardless, the logical flaw of this definition becomes clear when you realize its proponents have a vested interest in selling you the tools and mindsets built around the siloed collection and storage of data with their existing suite of metrics, logging, and tracing tools. The proponents of this definition let their business models constrain how they think about future possibilities.
