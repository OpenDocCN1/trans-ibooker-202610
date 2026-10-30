6. Next, we can choose whether we want a private or public IP address for our instance:

# Connections

Choose a network path for connecting to this instance. For extra security, consider using the Cloud SQL proxy. Learn more

Private IP
Requires additional APIs and permissions, which may require your system admin. Can’t be disabled once enabled. Learn more

Public IP
Authorize a network or use Cloud SQL Proxy to connect to this instance. Learn more

# Authorized networks

You have not authorized any external networks to connect to your Cloud SQL instance. External applications can still connect to the instance through the Cloud SQL Proxy. Learn more

Figure 11.29 – Selecting a connection type

7. Provide 4 hours for a backup. Also, ensure that you check the Create failover replica option if required:

# Backups

Automated backups and point-in-time recovery

Protect your data from loss at a minimal cost. Learn more

Automate backups
Choose a window of time for your data to be automatically backed up, which may continue outside the window until complete. Time is your local time zone (UTC+1).
5:00 PM — 9:00 PM

ADVANCED OPTIONS

Enable point-in-time recovery
Allows you to recover data from a specific point in time, down to a fraction of a second. Enables binary logs (required for replication). Make sure your storage can support the days of logs you’re retaining.

ADVANCED OPTIONS

You can’t disable these settings because they’re required for replication and high availability.

Figure 11.30 – Creating a backup schedule