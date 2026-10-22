# Chapter 9. Microservices

# A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 9th chapter of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at vwilson@oreilly.com.

You don’t often see strong links between a mainstream software architectural style and an Italian-inspired, globally popular cuisine. This is, however, the case with microservices and pizza. The roots of microservices can be traced back to around 2008 when the approach was pioneered at scale by the Internet giants we all know. At Amazon, the ‘two pizza rule’ emerged as a governing principle of team size for a single system component, which subsequently became known as a microservice.¹ What is the two-pizza rule? Very simply, every internal team should be small enough that it can be fed with two pizzas.

It is a misconception however that microservices are in some sense ‘smaller than a service’. The defining characteristic of a microservice is their scope, organized around a business capability.² Put very simply, microservices are an approach to designing and deploying fine-grained, highly cohesive, and loosely coupled services that are composed to fulfill the system’s requirements. These fine-grained services, or microservices, are independently deployed and must communicate and coordinate when