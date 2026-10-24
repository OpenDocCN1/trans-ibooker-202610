# 10.2 Implementing secure microservice-to-microservice communication

For the remainder of this chapter, we’ll look at implementing examples of the security around the loyalty program. The security requirements are as follows:

+   1. Authenticate users in the API Gateway.
+   2. Allow microservices to access the user identity.
+   3. Limit which microservices may collaborate.

The first two requirements can be fulfilled by an API Gateway. I will not go into the details of this, but only mention that you can use Azure API Management for this. The third requirement can be fulfilled using Kubernetes network policies, which we discuss with examples next.

# 10.2.1 Accessing the user identity in the loyalty program

In the loyalty program microservice, we will assume that the API Gateway is configured to perform user authentication and that the user identity is passed from the API Gateway to the loyalty program microservice as an identity token as illustrated in figure 10.5

![](bbox=[0.12, 0.45, 0.88, 0.62])

Figure 10.5 The API Gateway implemented in Azure API Management authenticates users and passes an identity token to the loyalty program microservice in Kubernetes.

In the following sections, you’ll learn how to use the identity token in the loyalty program by

+   Validating the token using ASP.NET authentication middleware
+   Authorizing in MVC controllers

The first step is to add authentication middleware. We do this in the Startup class by adding authentication to the service collection. I will only outline the solution here since the details depend on the exact API Gateway configuration, which is beyond the scope of this chapter.

In the loyalty program, we can change the ConfigureServices method in Startup to the following.