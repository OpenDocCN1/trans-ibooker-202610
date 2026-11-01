minute period. It is important to close the session if you are going for a break or you are not working on it. The cost of a session is $1.00.

For example, say you initiate an interactive session at 09:00 A.M. and work until 09:20 A.M., go for a break, and then initiate another session at 09:30 A.M., and work hard for the next hour, complete all your work, and go for another break. When you look at your AWS bill the next day, you will see $3.00 added to your bill even though the first session was only 20 minutes long.

In a different scenario, say you open a project for 2 minutes and close it, and then open the same project after 5 minutes and work for 10 minutes, close it, and repeat the same activity five times in a period of 30 minutes; it would only cost you $1. This is because every time you open a session, AWS Glue DataBrew assigns resources to provide quicker access and reduce the latency to assign resources to each project.

![](bbox=[0.125, 0.36, 0.875, 0.61])

Figure 2.9: Glue DataBrew interactive session getting initiated

AWS Glue DataBrew jobs also play a role in determining the pricing. When you create a dataset, you won’t have a data profile overview available for review. To generate the data profile overview, you need to run a data profile that creates an AWS Glue DataBrew job for you. In the upcoming sections, we’ll learn about the various options available for configuring our job before running it. However, for pricing purposes, one important configuration option to be aware of is how many nodes you assign to your job. This option can be found under Advanced job settings - optional. Note that this option is called optional because AWS Glue DataBrew has a default configuration of five nodes.