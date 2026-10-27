
Next we'll add a custom span, called getOrgDbCall, to the organization service to monitor how long it takes to retrieve organization data from the Postgres database. The trace for the organization service database calls can be seen in the OrganizationService class, which you'll find in the file /organization-service/src/main/java/com/optimagrowth/organization/service/OrganizationService.java. The findById() method call contains the custom trace. The following listing shows the source code for this method.

### Listing 11.9 Implementing the findById() method

```
package com.optimagrowth.organization.service;
//Some imports omitted for conciseness
import brave.ScopedSpan;
import brave.Tracer;

@Service
public class OrganizationService {
    //Some code omitted for conciseness
    @Autowired
    Tracer tracer;

    public OrganizationService findById(String organizationId) {
        Optional<Organization> opt = null;
        ScopedSpan newSpan = tracer.startScopedSpan("getOrgDBCall");
        try {
            opt = repository.findById(organizationId);
            simpleSourceBean.publishOrganizationChange("GET", organizationId);
            if (!opt.isPresent()) {
                String message = String.format("Unable to find an organization with the organization id %s", organizationId);
                logger.error(message);
                throw new IllegalArgumentException(message);
            }
            logger.debug("Retrieving Organization Info: " + opt.get().toString());
        } finally {
            newSpan.tag("peer.service", "postgres");
            newSpan.annotate("Client received");
            newSpan.finish();
        }

        return opt.get();
    }
    //Rest of class removed for conciseness
}
```

With these two custom spans in place, restart the services. Then select the following GET endpoint:

```
http://localhost:8072/license/v1/organization/4d10ec24-141a-4980-be34-2ddb5e0458c9/license/4af05c3b-a0f3-411d-b5ff-892c62710e16
```

If we look at the transaction in Zipkin, we should see the addition of two new spans when we call the licensing service endpoint to retrieve licensing information. Figure 11.22 shows the custom spans.

![](img/3c6dc613426280ca3eafe43dea24391e_367_0.png)

Figure 11.22 The custom spans for our services show up in the transaction trace.

Figure 11.22 also shows additional tracing and timing information related to our Redis and database lookups. Drilling down, we can see that the read call to Redis took 5.503 ms. Because the call didn’t find an item in the Redis cache, the SQL call to the Postgres database took 234.888 ms.

Now that we know how to set up a distributed tracing, API gateway, discovery service, and custom spans, let’s continue with the next chapter. In the next chapter, we will explain how to deploy everything that we’ve built throughout the book.

## Summary

- Spring Cloud Sleuth allows us to seamlessly add tracing information (correlation IDs) to our microservice calls.
- Correlation IDs can be used to link log entries across multiple services. This lets us observe the behavior of a transaction across all the services involved in a single transaction.
- While correlation IDs are powerful, we need to partner this concept with a log aggregation platform that will allow us to ingest logs from multiple sources and then search and query their contents.
- We can integrate Docker containers with a log aggregation platform to capture all the application logging data.
- We integrate our Docker containers with the ELK (Elasticsearch, Logstash, Kibana) Stack. This lets us transform, store, visualize, and query the logging data from our services.
- While a unified logging platform is essential, the ability to visually trace a transaction through its microservices is also a valuable tool.
- Zipkin allows us to see the dependencies that exist between services and the flow of our transactions, and understand the performance characteristics of each microservice involved in a user’s transaction.
- Spring Cloud Sleuth integrates with Zipkin easily. Zipkin automatically captures trace data from an HTTP call, which becomes an inbound/outbound message channel used within a Spring Cloud Sleuth–enabled service.
- Spring Cloud Sleuth maps each service call to the concept of a span. Zipkin then allows us to see the performance of a span.
- Spring Cloud Sleuth and Zipkin also let us define our own custom spans so that we can understand the performance of non-Spring-based resources (a database server such as Postgres or Redis).

## Deploying your microservices

### This chapter covers

- Understanding why DevOps is critical to microservices
- Configuring the core Amazon infrastructure for O-stock services
- Manually deploying O-stock services to Amazon
- Designing a build/deployment pipeline for your services
- Treating your infrastructure as code
- Deploying your application to the cloud

We’re almost at the end of the book, but not the end of our microservices journey. While most of this book has focused on designing, building, and putting Spring-based microservices into operation using the Spring Cloud technology, we haven’t touched on how to build and deploy microservices. Creating a build/deployment pipeline might seem like a mundane task, but in reality, it’s one of the most critical pieces of our microservices architecture.

Why? Remember, one of the key advantages of a microservices architecture is that microservices are small units of code that can be quickly built, modified, and deployed to a production environment independently of one another. The small size of the service means that new features (and critical bug fixes) can be delivered with a high degree of velocity. Velocity is the keyword here because velocity implies that little to no friction exists between creating a new feature or fixing a bug and getting our service redeployed. Lead times for deployment should be minutes, not days. To accomplish this, the mechanism that you use to build and deploy your code needs to be

- *Automated*—When we build our code, there should be no human intervention in the build/deployment process. The process of building the software, provisioning a machine image, and deploying the service should be automated and initiated by the act of committing code to the source repository.
- *Repeatable*—The process that we use to build and deploy our software should be repeatable; the same thing happens every time a build and deploy kicks off. Variability in our process is often the source of subtle bugs that are difficult to track down and resolve.
- *Complete*—The outcome of our deployed artifact should be an entire virtual machine or container image (Docker, for example) that contains the “complete” run-time environment for the service. This is an important shift in the way we think about our infrastructure.
- *Immutable*—Once the machine image containing our service is built, the image’s run-time configuration should not be touched or changed after deploying the image. If changes need to be made, they need to happen in the scripts kept under source control, and the service and infrastructure must go through the build process again.

The provisioning of our machine images needs to be completely automated via scripts and kept under source control along with the service source code. In a microservice environment, this responsibility usually shifts from an operations team to the development team owning the service. One of the core tenets of microservice development is pushing the developers to complete operational responsibility for the service.

Run-time configuration changes (garbage collection settings, Spring profile used, and so forth) should be passed as environment variables to the image, while application configuration should be kept separate from the container (Spring Cloud Config).

Building a robust and generalized build deployment pipeline is a significant amount of work and is often explicitly designed toward the run-time environment. It usually involves a specialized team of DevOps (developer/operations) engineers, whose sole job is to generalize the build process so that each team can build their microservices without having to reinvent the entire build process. Unfortunately, Spring is a development framework and doesn’t offer a significant number of capabilities to implement a build/deployment pipeline. But that doesn’t mean that such a pipeline can’t be built.
