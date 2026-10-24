+   - If one microservice authenticates a user, can other microservices trust that user?
+   - Are all microservices allowed to call each other?

The answers vary from system to system. The first part of this chapter discusses how to address these questions. The second part of the chapter outlines an implementation of one possible answer to the first question and shows a full implementation of an answer to the second question.

# 10.1 Microservice security concerns

Security is an important concern for almost any server-side system. It’s also a very broad topic, much of which is outside the scope of this book. We’ll concentrate on two areas of security that are relevant to developers of microservice systems:

+   1. Authentication and authorization
+   2. How to limit communication between microservices

Most systems have some functionality that’s only accessible to logged-in users. Think about the point-of-sale system discussed in earlier chapters. In chapters 4 and 5, we talked about adding a loyalty program that allows registered users to receive special offers via email, based on their interests. If users are interested in golf, they’ll be notified about good deals on golf balls. If users want to edit their interests—they may have given up golf and taken up quilting, instead—they need to be logged in. Otherwise, one user could edit another user’s interests, resulting in their being notified about the wrong offers. Making sure a user really is who they claim to be is a matter of authentication. Deciding what the user is allowed to do—for instance, that they can edit their own interests but not anyone else’s—is a matter of authorization.

Authentication and authorization are concerns that your systems will probably have regardless of whether you build them with microservices. The difference is that the granular nature of a microservice system begs the question: which microservices handle authentication and authorization? The following two sections address that question.

# Data in motion vs. data at rest

Systems handle data. That data is most often essential to the systems, but it’s also often essential to users. The data may be sensitive, such as users’ home addresses, credit card numbers, or medical records. Even if the data isn’t sensitive, it can still be valuable to the system—the product catalog of an e-commerce site isn’t sensitive, but it’s worth a lot to the business behind the site. The data your systems handle is important and needs to be handled safely. Broadly speaking, we can place data handling in one of two categories:

+   - Data in motion—When data is moved from one part of a system to another, it’s said to be in motion. For instance, when collaborating microservices exchange data via commands, queries, or events, the data they exchange is in motion.