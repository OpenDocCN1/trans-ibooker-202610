
For situations in which the data required for a processing or learning task is larger than a single computer’s system memory (RAM), you can use out-of-core, external memory, or incremental learning techniques and algorithms.

# Production Scalability

Scalability—the ability to handle expected load on the system—is a very important consideration when building production AI solutions. Required scalability is usually accomplished by using servers that are either *scaled up* (scaled vertically) or *scaled out* (scaled horizontally).

Scaling up means to increase the memory capacity and processing power of a single machine (and sometimes disk storage, as well), whereas horizontal scaling means to add additional low-cost, commodity computing resources to distribute the workload. The latter is often referred to as *distributed computing*. Both options typically result in increases in cost.

Another technique for scaling out to handle a large number of concurrent requests that require processing data through a production model (e.g., make a prediction) is to deploy the same model to many different machines and distribute the requests through routing (e.g., load balancing), or to spin up ephemeral (temporary), on-demand, *serverless* workers using a technology such as AWS’s Lambda. Serverless technologies such as Lambda are highly scalable and do not require the creation, maintenance, and general overhead of running a full and persistent server.

Other options include using a Platform as a Service (PaaS; e.g., Heroku) or Infrastructure as a Service (IaaS) provider like AWS or GCP. These platforms help abstract away many of the complexities associated with system and network administration, DevOps/DataOps, and site reliability engineering, for example. There also are a growing number of scalable, specialized APIs being made available as a service that provide various AI, machine learning, and other advanced analytics-related functionality.

# Learning and Solution Maintenance

Lastly, AI solutions are trained, as discussed, in a local or remote development environment. After you develop and test them, a deliverable is then deployed to a production environment where new and unseen data passes through it in order to produce the desired benefits and outcomes. This can be handled offline in a batch process, in real-time (minimal time delays), or near real-time (e.g., acceptable delay of several seconds or minutes).

Offline learning (aka batch learning) is when deliverables are trained outside of production on an entire dataset or on a subset of the data (minibatch). Part of this involves storage and access of data from data stores such as a relational database, NoSQL database, data warehouse, or data lake.

Online learning, on the other hand, refers to when retraining and recreation of the deliverable, along with performance assessment, occurs online in a production environment with production data. This process is usually carried out on a recurring interval (cadence) that can be minutes, days, or longer.

Online learning is intended to address maintenance and upkeep of target performance based on changing data, as in the batch learning case, but it also is designed to incrementally update and improve deployed solutions without retraining on an entire dataset. This requires performant and available data storage and access and is dependent on factors such as network communications, latency, and availability of network resources.

Online learning is associated with the concept of online algorithms, in which algorithms receive their input over time and not all at once, as in the offline case. Online learning is also associated with incremental learning techniques, which is when models continue learning as new data arrives while also retaining previously learned information.

Because data, and the underlying information on which AI solutions are based, can change over time due to trends, behaviors, and other factors, the solutions should be updated on a recurring interval. This allows them to capture these changes and maintain the desired level of performance. Without doing this, performance degradation or drift is common, particularly with predictive models.

Manual deliverable training and optimization iterations are typically carried out before deploying the new and optimized deliverable to production. Alternatively, automated learning is another method for automating these deliverable updates. Automated learning is when model training, validation, performance assessment, and optimization are automated on a regular cadence, and then new and improved models are deployed to replace existing models in production. The deployment process can be either automated or manual (for an additional safety check).

This approach should include some form of model performance tracking and comparison framework to determine whether automatically generated and validated models are an improvement relative to a previous or base model.

# Bibliography

*   Coppenhaver, Robert. *From Voices to Results - Voice of Customer Questions, Tools and Analysis: Proven techniques for understanding and engaging with your customers*. Packt Publishing, 2018.

*   Domingos, Pedro. The Master Algorithm: How the Quest for the Ultimate Learning Machine Will Remake Our World. New York: Basic Books, 2015.

*   Drexler, K.E. (2019): “Reframing Superintelligence: Comprehensive AI Services as General Intelligence”, Technical Report 2019-1, Future of Humanity Institute, University of Oxford.

*   The Future of Jobs Report 2018, World Economic Forum, Switzerland, 2018.

*   Hawkins, Jeff, and Sandra Blakeslee. *On Intelligence:* Times Books/Henry Holt, 2008.

*   Horowitz, Ben. *The Hard Thing About Hard Things: Building a Business When There Are No Easy Answers*. New York: HarperCollins, 2014.

*   Hu, H. (2015). Graph Based Models for Unsupervised High Dimensional Data Clustering and Network Analysis. UCLA. ProQuest ID: Hu_ucla_0031D_13496. Merritt ID: ark:/13030/m50z9b68. Retrieved from [`http://bit.ly/2X1gRss`](http://bit.ly/2X1gRss).

*   Krug, Steve. *Don’t Make Me Think, Revisited: A Common Sense Approach to Web Usability*. 3rd ed., New Riders, 2014.

*   National Commission on Technology, Automation and Economic Progress, Technology and the American Economy, Volume 1, February 1966, pg. 9.

*   Olsen, Dan. *The Lean Product Playbook: How to Innovate with Minimum Viable Products and Rapid Customer Feedback*. New Jersey: Wiley, 2015.

*   Patterson, Josh, and Adam Gibson. *Deep Learning: A Practitioner’s Approach*. O’Reilly Media, 2017.

*   Pearl, Judea, and Dana Mackenzie. *The Book of Why: The New Science of Cause and Effect*. New York: Basic Books, 2018.

*   Sinek, Simon. *Start with Why: How Great Leaders Inspire Everyone to Take Action*. New York: Portfolio/Penguin, 2009.

# Index

## A

*   AARRR (acquisition, activation, retention, referral, and revenue), [Measure Success](#ch13.html#idm45909201758472)

*   acquihiring, [Acquire, Retain, and Train Talent](#ch13.html#idm45909201874344)

*   acquire phase of GABDO AI Process Model, [Acquire](#app02.html#ix_appendix_B-asciidoc3)\-[Example continued](#app02.html#idm45909201144312)

    *   acquiring data, [Acquire Data](#app02.html#idm45909201150136)

    *   identifying data, [Identify Data](#app02.html#idm45909201161928)

    *   preparing data, [Prepare Data](#app02.html#idm45909201168600)

*   action-readiness, [Action ready](#ch12.html#idm45909202016728)

*   actionable insights, [Deep Actionable Insights](#ch07.html#idm45909202887816), [Take Action](#app02.html#idm45909201100552)

*   activation, defined, [Biological Neural Networks Overview](#app01.html#idm45909201307032)

*   advanced analytics, [Racing to Business Success](#ch01.html#idm45909204369256)

*   AGI (see artificial general intelligence)

*   Agile development methodology, [An Agile Analogy](#ch03.html#idm45909204029432)
