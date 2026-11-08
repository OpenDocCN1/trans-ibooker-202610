
If you select the `Automatic Seeding` option, then an empty database is initially created on each `Replica`. The data is then seeding using `VDI` (`Virtual Device Interface`) over the log stream transport. This option is slower than initializing with a backup but avoid transferring large backup files between shares.

**Tip**
If your `availability group` will contain many databases, then it may be best to perform the backup/restore yourself. This is because the inbuilt utility will perform the actions sequentially, and therefore, it may take a long time to complete.

On the `Validation page`, rules that may cause the setup to fail are checked. If any of the results come back as `Failed`, then you need to resolve them before you attempt to continue.

**Tip**
A common gotcha is discovered during the validation process, when you are using named instances. A requirement of `availability groups` is that the databases must reside in the same file path on all replicas. The default file location for `SQL Server` data files includes the name of the instance, however. This does not matter for default instances, because the instance name within the file path is always `MSSQLSERVER`. For named instances, however, ensure that the database files are stored in a file path that exists on all servers.

Once validation tests are complete and we move to the `Summary page`, we are presented with a list of the tasks that are to be carried out during the setup.

As setup progresses, the results of each configuration task display on the `Results page`. If any errors occur on this page, be sure to investigate them, but this does not necessarily mean that the entire `availability group` needs to be reconfigured. For example, if the creation of the `availability group listener` fails because the `VCO` had not been presented in `AD`, then you can re-create the listener without needing to re-create the entire `availability group`.

As an alternative to using the `New Availability Group wizard`, you can perform the configuration of the `availability group` using the `New Availability Group dialog box`, followed by the `Add Listener dialog box`. This method of creating an `availability group` is examined in the next section.

## Using the New Availability Group Dialog Box

Now that we have successfully created our first `availability group`, let’s create a second `availability group` for `Sales`. This `availability group` will contain the `Sales` and `Customer` databases. This time, we use the `New Availability Group` and `Add Listener` dialog boxes. We begin this process by backing up the two databases. Just like when we created the `HR` `availability group`, the databases are not selectable until we perform the backup. Unlike when we used the wizard, however, we have no way to make `SQL Server` perform the initial database synchronization using a backup/restore option. Therefore, we must either back up the database to the share that we created during the previous demonstration and then restore the backup, along with a transaction log backup, to the secondary instance, or use `Automatic Seeding`. In this example, we will use `Automatic Seeding`, so there is no need to restore the databases to the secondary `Replicas` in advance. The script in `Listing 5-4` will perform the Full backup of the databases.

**Tip**
For `Automatic Seeding` to work, the `Availability Group` must be granted the `CREATE ANY DATABASE` permission on the secondary servers.

```
--Back Up Sales Database
BACKUP DATABASE Sales TO DISK = N'C:\\Backups\\Sales.bak' WITH NAME = N'Sales-Full Database Backup';
GO
--Back Up Customers Database
BACKUP DATABASE Customers TO DISK = N'C:\\Backups\\Customers.bak' WITH NAME = N'Customers-Full Database Backup';
GO
```
`Listing 5-4`
Backing Up and Restoring the Database

If we had not already created an `availability group`, then our next job would be to create a `TCP` endpoint so the instances could communicate. We would then need to create a login for the service account on each instance and grant it the connect permissions on the endpoints. Because we can only ever have one database mirroring endpoint per instance, however, we are not required to create a new one, and obviously we have no reason to grant the service account additional privileges. Therefore, we continue by creating the `availability group`. To do this, we drill through `AlwaysOn High Availability` in `Object Explorer` and select `New Availability Group` from the context menu of `availability groups`.

This causes the `General tab` of the `New Availability Group dialog box` to display, as illustrated in `Figure 5-9`. On this screen, we type the name of the `availability group` in the first field. Then, we click the Add button under the `Availability Databases` window before we type the name of the database that we wish to add to the group. We then need to click the Add button under the `Availability Replicas` window before we type the `server\\instance name` of the secondary replica in the new row. We have set `Required Synchronized Secondaries to Commit` to `1`, however. This setting, which was introduced in `SQL Server 2017`, guarantees that the specified number of secondary replicas writes the transaction data to log before the primary replica commits each transaction. In our scenario, where we only have a single synchronous secondary, that in the event of a failure on the `Primary Replica`, failover will happen automatically, but the `Secondary Replica` will not allow user transactions to be written to the database, until the original `Primary Replica` comes back online. This absolutely guarantees that there can be no data loss in any circumstances. If we had left this setting as `0` (as we did in the first example in this chapter), then in the event that the `Primary Replica` failed and users wrote transactions to the `Secondary Replica`, before this `Replica` also failed, then data loss could occur, as the other `Replicas` are configured in `asynchronous commit` mode.

`Figure 5-9 ¶ The New Availability Group Dialog Box`

Now we can begin to set the replica properties. We discussed the `Role`, `Availability Mode`, `Failover Mode`, `Readable Secondary`, and `Endpoint URL` properties when we created the `HR` `availability group`. The `Connection in Primary Role` property defines what connections can be made to the replica if the replica is in the primary role. You can configure this as either `Allow All Connections` or `Allow Read/Write connections`. When `Read/Write` is specified, applications using the `Application Intent = Read only` parameter in their connection string will not be able to connect to the replica.

The `Session Timeout` property sets how long the replicas can go without receiving a ping from one another before they enter the `DISCONNECTED` state and the session ends. Although it is possible to set this value to as low as `5` seconds, it is usually a good idea to keep the setting at `60` seconds; otherwise, you run the risk of a false-positive response, resulting in unnecessary failover. If a replica times out, it needs to be resynchronized, since transactions on the primary will no longer wait for the secondary, even if the secondary is running in `Synchronous Commit` mode.

On the `Backup Preferences tab` of the dialog box, we define the preferred replica to use for automated backup jobs, as shown in `Figure 5-10`. Just like when using the wizard, we can specify `Primary`, or we can choose between enforcing and preferring backups to occur on a secondary replica. We can also configure a weight, between `0` and `100` for each replica, and use the `Exclude Replica` check box to avoid backups being taken on a specific node.

`Figure 5-10 ¶ The Backup Preferences Tab`
