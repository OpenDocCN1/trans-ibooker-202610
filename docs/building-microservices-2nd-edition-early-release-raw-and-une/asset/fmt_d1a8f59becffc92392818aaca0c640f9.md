
The key thing though is that from environment to environment the exact topology of your microservice will change. You need to find ways therefore to change the number of instances from one environment to another, along with any environment-specific configuration. We also want to build our service instances once and once only, so it follows that any environment-specific information needs to be separate from the deployed service artifact.

How you go about varying the topology of your microservice from one environment to another will depend greatly on the mechanism you use for deployment, and also how much the topologies vary. If the only thing that changes from one environment to another is the number of microservice instances, this might be as simple as parameterising this value to allow for different numbers to be passed in as part of the deployment activity. We’ll touch on the topic of Kubernetes later, a look at some options for how environment-specific changes can be made.

So, to summarize, a single logical microservice can be deployed into multiple environments. From one environment to the next the number of instances of each microservice can vary based on the requirements of each environment.

# Principles Of Microservice Deployment

With so many options facing you for how to deploy your microservices, I think it’s important that I establish some core principles in this area. A solid understanding of these principles will stand you in good stead no matter what choices you end up making. We’ll look at each principle in detail shortly, but just to get us started, here are the core ideas we’ll be covering.

Isolated Execution:
Run microservice instances in an isolated fashion where they have their own computing resources, and their execution cannot impact other microservice instances running nearby.

Focus On Automation:
As the number of microservices increases, automation becomes increasingly important. Focus on choosing technology which allows for a high degree of automation, and adopt automation as a core part of your culture.

Infrastructure As Code:
Represent the configuration for your infrastructure to ease automation and promote information sharing. Store this code in source control to allow for environments to be recreated.

Zero-downtime Deployment:
Take independent deployability further, and ensure that deploying a new version of a microservice can be done without any downtime to users of your service (be it humans or other microservices).

Desired State Management:
Use a platform that maintains your microservice in a defined state, launching new instances if required in the event of outage or traffic increases. Consider GitOps to use this in conjunction with Infrastructure As Code to drive even more of your operations tasks from code.

## Isolated Execution

You may be tempted, especially early on in your microservices journey, to just put all of your microservice instances on a single machine (which could be a single physical machine, or single VM), as shown in [Figure 7-10](#ch07.xhtml#a61-multi-services-per-host). Purely from a host management point of view, it is simpler. In a world where one team manages the infrastructure and another team manages the software, the infrastructure team’s workload is often a function of the number of hosts it has to manage. If more services are packed on to a single host, the host management workload doesn’t increase as the number of services increases.

![](Images/bdms_0606.png)

There are some challenges with this model, though. First, it can make monitoring more difficult. For example, when tracking CPU, do I need to track the CPU of one service independent of the others? Or do I care about the CPU of the host as a whole? Side effects can also be hard to avoid. If one service is under significant load, it can end up reducing the resources available to other parts of the system. This was an issue that [Gilt](http://bit.ly/1z1WR3T), an online fashion retailer, encountered. Starting with a Ruby on Rails monolith, they decided to move to microservices to make it easier to scale the application and also better accomodate a growing number of developers. Initially Gilt coexisted many microservices on a single box, but uneven load on one of the microservices would have an adverse impact on everything else running on that host. This makes impact analysis of host failures more complex as well—taking a single host out of commission can have a large ripple effect.

Deployment of services can be somewhat more complex too, as ensuring one deployment doesn’t affect another leads to additional headaches. For example, if each microservice has different (and potentially contradictory) dependencies, how can I make that work?

This model can also inhibit autonomy of teams. If services for different teams are installed on the same host, who gets to configure the host for their services? In all likelihood, this ends up getting handled by a centralized team, meaning it takes more coordination to get services deployed.

Fundamentally, running lots of microservice instances on the same machine (virtual or physical) ends up drastically undermining one of the key principles of microservices as a whole - independent deployability. It follows therefore, that we really want to run microservice instances in isolation, as we see in [Figure 7-11](#ch07.xhtml#a61-single-service-per-host).

![](Images/bdms_0608.png)

Each microservice instance gets its own, isolated, execution environment. It can install its own dependencies, have its own set of ring-fenced resources.

As my old colleague Neal Ford puts it, many of our working practices around deployment and host management are an attempt to optimize for scarcity of resources. In the past, the only option if we wanted another machine to achieve isolation was to buy or rent another physical machine. This often had a large lead time to it and results in a long-term financial commitment. In my experience, it’s not uncommon for clients to provision new servers only every two to three years, and trying to get additional machines outside of these timelines is difficult. But on-demand computing platforms have drastically reduced the costs of computing resources, and improvements in virtualization technology mean there is more flexibility, even for in-house hosted infrastructure.

With containerization joining the mix, we have more options than ever before for provisioning an isolated execution environment. As [Figure 7-12](#ch07.xhtml#isolation-tradeoffs) shows, broadly speaking, we go from the extreme of having dedicated physical machines for your services, which gives you the best isolation but probably the highest cost, to containers at the other end which gives you weaker isolation but tends to be more cost effective and much faster to provision. We’ll come back to some of the specifics around technology like containerization later in this chapter.

![](Images/isolation-tradeoffs.png)

If you were deploying your service on to a PAAS-like tool, such as AWS Lambda, Heroku or similar, this isolation is provided for you. Depending on the nature of the platform itself, you could likely expect your microservice instance to end up running inside a container or dedicated VM behind the scenes.

In general, the isolation around containers has improved sufficiently making them a more natural choice for microservice workloads. The difference in isolation between containers and VMs has reduced to the point where for the vast majority of workloads it is “good enough”, which is in large part why they are such a popular choice, and tend to be my default choice in most situations.

## Focus On Automation
