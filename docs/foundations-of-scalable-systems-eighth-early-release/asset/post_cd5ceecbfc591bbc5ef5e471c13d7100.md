
should be straightforward to obtain from the serverless monitoring tools available

Now, I’ll move on to the really interesting part – the results.

## Results

Table 8-3 shows the mean throughput for each test configuration. The highest throughput of 6178 requests per second is provided by the (CPU80, max10) configuration. This value is 1.7% higher than that provided by the default settings (CPU60, max10), and around 9% higher than the lowest throughput of 5605 requests per second. So the results show a roughly 10% variation from lowest to highest throughput. Same code. Same request load. Different configuration parameters.

Table 8-3. Mean Throughput for each test

| Throughput | max10 | max35 | max60 | max80 |
|---|---|---|---|---|
| CPU60 | 6006 | 6067 | 5860 | 5636 |
| CPU70 | 6064 | 6121 | 5993 | 5793 |
| CPU80 | 6178 | 5988 | 5989 | 5605 |

Now I’ll factor in cost. In Table 8-4, I’ve normalized the cost for each test run by the cost of the default GAE configuration {CPU60, max10}. So for example, the cost of the {CPU70, max10} configuration was 18% higher than the default, and the cost of the {CPU80, max80} configuration was 45% lower than the default.

Table 8-4. Mean Cost for each test condition

| Normalized Instance Hours | max10 | max35 | max60 | max80 |
|---|---|---|---|---|
| CPU60 | 100% | 72% | 63% | 63% |
| CPU70 | 118% | 82% | 63% | 55% |
| CPU80 | 100% | 72% | 82% | 55% |

There are several rather interesting observations we can make from these results:

- The default settings {CPU60, max10} give neither the highest performance nor lowest cost. This configuration makes Google happy but maybe not your client.
- We obtain 3% higher performance with the {CPU80, max10} configuration at the same cost of the default configuration.
- We obtain marginally (approximately 2%) higher performance with 18% lower costs from the {CPU70, max35} configuration as compared to the default configuration settings.
- We obtain 96% of the default configuration performance at 55% of the costs with the {CPU70, max80} test configuration. That is a pretty decent cost saving for slightly lower throughput.

Armed with this information, you can choose the configuration settings that best balance your costs and performance needs. With multiple, dependent configuration parameters, you are unlikely to find the ‘best’ setting through intuition and expertise. There are too many intertwined factors at play for that to happen. Parameter studies let you quickly and rigorously explore a range of parameter settings. With two or three parameters and three or four values for each, you can explore the parameter space quickly and cheaply. This enables you to see the effects of the combinations of values and make educated decisions on how to deploy your application.

## Summary and Further Reading

Serverless platforms are a powerful tool for building scalable applications. They eliminate many of the deployment complexities associated with managing and updating clusters of explicitly allocated virtual machines. Deployment is as simple as developing the service’s code, and uploading it to the platform along with a configuration file. The serverless platform you are using takes care of the rest.

In theory anyway.

In practice of course, there are important dials and knobs that you can use to tune the way the underlying serverless platforms manage your functions. These are all platform specific, but many relate to performance and scalability, and ultimately the amount of money you pay. The case study in this chapter illustrated this relationship and provided you with an approach you can utilize to find that elusive ‘sweet spot’ that provides the required performance at lower costs than the default platform parameter settings provide.

Exploiting the benefits of serverless computing requires you to buy into a cloud service provider. There are many to choose from, but all come with the attendant vendor lock-in and downstream pain and suffering if you ever decide to migrate to a new platform.

There are open source serverless platforms such as Apache OpenWhisk (https://openwhisk.apache.org/) that can be deployed to on-premise hardware or cloud-provisioned virtual resources. There are also solutions such as the Serverless Framework (https://www.serverless.com/) that are provider-independent. These make it possible to deploy applications written in Serverless to a number of mainstream cloud providers, including all the usual suspects. This delivers code portability but does not insulate the system from the complexities of different provider deployment environments. Inevitably, achieving the required performance, scalability and security on a new platform is not going to be a walk in the park.

A great source of information on serverless computing is Jason Katzer’s *Learning Serverless* (2020, O’Reilly Media). I’d also recommend two extremely interesting articles that discuss the current state of the art and future possibilities for serverless computing. These are:

- D. Taibi, J. Spillner and K. Wawruch, “Serverless Computing-Where Are We Now, and Where Are We Heading?,” in IEEE Software, vol. 38, no. 1, pp. 25-31, Jan.-Feb. 2021, doi: 10.1109/MS.2020.3028708.
- J. Schleier-Smith et al, 2021. What serverless computing is and should become: the next phase of cloud computing. Commun. ACM 64, 5 (May 2021), 76–84.

Finally, serverless platforms are a common technology for implementing *microservices architectures*. Microservices are an architectural pattern for

decomposing an application into multiple independently deployable and scalable ‘parts’. This design approach is highly amenable to a serverless-based implementation, and conveniently, are the topic we cover in the next chapter.

1 A brief overview of scalable services can be found at this link https://medium.com/@i.gorton/six-rules-of-thumb-for-scaling-software-architectures-a831960414f9
2 A survey of cloud bills is an interesting read - https://www.prnewswire.com/news-releases/survey-enterprises-overspending-by-millions-on-cloud-bills-300968100.html
3 An example of unexpected cloud bills is at https://www.infotech.com/software-reviews/research/unexpected-cloud-costs-are-rocking-the-enterprise
4 This link details the supported runtime environments - https://cloud.google.com/appengine/docs/the-appengine-environments
5 A GAE language comparison illustrates the load times - see https://medium.com/dev-genius/scalability-and-cost-analysis-for-cloud-based-software-systems-part-1-472012435b26
6 See this link for the documentation - https://cloud.google.com/appengine/docs/standard/java/config/appref#automatic_scaling_max_concurrent_requests
7 There’s also an optional min-pending-latency parameter, with a default value of zero. If you are brave, how the minimum and maximum values work together is explained here. https://cloud.google.com/appengine/docs/standard/python/config/appref#scaling_elements
8 As of 2021, Lambda also supports services that are built using Docker containers. This gives the developer the scope to choose language runtime when creating the container image.
9 A good description of cold start behavior is at this link https://medium.com/hackernoon/im-afraid-you-re-thinking-about-aws-lambda-cold-starts-all-wrong-7d907f278a4f
10 This experiment describes how long idle function are kept resident - https://acloudguru.com/blog/engineering/how-long-does-aws-lambda-keep-your-idle-functions-around-before-a-cold-start?utm_source=medium_blog&utm_medium=redirect&utm_campaign=medium_blog
11 https://aws.amazon.com/lambda/pricing/
12 “At 1,769 MB, a function has the equivalent of one vCPU (one vCPU-second of credits per second).” from https://docs.aws.amazon.com/lambda/latest/dg/configuration-function-common.html
13 Established customers can negotiate with AWS to increase these limits.
14 Alternatively, if the lambda usage is across different applications, it could be separated into different accounts. AWS account design and usage is however outside the scope of this book.
15 Actually, this maximum reserved concurrency for a function is the (Burst Limit -100). AWS reserves 100 concurrent instances for all functions that are not associated with explicit concurrency limits. This ensures that all functions have access to some spare capacity to execute.
