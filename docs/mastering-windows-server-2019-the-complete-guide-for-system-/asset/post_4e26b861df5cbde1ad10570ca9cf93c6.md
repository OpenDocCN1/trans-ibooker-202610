
## Virtual machine resiliency

As you can successfully infer from the name, **virtual machine resiliency** is an improvement in clustering that specifically benefits Hyper-V server clusters. In the clustering days of Server 2012 R2, it wasn't uncommon to have some intra-array, or intra-cluster, communication problems. These sometimes represented themselves as transient failures, meaning that the cluster thought a node was going offline when it actually wasn't, and would set into motion a failover that sometimes caused more downtime than if the recognition patterns for a real failure would have simply been a little bit better in the first place. For the most part, clustering and the failover of cluster nodes worked successfully, but there is always room for improvement. That is what virtual machine resiliency is all about. You can now configure options for resiliency, giving you the ability to more specifically define what behavior your nodes will take during cluster-node failures. You can define things such as **resiliency level**, which tells the cluster how to handle failures. You also set your own **resiliency period**, which is the amount of time that VMs are allowed to run in an isolated state.

Another change is that unhealthy cluster nodes are now placed into quarantine for an admin-defined amount of time. They are not allowed to rejoin the cluster until they have been identified as healthy and have waited out their time period, preventing situations such as a node that was stuck in a reboot cycle inadvertently rejoining the cluster and causing continuous problems as it cycles up and down.

## Storage Replica (SR)

SR is a new way to synchronize data between servers. It is a data-replication technology that provides the ability for block-level data replication between servers, even across different physical sites. SR is a type of redundancy that we hadn't seen in a Microsoft platform prior to Windows Server 2016; in the past, we had to rely on third-party tools for this kind of capability. SR is also important to discuss on the heels of failover clustering because SR is the secret sauce that enables multi-site failover clustering to happen. When you want to host cluster nodes in multiple physical locations, you need a way to make sure that the data used by those cluster nodes is synced continuously, so that a failover is actually possible. This data flow is provided by SR.

One of the neat data points about SR is that it finally allows a single-vendor solution, that vendor being Microsoft of course, to provide the end-to-end technology and software for storage and clustering. It is also hardware-agnostic, giving you the ability to utilize your own preference for storage media.

SR is meant to be tightly integrated and one of the supporting technologies of a solid failover clustering environment. In fact, the graphical management interface for SR is located inside the Failover Cluster Manager software – but is of course also configurable through PowerShell – so make sure that you take a look into failover clustering and Storage Replica as a *better together* story for your environment.

Storage Replica is now available inside Server 2019 Standard edition! (Previously, it required Datacenter edition, which was prohibitive to some implementations.) Administration of SR is also now available inside the new **Windows Admin Center** (WAC). Let's put together a Storage Replica environment, so you can see how quick and easy it is to create storage redundancy.

## Configuring Storage Replica

I have created two new servers in my lab, with the intention of turning them into redundant file servers via SR. These servers are called FS01 and FS02 and are joined to my domain, and there are a few important things I have done with them:

- Attached a new disk to be used for data storage. Windows is running on a normal virtual disk on my server, but I added a second one that will be dedicated to storage. The important part here is that the data disk must be initialized with GPT.
- Created a volume to contain data. This volume in Windows is labeled as the S: drive, and once again I am just making sure that this data volume resides on a GPT-initialized disk.
- Created a separate volume to contain SR logging data. When creating the SR replication set shortly, we will need to define a location for keeping SR logs, and it is easiest to do so on its own drive letter. I have called mine the L: drive. It also resides on my GPT-initialized data disk. The volume used for logs must be a minimum of 9GB.
- Installed the **Storage Replica** feature inside Windows, on both servers.

## Initializing disks as GPT

If you're not familiar with the process stated above to initialize your new data disk as GPT, let's cover that for a minute. After attaching a new disk to your server, right-click on the **Start Button** and head into **Disk Management**. This tool should automatically recognize the new disk and ask whether you want to initialize it using MBR or GPT. Make sure to select **GPT** here:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_542_0.png)

Figure 12.25: Initializing a new disk as GPT

## Testing preparedness for Storage Replica

We now have two file servers, each with a new GPT data disk that contains two new volumes. One will be used for SR data storage, and the other for SR logs. Here's a quick look at the volumes I have. They are set up exactly the same on both servers:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_543_0.png)

Figure 12.26: Volumes created on the servers

At this point, we could plow forward with creating the Storage Replica environment, and since these are brand new servers with identical configurations, it would most likely work. However, you will often be configuring SR on servers that have already been running in production for a while, or at least the primary file server where your source data is going to be located. Knowing whether or not SR is going to be successful would be comforting knowledge.

Thankfully, there is a testing process that SR can run before putting your configurations into place. Using PowerShell, let's issue a big long command from our primary FS01 server. The following is the command I am running. You will, of course, have to adjust it to accommodate your own server names and volume letters. As you can see at the end of the command, I specify for the output to be placed inside the C:\SR_TEST folder, so make sure that folder exists on your server:

```
Test-SRTopology -SourceComputerName FS01 -SourceVolumeName S: -SourceLogVolumeName L: -DestinationComputerName FS02 -DestinationVolumeName S: -DestinationLogVolumeName L: -DurationInMinutes 10 -ResultPath C:\SR_TEST\
```

This test will gather information about the servers and volumes and run some sample data transfers in the background for 10 minutes (as specified in the command). After completion, you will find a results file inside c:\SR_TEST that should launch in a web browser to show you all kinds of resulting information. Any issues with the environment that may hinder SR's ability to work will be detailed here so that you can go resolve those issues:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_544_0.png)

Figure 12.27: Storage Replica test report

You'll notice in Figure 12.27 that I do have some warnings in my lab, which I did intentionally so that you had a visual of what the warnings look like. I never opened up ICMP traffic between these servers, so I received a warning about not being able to perform the network latency test. Also shown are some physical memory warnings. In a Storage Replica group, each server needs to have 2GB of RAM for SR to work properly, which is what I assigned to my VMs. While 2GB is required, 4GB is recommended, and the warnings are simply letting me know. In a production environment, you would never run a file server with even as little as 4GB of RAM, so you will likely not encounter these warnings during a real implementation.
