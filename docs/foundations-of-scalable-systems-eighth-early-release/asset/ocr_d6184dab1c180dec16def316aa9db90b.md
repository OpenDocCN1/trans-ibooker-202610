instance. This means all these simultaneous requests will incur additional response times due to cold start costs.

If a new request does not arrive and a resident runtime instance is not immediately reutilized, Lambda freezes the execution environment. If subsequent requests arrive, the environment is thawed and reused. If more requests do not arrive for the function, after a platform-controlled number of minutes Lambda will deactivate a frozen instance so it does not continue to consume platform resources.¹⁰

Cold start costs can be mitigated by using provisioned concurrency. This tells Lambda to keep a minimum number of runtime instances resident and ready to process requests with no cold start overheads. The ‘no free lunch’ principle applies of course, and charges increase based on the number of provisioned instances. You can also make a Lambda function a target of an AWS Application Load Balancer, in a similar fashion to that discussed in Chapter 5. For example, a load balancer policy can be defined that increases the provisioned concurrency for a function at a specified time, in anticipation of an increase in traffic.

# Execution Considerations

When you define a Lambda function, you specify the amount of memory that should be allocated to its runtime environment. Unlike GAE, you do not specify the number of vCPUs to utilize. Rather, the computation power is allocated in proportion to the memory specified, which is between 128MB and 10GB.

Lambda functions are charged for each millisecond of execution. The cost per millisecond grows with the amount of memory allocated to the runtime environment. For example, at the time of writing the costs per millisecond for a 2GB instance are double a 1GB instance.¹¹ Lambda does not specify precisely how much more compute capacity this additional memory buys your function, however. Still, the larger the amount of memory allocated, then the faster your Lambda functions will likely execute.¹²

This situation creates a subtle trade-off between performance and costs. Let’s examine a simple example based on the costs for 1GB and 2GB instances