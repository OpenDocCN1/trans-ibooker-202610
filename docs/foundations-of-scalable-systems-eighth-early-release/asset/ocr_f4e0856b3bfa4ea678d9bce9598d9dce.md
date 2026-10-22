interact with the service through its published API, which is accessible across the Internet.

Let’s look at the API and service implementation in more detail.

![](bbox=[0.28, 0.18, 0.6, 0.48])

Figure 5-1. A Simple Service

# Application Programming Interface (API)

An API defines a contract between the client and server. The API specifies the types of requests that are possible, the data that is needed to accompany the requests, and the results that will be obtained. APIs have many different variations, as I explained in RPC/RMI discussions in Chapter 3. While there remains some API diversity in modern applications, the predominant style relies on HTTP APIs. These are typically, although not particularly accurately, classified as RESTful.

REST is actually an architectural style that was defined by Roy Fielding in his PhD thesis¹. A great source of knowledge on RESTful APIs and the various degrees to which Web technologies can be exploited is REST in Practice by Jim Webber, et al. (O’Reilly). Here I’ll just briefly touch on the HTTP CRUD API pattern. This pattern does not fully implement the principles of REST, but it is widely adopted in Internet systems today. It