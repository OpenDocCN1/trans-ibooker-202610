+   - **Resources:** An activity needs resources to be performed. For example, an activity like PigActivity or EMRAcitivity requires computational resources like EMR to be available. AWS Data Pipeline supports the following types of resources:
+     - Ec2Resource
+     - EMRCluster

+   - **Actions:** During a pipeline execution, steps can succeed, fail, or get delayed. You would need to take appropriate actions when such an event happens. AWS Data Pipeline caters to this need of pipeline building and supports the following actions:
+     - SnsAlaram – This action sends an SNS notification to a topic based on onSuccess, onFail, and onLateAction events.
+     - Terminate – This action triggers the cancellation of pending or unfinished activities, resources, or data nodes.

# Large-Scale Data Transfer Solutions

We have seen a number of different technologies that allow you to collect data from different data sources when the volumes are low to moderate. However, when you would come across situations when you need to migrate large data volumes, such as when data sizes are over multiple terabytes or petabytes, transferring the data over an Internet connection could take enormously large amounts of time. There are also scenarios where you need portable data transfers but having the ability to transfer multiple Terabytes of data. Those are the scenarios in which you either need a portable solution like AWS Snowcone or large-scale data transfer options like AWS Snowball and AWS Snowmobile and connectivity options like AWS Direct Connect. Let’s look in a bit more detail about what these options are.

# AWS Snowcone

AWS Snowcone is the smallest member of the AWS Snow family which weighs as little as 2.1 kg and provides you with 8 terabytes of usable storage. Its small size makes it ideal for tight spaces or where portability is the key. It is useful in a number of scenarios including cases where organizations want to move multiple Terabytes of data to the cloud but lack adequate network bandwidth, or in some cases little to no connectivity in places where the data resides.

Furthermore, AWS Snowcone might be useful in Healthcare IoT where it can be used for data collection and data processing in healthcare facilities or remote medical areas. It can be used to transfer crucial data from emergency vehicles in real-time.