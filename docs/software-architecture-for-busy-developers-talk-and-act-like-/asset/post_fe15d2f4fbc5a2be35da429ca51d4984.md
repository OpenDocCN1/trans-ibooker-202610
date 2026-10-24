
- Getting started with quality-attribute scenarios
- Practical use case
- ATAM and agile at scale

## Introducing ATAM

Design choices are all about trade-offs. In many enterprises and for many projects, we usually aim to design and develop top-notch software but may end up with unexpected outcomes. These deviations from initial expectations could be due to shortcuts we were forced to take, budget restrictions, a permanent scope change, a lack of proper analysis, a lack of a well-thought-through architecture, and so on. All these reasons may lead to some design choices that in turn lead to trade-offs. Without a formal way of identifying these trade-offs, organizations lack the ability to make informed decisions or even to conduct **root cause analysis (RCA)** when problems occur in production.

An example of a shortcut could be that there is no budget left to fine-tune some security aspects, but in omitting this, you will potentially increase your exposure to malicious users. For an asset that deals with public data, this would be less risky than for an asset dealing with **personally identifiable information (PII)** data. Similarly, if the number-one requirement of a system is to be portable, you would likely envision a containerization platform instead of writing a plain .NET application leveraging Windows-specific features.

As you can see, it’s all about exercising good judgment over a specific context, and in essence, that is what ATAM is all about. **ATAM** helps software architects assess both some generic and specific concerns by assessing design choices against software quality attributes to meet some quality goals.

This may sound obvious, but quality, often defined as a combination of *fit for purpose* and *fit for use*, is sometimes an absolute requirement for software to function correctly or to adhere to high industry standards. While a slow report may irritate an end user, in a true real-time system, not being able to handle an event within 3 milliseconds would just not be acceptable.

Let’s come back to the fit-for-use and fit-for-purpose notions. **Fit for purpose** means that what was developed corresponds to the functional requirements, while **fit for use** means that what was developed works reliably. A typical example to illustrate these notions is given here: What do you expect from a washing machine? You expect it to clean your clothes *and* to function reliably. Indeed, a machine that would clean optimally but would encounter breakdowns every 2 weeks would be disappointing. That is why quality is the combination of these two notions.

The beauty of ATAM is that it forces you to think about fit for use and fit for purpose, which influences the resulting quality of a product.

ATAM will help you define your quality goals and make design decisions accordingly. It makes it possible to detect whether a quality attribute of interest is affected by one or more design decisions, enabling you to set priorities and focus on the most important factors that will be key to the success of the product you are building. ATAM should ideally be performed prior to the actual implementation, but you can also use it in *brownfield scenarios* to assess the adequacy of an as-is architecture. ATAM should be used as an inspirational source and in a pragmatic way. You do not need to apply it by the book.

Let's now explain the heart of ATAM.

## Understanding sensitivity points, trade-off points, risks, and non-risks

As stated in the previous section, making a design choice often has a positive or negative impact on one or more quality attributes. The two most important concepts to grasp are sensitivity points and trade-off points, and, to a lesser extent, risks and non-risks. Let's have a quick definition of these before going into more elaborate scenarios, as follows:

- **Sensitivity points** represent architectural choices that can importantly impact a single quality attribute—for example, maintenance activities could impact the availability of a quality attribute.
- **Trade-off points** are architectural decisions that may impact at least two sensitivity points, hence the trade-off, meaning that you might be obliged to sacrifice one quality attribute in favor of another one. For example, doing client-side encryption will impact security in a positive way but performance in a negative way. You may opt for better security but inferior performance.
- **Risks** are the consequences of some architectural decisions. You and the business might accept a certain level of residual risk—for example, you may decide to host your asset in a single data center. The residual risk is that you may experience an entire outage, should the data center suffer from a major disaster.
- **Non-risks** are opportunities brought by *good* architectural decisions—for example, using a certain technology may bring you additional security features out of the box.

**Sensitivity points** and **trade-off points** tell software architects where to focus their attention. They help identify what is important to achieve quality goals. Remember that these goals should be derived from business goals. Here are a few examples of sensitivity points:

- **Scalability**: The level of parallelism *might be sensitive* to the number of queues and/or partitions defined in the message broker.
- **Performance**: Latency *might be sensitive* to the application protocol we choose (for example, **HyperText Transfer Protocol 1 (HTTP/1)** or HTTP/2).
- **Security**: The risk of data leakage *might be sensitive* to the level of encryption applied to data at rest.

It is good to use the *might be sensitive* or *is sensitive* phrase construct when expressing sensitivity points, to emphasize the relationship between the consequence and the cause. Once defined, sensitivity points act as a baseline of factors that could impact the resulting quality. So, for instance, when taking the second item in the preceding list, there is a world of difference when working with HTTP/1 or HTTP/2 in terms of latency (better performance). However, although latency is primarily concerned by the protocol in use, other items, such as load balancing, could also be affected. Let's add an extra sensitivity point to illustrate our purpose, as follows:

- **Availability**: Backends *might be sensitive* to the application protocol we choose (for example, HTTP/1 or HTTP/2).

With the preceding sensitivity point added, we have two different quality attributes (performance and availability) that could be significantly impacted by the choice of protocol we make. That is a *trade-off point* because opting for HTTP/2 has a positive impact on performance (improved latency) but a potentially negative one on availability. The rationale behind this is that most load balancers are still layer-4 (**Transmission Control Protocol/Internet Protocol (TCP/IP)**) and do not understand layer-7 (in our case, HTTP/1 or HTTP/2), meaning that they will fail to properly load-balance HTTP/2 (used under the hood by the popular **Google remote procedure call (gRPC)**) because existing clients will keep sending their requests to the same backend, irrespective of whether that backend is healthy or on the verge of becoming unhealthy.

So, to make a proper design decision in this case, we would need to also consider our load balancers and make sure they are compatible with such a decision. If they are not, we might reconsider the protocol we want to use. If we keep using HTTP/2 despite our old-school load balancers, we end up with a **risk**. ATAM defines risks as potentially problematic design decisions or decisions that have not been made yet. On the contrary, a **non-risk** is based on some assumptions.

For example, on the same topic, an example of a non-risk would be this: Assuming a concurrent user base of 10 users per minute, our backend systems should still be able to deal with the load, despite our layer-4 load balancers' lack of proper understanding of layer-7 protocols. This non-risk is, therefore, valid while the assumption remains valid. Should the expected user base grow significantly, this non-risk will not be valid anymore or must be revalidated.

Another possible non-risk could be implied by some architectural styles; for instance, the system will always talk to the most responsive backend service because our service mesh technologies are layer-4 and layer-7 aware, under normal or high load.

If you have understood this section, then you have understood the essence of ATAM. It is a method that enables you to identify the key quality goals and success factors of well-crafted software architecture that is fit for purpose and fit for use. Let's now explore the quality attributes.

## Exploring quality attributes

When building an application or a system, you are always confronted with both **functional requirements (FRs)** and **non-functional requirements (NFRs)**. FRs are emitted by business users and business analysts, and they describe the features that must be developed or made available. NFRs are most of the time expected, but not directly expressed by the business. Indeed, every user wants to have a performant, reliable, and always available application. Similarly, every user assumes that the application/system is safe and respects their privacy, but all of this comes at a price, and the extent to which you want to respond to these NFRs will impact the cost and time dimensions. The systems you integrate with, the hosting platform you choose to host the asset, and many other factors could make it hard to achieve the NFRs.

The purpose of quality attributes is to express the NFRs and define expectations accordingly. The list of software quality attributes is broad; on its `https://en.wikipedia.org/wiki/List_of_system_quality_attributes` web page, Wikipedia regroups about 80 attributes. It is, therefore, crucial to refine them and pick the ones that make the most sense to you and your specific business scenario or industry.

Indeed, in some industries, you might have some common ground across assets from which you can extract a quality attribute baseline. In any case, whether you have a baseline or not, a good starting point is to request the **service-level agreement (SLA)** expected by the business. In large organizations, such SLAs are mainstream, even for internal customers.
