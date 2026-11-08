
We could then create an Availability Group Listener by using the `New-SqlAvailabilityGroupListener` cmdlet. This is demonstrated in Listing [5-10](#394392_3_En_5_Chapter.xhtml#PC10).

```powershell
#Specify the options for the Listener
$ListenerOptions = @{
    Name = "Foo"
    StaticIp = "10.0.0.14/255.255.255.0"
    Path = "SQLSERVER:\\Sql\\CLUSTERNODE1\\PROD\\AvailabilityGroups\\Foo"
}
#Create the Listener
New-SqlAvailabilityGroupListener @ListenerOptions
```

Listing 5-10

Create an Availability Group Listener

## Summary
AlwaysOn Availability Groups can be implemented with up to eight secondary replicas, combining both Synchronous and Asynchronous Commit modes. When implementing high availability with availability groups, you always use Synchronous Commit mode, because Asynchronous Commit mode does not support automatic failover. When implementing Synchronous Commit mode, however, you must be aware of the associated performance penalty caused by committing the transaction on the secondary replica before it is committed on the primary replica. For disaster recovery, you will normally choose to implement Asynchronous Commit mode.

The availability group can be created via the New Availability Group wizard, through dialog boxes, through `T-SQL`, or even through `PowerShell`. If you create an availability group using dialog boxes, then some aspects, such as the endpoint and associated permissions, must be scripted using `T-SQL` or `PowerShell`.

If you implement disaster recovery with availability groups, then you need to configure a multi-subnet cluster. This does not mean that you must have SAN replication between the sites, however, since availability groups do not rely on shared storage. What you do need to do is add additional IP addresses for the administrative cluster access point and also for the Availability Group Listener. You also need to pay attention to the properties of the cluster that support client reconnection to ensure that clients do not experience a high number of timeouts.

© Peter A. Carter 2020 P. A. CarterSQL Server 2019 AlwaysOn[https://doi.org/10.1007/978-1-4842-6479-9\_6](https://doi.org/10.1007/978-1-4842-6479-9_6) Begin Abstract

# 6. Implementing AlwaysOn Availability Groups On Linux

Peter A. Carter^([1](#394392_3_En_6_Chapter.xhtml#Aff2) ) (1) Botley, UK End Abstract

Since SQL Server 2017, it has been possible to install SQL Server on Linux as well as Windows. AlwaysOn Availability Groups can be configured on Linux with much of the same functionality, as when the instances are hosted on Windows. The installation and configuration of SQL Server on Linux is beyond the scope of this book, but full details can be found in the Apress title *Pro SQL Server 2019 Administration*, which can be found at [www.apress.com/gb/book/9781484250884#otherversion=9781484250891](http://www.apress.com/gb/book/9781484250884%2523otherversion%253D9781484250891).

This chapter focuses on configuring Availability Groups on Linux, for the purpose of providing high availability (HA). We will first have a brief overview of the technologies involved in implementing pacemaker clusters (Pacemaker is a Linux-based clustering technology), before configuring a two-node Availability Group, with automatic failover.

### Note
For the demonstrations in this chapter, we will use three servers, namely, LinuxProd, LinuxSyncHA, and LinuxConfig. A default instance of SQL Server 2019 is installed on each of the servers. The servers are built with the Ubuntu 18.04 operating system and are not joined to a domain.

In this chapter, we will perform the following activities:
* Create the Sales Databases.
* Enable Availability Groups on our instances and prepare the instances to support Availability Groups.
* Create an Availability group for the Sales database.
* Create the cluster.
* Configure the cluster.

## Linux Cluster Technologies
The following sections will provide a high-level overview of the technologies that are used to configure a cluster in Linux. Before we begin, however, it is important to understand that unlike building Availability Groups on a Windows cluster, when building Availability Groups on a Linux pacemaker cluster, the Availability Group is not integrated with the cluster to the same degree. Specifically, the Availability Group is not aware of the cluster’s state, or even existence. Therefore, failover operations must be performed at the cluster level. Additionally, while it is still possible to create a listener, the functionality is limited. You need to manually register the listener’s name in DNS and there is no support for read-only routing.

### Pcs
When you install `pcs`, the package will also install Pacemaker and Corosync. The `pcs` component itself, however, is a command-line utility, which is used to manage the Pacemaker cluster.

### Pacemaker
Pacemaker is the heart of the cluster. It is responsible for managing cluster maintenance events, such as nodes being added and removed. It is also responsible for managing cluster events and moving resources between nodes to ensure the resources remain available after a failure occurs on a node.

Pacemaker supports all standard cluster configurations, including active/active, active/passive, N+1, and N+M. A discussion of advanced Pacemaker configurations is beyond the scope of this book; however, further details can be found in the Apress title *Pro Linux High Availability Clustering*, which can be found at [www.apress.com/gp/book/9781484200803](http://www.apress.com/gp/book/9781484200803).

### Corosync
Corosync provides the messaging layer for the cluster. It implements the quorum system by sending messages between nodes to check the health status and notifies Pacemaker if the quorum is lost.

### STONITH
A supported configuration of a Pacemaker cluster must use fencing, which helps bring the cluster to a known good state, in the event of a node, or resources state being unknown. There are two types of fencing: resource fencing and node fencing. Resource fencing is used to prevent a resource from starting on a node. This can avoid data corruption, in the event of an outage while a resource is being configured. Node fencing prevents a node from running any resources. This is usually required if a node becomes unresponsive. Node fencing requires a fencing device and a fencing agent. The fencing device may be an uninterrupted power supply, a power distribution unit, or a management interface, such as lights-out device or blade power control device. The agent that interacts with these devices is STONITH (shoot the other node in the head). Because the implementation of STONITH is very specific to the fencing device you use, we will not configure it in this chapter. That is fine for development or testing purposes, but a production cluster should always have STONITH configured.

## Preparing to Implement AlwaysOn Availability Groups
Before creating an Availability Group, there are several prerequisite tasks that need to be performed. Namely, we enable Availability Groups on the SQL Server instances and create and backup the database that we want to be made highly available, and because Linux servers cannot authenticate with each other, we will need to create certificates that can be used for authentication. The following sections demonstrate how to perform each of these tasks.

### Enable Availability Groups
Just as is required in a Windows environment, the first step in configuring Availability Groups on Linux is to enable the feature at the service level. The script in Listing [6-1](#394392_3_En_6_Chapter.xhtml#PC1) demonstrates how to enable Availability Groups and then restart the service. This script needs to be executed on each server that will host a Replica.

### Tip
`sudo` is used to elevate your permission when running a command.

```bash
sudo /opt/mssql/bin/mssql-conf set hadr.hadrenabled 1
sudo systemctl restart mssql-server
```

Listing 6-1

Enable Availability Groups

### Create the Sales Database
