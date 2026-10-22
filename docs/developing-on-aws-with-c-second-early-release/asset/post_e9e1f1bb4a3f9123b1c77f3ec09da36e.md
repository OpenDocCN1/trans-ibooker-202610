
maintaining replicas of your production servers on AWS EC2 and Elastic Block Store (EBS). This replication concept can be repurposed for the sake of performing a lift and shift rehosting. You simply set up replication to AWS then, when you are ready, switch over to running your application exclusively from your AWS replicas, allowing you to decommission the original server. An overview of how the Application Migration Service replicates your servers is shown in Figure 2-6

## Application Migration Service Replication

![](img/044e6b5616cc4c02f2683d0cc9990365_54_0.png)

Figure 2-6. Overview of the Application Migration Service

The Application Migration Service is accessed via the AWS management console, type “MGN” into the search or find it in the menu of the Migration Hub as in Figure 2-7

![](img/044e6b5616cc4c02f2683d0cc9990365_55_0.png)

Figure 2-7. Access MGN through the AWS Management Console

Set up of the Application Migration Service begins by installing the replication agent onto your servers, similar to how we installed the discovery agent for the Application Discovery Service. First log onto your Windows server and open up a PowerShell window as Administrator to download the agent installer for Windows. Replace <region> with the AWS region you would like to migrate your servers into.

```
PS C:\> mkdir MGNAgent
PS C:\> cd .\MGNAgent\
PS C:\MGNAgent> Invoke-WebRequest
https://aws-application-migration-service-<region>.s3.
<region>.amazonaws.com
/latest/windows/AwsReplicationWindowsInstaller.exe
-OutFile C:\MGNAgent\AwsReplicationWindowsInstaller.exe
```

Next put your region, access key ID and secret into variables if you haven’t already and execute the installer

```
PS C:\ADS Agent> $AWS_REGION="<region>"
PS C:\MGNAgent> $KEY_ID="<your-key-id>"
PS C:\MGNAgent> $KEY_SECRET="<your-key-secret>"

PS C:\MGNAgent> .\AwsReplicationWindowsInstaller.exe --region $AWS_REGION
--aws-access-key-id $KEY_ID --aws-secret-access-key $KEY_SECRET
```

The agent installer will ask you which disks on this server you want to replicate and then will get to work syncing your disks to AWS. You can see the status of the agent installer operations in the console window as shown in the following Figure 2-8.

![](img/044e6b5616cc4c02f2683d0cc9990365_56_0.png)

Figure 2-8. MGN Replication Agent Console Window

If you head back over to the MGN Management Console you will be able to see your server under Source Servers in the menu. Click on your server name and you can see the status of the replication for this server as shown in Figure 2-9. It can take a while to get through all the stages but once complete the status in the console will change to “Ready For Testing”. One nice feature of the Application Migration Service is the ability to spin up an instance of a server from a replica and test that everything is as you expect

it to be, without interrupting or otherwise interfering with the replication itself.

### Replication initiation steps

- Create security groups
- Launch Replication Server
- Boot Replication Server
- Authenticate with service
- Download replication software
- Create staging disks
- Attach staging disks
- Pair Replication Server with AWS Replication Agent
- Connect AWS Replication Agent to Replication Server
- Start data transfer

Figure 2-9. MGN Replication Status in the Management Console

To test a server select Launch test instances from the Test and Cutover action menu of your source server. This will launch a new EC2 instance that should mirror the original Windows server you replicated, with transfer of licenses handled automatically by the Application Migration Service. You can connect to the EC2 instance with Remote Desktop (RDP) by selecting it in the list of EC2 instances from the management console once it becomes ready. When you are happy that the test instance is working the way you expect it to you can execute the final stage of the migration: Cutover.

If you refer back to the stages of application migration via replication in Figure 2-6 you can see the final stage being Execute Cutover. This is where we create the resources for all our source servers (that is: spin up a new EC2 instance for each server), stop replication of our old servers, and allow us to decommission the original servers we installed the replication agents onto.

So now we have all our servers running like-for-like on EC2 we have performed the lift and shift rehosting, what’s next? Staying inside the realm of *rehosting*, we can go one step further and take advantage of AWS’s managed environment for running a web application: Elastic Beanstalk.

## Elastic Beanstalk

Elastic Beanstalk is a *managed* hosting environment for your web application. It supports a variety of backend stacks such as Java, Ruby or PHP but it is the .NET Framework support that we are most interested in here. The difference between hosting our app on a Windows server on EC2, as we did in the previous section, and using Elastic Beanstalk can be distilled down to one key discrepancy:

With an unmanaged server you upload your compiled and packaged website files to a server and then tweak the settings on that server in order to handle web traffic. With a managed service you upload your package to the cloud and the service will take care of the rest for you, setting up load balancers and dynamically scaling virtual machines horizontally. Managed services are the real draw for deploying your applications to the cloud and the more you lean into having AWS manage your infrastructure for you, the more you can concentrate on just writing code and solving problems for your business. Later in this book we will cover serverless programming and how you can architect your .NET applications to be more serverless, a concept rooted in managed services as much as possible. You can think of Elastic Beanstalk as an equivalent to “App Service” which you may be familiar with from a slightly *bluer* cloud provider.

So let’s get started with our first managed service on Elastic Beanstalk. The simplest way to deploy code is by using the AWS Toolkit for Visual Studio. With the AWS Toolkit installed, deploying to Elastic Beanstalk really is as simple as right clicking your solution and selecting “Publish to Elastic Beanstalk”. Figure 2-10 Shows the toolkit in use in the Solution Explorer of Visual Studio.

![](img/044e6b5616cc4c02f2683d0cc9990365_59_0.png)

Figure 2-10. Publishing to Elastic Beanstalk Directly from Visual Studio

There is of course also a CLI tool you can use in your CI pipeline. To get started with the Elastic Beanstalk CLI see the EB CLI Installer on GitHub. The CLI tool allows you to publish your code directly to Elastic Beanstalk from AWS CodeBuild or GitHub.

Lastly, there is one final tool worth mentioning if you are thinking of trying out Elastic Beanstalk and that is the Windows Web App Migration Assistant (WWMA). This is a PowerShell script that you can run on any Windows Server with an IIS hosted web application to automatically migrate the application onto Elastic Beanstalk without needing the source code at all. This is useful if you have a legacy .NET application that is no longer maintained and you want to take advantage of the benefits of Elastic Beanstalk but you no longer perform releases for this app. It is a true rehosting tool that simply moves the compiled website assets from your C:\inetpub folder on the server into EC2 instance(s) managed and scaled by Elastic Beanstalk.

## Replatforming via Containerization

As a migration strategy, replatforming is concerned with looking at the *platform* on which our .NET application is running and exploring moving somewhere else. In the case of a .NET Framework web application that *platform* will be some version of Windows Server running IIS. While previously we looked at Elastic Beanstalk as a way of getting more out of this infrastructure, Elastic Beanstalk still hosts your application on IIS on Windows Server, albeit in a much more scalable and efficient way. If we want to really push the envelope for scalability and performance, while still keeping away from the original source code (these are “legacy” applications after all) then we need to move away from IIS and onto something else. This is where containerization comes in.

I’m going to skip over exactly what containerization is and why it matters as we have Chapter 5 later in this book dedicated to Containerization of .NET, suffice to say moving your legacy .NET application from Windows Server web hosting to containers unlocks both performance and cost benefits to your organization, all without having to touch any of that legacy code.

## App2Container

App2Container is a command line tool from AWS that runs against your .NET application running on IIS on a Windows Server. It analyzes your application and dependencies, then creates Docker container images that can be deployed to an orchestration service in the cloud such as Elastic Container Service (ECS) or Amazon Elastic Kubernetes Services (Amazon EKS). Because App2Container runs on an application that is already deployed to a server, it doesn’t need access to your source code and sits right at the end of a deployment pipeline. For this reason App2container is perfect for quickly replatforming an old application that is not being actively developed and you don’t want to be rebuilding, simply skip right to the last two steps of the pipeline shown in Figure 2-11 and containerize the production files.
