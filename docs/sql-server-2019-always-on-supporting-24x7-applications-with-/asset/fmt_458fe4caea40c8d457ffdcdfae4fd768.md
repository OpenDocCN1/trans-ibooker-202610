
# Availability Group Configuration

The configuration-only node does not contain a copy of the user databases, but the Availability Group metadata is synchronized, using synchronous commit mode. The configuration-only node can use any version of `SQL Server`, including `SQL Server Express edition`, to avoid additional costs. A single instance can also act as the witness for multiple Availability Groups. However, a single Availability Group can only ever have a single configuration-only node.

Figure [6-2](#394392_3_En_6_Chapter.xhtml#Fig2) shows the Backup Preferences page of the New Availability Group dialog box. Here, we have configured backups to be taken from the Primary node. ![Figure 6-2 ¶ New Availability Group Dialog Box – Backup Preferences Tab](images/394392_3_En_6_Chapter/394392_3_En_6_Fig2_HTML.jpg)

Figure 6-2 ¶ New Availability Group Dialog Box – Backup Preferences Tab

Because we are only configuring high availability, there is no need to configure the Read-only Routing. A discussion of Read-only Routing and other configuration options not discussed here can be found in Chapter [5](#394392_3_En_5_Chapter.xhtml), however.

Alternatively, we could create the Availability Group via T-SQL, using the script in Listing [6-8](#394392_3_En_6_Chapter.xhtml#PC8).

```
USE master
GO
CREATE AVAILABILITY GROUP LinuxAOAG WITH (
      AUTOMATED_BACKUP_PREFERENCE = PRIMARY,
      DB_FAILOVER = OFF,
      DTC_SUPPORT = NONE,
      CLUSTER_TYPE = EXTERNAL,
      REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT = 0
) FOR DATABASE Sales
REPLICA ON
      'linuxconfig' WITH (
            ENDPOINT_URL = 'TCP://linuxconfig.AlwaysOn.com:5022',
            AVAILABILITY_MODE = CONFIGURATION_ONLY
      ),
      'linuxprod' WITH (
            ENDPOINT_URL = 'TCP://linuxprod.AlwaysOn.com:5022',
            FAILOVER_MODE = EXTERNAL,
            AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
            SESSION_TIMEOUT = 10,
            BACKUP_PRIORITY = 50,
            SEEDING_MODE = MANUAL,
            PRIMARY_ROLE(ALLOW_CONNECTIONS = ALL),
            SECONDARY_ROLE(ALLOW_CONNECTIONS = NO)
      ),
      'linuxsyncha' WITH (
            ENDPOINT_URL = 'TCP://linuxsyncha.AlwaysOn.com:5022',
            FAILOVER_MODE = EXTERNAL,
            AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
            SESSION_TIMEOUT = 10,
            BACKUP_PRIORITY = 0,
            SEEDING_MODE = MANUAL,
            PRIMARY_ROLE(ALLOW_CONNECTIONS = ALL),
            SECONDARY_ROLE(ALLOW_CONNECTIONS = NO)
      )
;
GO
```

Listing 6-8

Create the Availability Group

To manually seed the Sales database with a backup, you can copy the backup file to the secondary server by running the command in Listing [6-9](#394392_3_En_6_Chapter.xhtml#PC9) from the primary server.

```
Scp /var/opt/mssql/data/sales.bak pete@linuxprod:/var/opt/mssql/data
```

Listing 6-9

Copy the Backup of the Sales Database

The database can be restored using the command in Listing [6-10](#394392_3_En_6_Chapter.xhtml#PC10).

```
USE master
GO
RESTORE DATABASE Sales FROM DISK = '/var/opt/mssql/data/Sales.bak' WITH
      FILE = 1,
      MOVE N'Sales' TO N'/var/opt/mssql/data/Sales.mdf',
      MOVE N'Sales_log' TO N'/var/opt/mssql/data/Sales_log.ldf',
      NORECOVERY ;
GO
```

Listing 6-10

Restore the Sales Database

Running the script in Listing [6-11](#394392_3_En_6_Chapter.xhtml#PC11) will join our secondary replica to the Availability Group and ensure that it has the appropriate permissions. This script needs to be run against all instances that will participate in the Availability Group.

```
ALTER AVAILABILITY GROUP LinuxAOAG JOIN WITH (CLUSTER_TYPE = EXTERNAL) ;
GO
ALTER AVAILABILITY GROUP LinuxAOAG GRANT CREATE ANY DATABASE ;
GO
```

Listing 6-11

Join the Secondary to the Availability Group

## Configuring the Cluster

Before creating the cluster, we will need to create a login that will be used by the Pacemaker account and assign it the permissions that it requires, in each of the SQL Server instances that will be participating in the Availability Group. This can be achieved by using the script in Listing [6-12](#394392_3_En_6_Chapter.xhtml#PC12).

```
CREATE LOGIN Pacemaker WITH PASSWORD = 'Pa$$w0rd'
GO
GRANT ALTER, CONTROL, VIEW DEFINITION ON AVAILABILITY GROUP::LinuxAOAG TO Pacemaker ;
GRANT VIEW SERVER STATE TO Pacemaker
```

Listing 6-12

Configure the Pacemaker Login

Next, we will need to configure the firewall. The ports that the cluster will require are detailed in Table [6-1](#394392_3_En_6_Chapter.xhtml#Tab1).

Table 6-1

Ports Required by Cluster

| Port || Description |
| --- | --- |
| TCP 2224 || Required on all nodes. `pcsd` Web UI and node-to-node communication |
| --- | --- |
| TCP 3121 || Required on all nodes if Pacemaker has remote nodes. Used for communication between the `crmd` and `pacemaker_remoted` daemons |
| TCP 21064 || Required on all nodes. Used by resources requiring DLM |
| UDP 5405 || Required on all nodes. Used by Corosync |
| TCP 1433 || Required on all nodes. The examples in this chapter have the SQL instances listen on 1433; however, you should change this to match your own environment |
| TCP 5022 || Required on all nodes. The examples in this chapter have the AlwaysOn endpoints on 5022; however, you should change this to match your own environment |

Listing [6-13](#394392_3_En_6_Chapter.xhtml#PC13) provides an example of how to configure firewall rules in Ubuntu.

```
sudo ufw allow 2224/tcp
sudo ufw allow 3121/tcp
sudo ufw allow 21064/tcp
sudo ufw allow 5405/udp
sudo ufw allow 1433/tcp
sudo ufw allow 5022/tcp
sudo ufw reload
```

Listing 6-13

Configure Firewall Rules

Next, we will install the Pacemaker components. We can do this with the command in Listing [6-14](#394392_3_En_6_Chapter.xhtml#PC14). This command should be run on all server participating in the Availability Group.

```
sudo apt-get install pacemaker pcs resource-agents
```

Listing 6-14

Install Pacemaker Components

**Caution**

For a production system, be sure to also install `fence-agents` for node fencing.

During the installation of the `pcs` components, a `hacluster` will be created. You should set a password for this user using the `passwd` command, as shown in Listing [6-15](#394392_3_En_6_Chapter.xhtml#PC15). After running the command, you will be prompted to enter and then confirm the new password.

**Note**

It is important that you set the same password on each node.

```
Sudo passwd hacluster
```

Listing 6-15

Set hacluster Password

We can now enable the `pcsd` and `pacemarker` services, using the script in Listing [6-16](#394392_3_En_6_Chapter.xhtml#PC16). You will notice that before enabling pacemaker, we issue a command to destroy any existing cluster. This is because a file called `/etc/cluster/corosync.conf` was created during the install of `pcs`, however, enabling pacemaker attempts to create this file. Using the `cluster destroy` command removes this file to avoid a failure when pacemaker is enabled.

```
sudo systemctl enable pcsd
sudo systemctl start pcsd
sudo pcs cluster destroy
sudo systemctl enable pacemaker
```

Listing 6-16

Enable pcs and pacemarker

Now that pacemaker is enabled, we can build the cluster. The process for this consists of running five `pcs cluster` commands. The first is `auth`. This is used to authenticate `pcs` to the `pcs` daemon on all of the nodes. The command expects a list of nodes, followed by the username and password of the `pcs` user that was created during install. Listing [6-17](#394392_3_En_6_Chapter.xhtml#PC17) demonstrates how to use the command in our scenario.

```
sudo pcs cluster auth LinuxProd LinuxSyncHA LinuxConfig –u hacluster
```

Listing 6-17

Authenticate Against pcs Daemon

Because we have not specified the `–p` parameter when you run this command, you will be prompted to enter the password for `hacluster`.
