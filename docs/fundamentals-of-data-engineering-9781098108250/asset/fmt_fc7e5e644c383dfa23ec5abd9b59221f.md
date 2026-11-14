
## Principle 2: Design for Failure

The acceptable state after recovery. In data systems, data is often lost during an outage. In this setting, the recovery point objective (`RPO`) refers to the maximum acceptable data loss.

Engineers need to consider acceptable reliability, availability, `RTO`, and `RPO` in designing for failure. These will guide their architecture decisions as they assess possible failure scenarios.

## Principle 3: Architect for Scalability

Scalability in data systems encompasses two main capabilities. First, scalable systems can *scale up* to handle significant quantities of data. We might need to spin up a large cluster to train a model on a petabyte of customer data or scale out a streaming ingestion system to handle a transient load spike. Our ability to scale up allows us to handle extreme loads temporarily. Second, scalable systems can *scale down*. Once the load spike ebbs, we should automatically remove capacity to cut costs. (This is related to principle 9.) An *elastic system* can scale dynamically in response to load, ideally in an automated fashion.

Some scalable systems can also *scale to zero*: they shut down completely when not in use. Once the large model-training job completes, we can delete the cluster. Many serverless systems (e.g., serverless functions and serverless online analytical processing, or `OLAP`, databases) can automatically scale to zero.

Note that deploying inappropriate scaling strategies can result in overcomplicated systems and high costs. A straightforward relational database with one failover node may be appropriate for an application instead of a complex cluster arrangement. Measure your current load, approximate load spikes, and estimate load over the next several years to determine if your database architecture is appropriate. If your startup grows much faster than anticipated, this growth should also lead to more available resources to rearchitect for scalability.

## Principle 4: Architecture Is Leadership

Data architects are responsible for technology decisions and architecture descriptions and disseminating these choices through effective leadership and training. Data architects should be highly technically competent but delegate most individual contributor work to others. Strong leadership skills combined with high technical competence are rare and extremely valuable. The best data architects take this duality seriously.

Note that leadership does not imply a command-and-control approach to technology. It was not uncommon in the past for architects to choose one proprietary database technology and force every team to house their data there. We oppose this approach because it can significantly hinder current data projects. Cloud environments allow architects to balance common component choices with flexibility that enables innovation within projects.

Returning to the notion of technical leadership, `Martin Fowler` describes a specific archetype of an ideal software architect, well embodied in his colleague `Dave Rice`:^([9](#ch03.html#ch01fn21))

> In many ways, the most important activity of *Architectus Oryzus* is to mentor the development team, to raise their level so they can take on more complex issues. Improving the development team's ability gives an architect much greater leverage than being the sole decision-maker and thus running the risk of being an architectural bottleneck.

An ideal data architect manifests similar characteristics. They possess the technical skills of a data engineer but no longer practice data engineering day to day; they mentor current data engineers, make careful technology choices in consultation with their organization, and disseminate expertise through training and leadership. They train engineers in best practices and bring the company's engineering resources together to pursue common goals in both technology and business.

As a data engineer, you should practice architecture leadership and seek mentorship from architects. Eventually, you may well occupy the architect role yourself.

## Principle 5: Always Be Architecting

We borrow this principle directly from `Google Cloud's Five Principles for Cloud-Native Architecture`. Data architects don't serve in their role simply to maintain the existing state; instead, they constantly design new and exciting things in response to changes in business and technology. Per the [`EABOK`](https://oreil.ly/i58Az), an architect's job is to develop deep knowledge of the *baseline architecture* (current state), develop a *target architecture*, and map out a *sequencing plan* to determine priorities and the order of architecture changes.

We add that modern architecture should not be command-and-control or waterfall but collaborative and agile. The data architect maintains a `target architecture` and sequencing plans that change over time. The `target architecture` becomes a moving target, adjusted in response to business and technology changes internally and worldwide. The `sequencing plan` determines immediate priorities for delivery.

## Principle 6: Build Loosely Coupled Systems

> When the architecture of the system is designed to enable teams to test, deploy, and change systems without dependencies on other teams, teams require little communication to get work done. In other words, both the architecture and the teams are loosely coupled.
>
> 
> `Google DevOps tech architecture guide`^([10](#ch03.html#idm45075141844176))

In 2002, `Bezos` wrote an email to `Amazon` employees that became known as the `Bezos API Mandate`:^([11](#ch03.html#ch01fn22))

1.  All teams will henceforth expose their data and functionality through service interfaces.

2.  Teams must communicate with each other through these interfaces.

3.  There will be no other form of interprocess communication allowed: no direct linking, no direct reads of another team's data store, no shared-memory model, no back-doors whatsoever. The only communication allowed is via service interface calls over the network.

4.  It doesn't matter what technology they use. `HTTP`, `Corba`, `Pubsub`, custom protocols—doesn't matter.

5.  All service interfaces, without exception, must be designed from the ground up to be externalizable. That is to say, the team must plan and design to be able to expose the interface to developers in the outside world. No exceptions.

The advent of `Bezos's API Mandate` is widely viewed as a watershed moment for `Amazon`. Putting data and services behind `APIs` enabled the loose coupling and eventually resulted in `AWS` as we know it now. `Google's` pursuit of loose coupling allowed it to grow its systems to an extraordinary scale.

For software architecture, a loosely coupled system has the following properties:

1.  Systems are broken into many small components.

2.  These systems interface with other services through abstraction layers, such as a messaging bus or an `API`. These abstraction layers hide and protect internal details of the service, such as a database backend or internal classes and method calls.

3.  As a consequence of property 2, internal changes to a system component don't require changes in other parts. Details of code updates are hidden behind stable `APIs`. Each piece can evolve and improve separately.

4.  As a consequence of property 3, there is no waterfall, global release cycle for the whole system. Instead, each component is updated separately as changes and improvements are made.

Notice that we are talking about *technical systems*. We need to think bigger. Let's translate these technical characteristics into organizational characteristics:

1.  Many small teams engineer a large, complex system. Each team is tasked with engineering, maintaining, and improving some system components.
