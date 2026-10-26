You can also use an Apache Flink interface for your Kinesis Data Analytics application. Essentially you would create your Flink application in your favorite integrated development environment (IDE) and then upload the application code to Kinesis Data Analytics. This would allow you to run your application in a fully managed elastic service. You can then use the AWS ecosystem to manage your application. For example, you can configure various metrics with Amazon CloudWatch—such as Applications, Tasks, Operator, and Parallelism—and also display the Job graph in the AWS Management Console.

Kinesis provides a Kinesis process unit (KPU) as a resource to run the code. Each KPU is 1 vCPU, 4 GB of RAM, and 50 GB of storage at the time of this writing. The number of KPUs needed for your application is calculated using the Parallelism and ParallelismPerKPU properties:

+   - Parallelism – This is the property that is used to set the default Apache Flink application parallelism. All operators, sinks, and sources will use this level of parallelism unless they are explicitly overridden in the application. The default value is 1 and the maximum value is 64.

+   - ParallelismPerKPU – This property is used to set the number of parallel tasks that can be scheduled per KPU of your application. The default is 1 and the maximum is 8.

Please note that the limits are soft limits and can be increased by requesting a limit increase.

You can use the autoscaling capability of your Kinesis application to scale your resource up and down depending on the requirements and throughput needed (Figure 2.19).

# FIGURE 2.19 Kinesis Data Analytics - Flink Interface

![](bbox=[0.12, 0.52, 0.85, 0.9])