# Instance info

Instance ID *
cloudarchitect001-replica
Use lowercase letters, numbers, and hyphens. Start with a letter.

Database version
PostgreSQL 9.6

# Choose region and zonal availability

For better performance, keep your data close to the services that need it. Region is permanent, while zone can be changed any time.

Region
us-central1 (Iowa)

Single zone
In case of outage, no failover. Not recommended for production.

Multiple zones (Highly available)
Automatic failover to another zone within your selected region. Recommended for production instances. Increases cost.

Figure 11.33 – Creating a replica

2. Once the read replica has been created, we will see the following in our normal instance view:

![](bbox=[0.22, 0.6, 0.75, 0.81])

Figure 11.34 – Viewing the read replica

In this section, we looked at read replicas and the various options available. In the next section, we will look at failover replicas.