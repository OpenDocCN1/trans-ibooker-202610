+   - Password Policies
+   - Anti-Phishing Protection
+   - Data Loss Prevention Controls

# Storage

It is vital that the data held in cloud storage is highly available, and that only those who are authorized can access the data. Let's look at permissions, encryption, replication, and high availability:

+   - **Permissions:** Users have a storage identity and are put into different storage groups that have different rights.
+   - **Encryption:** With cloud storage, you may need to have more than one type of encryption. You would use symmetric encryption as there will be a large amount of data; normally, AES-256 will be used. Microsoft uses RSA 2048 for blob storage. You will also need encryption for data in transit, such as TLS or SSL. Microsoft uses **Transport Data Encryption (TDE)** to access resources in the cloud using RSA 2048 or 3072.
+   - **Replication:** Replication is a method wherein data is copied from one location to another immediately. In the cloud, multiple copies of your data are always held for redundancy. This is in case there are power or hardware failures or environmental disasters such as hurricanes. The data cannot be located outside of the region where it is created. There are four different types of replication:
+     a. **Local Redundant Storage (LRS):** Three copies of your data are replicated at a single physical location. Not good for high availability. It is the cheapest solution, but if the power goes then everything has gone.
+     b. **Zone Redundant Storage (ZRS):** Data is replicated between three separate zones within your region. It should be used in your primary region; however, if a disaster affects the region, then you have no access to data.
+     c. **GEO Redundant Storage (GRS):** Three copies of your data are replicated in a single physical location in the primary region using LRS, then one copy is replicated to a single location in a secondary region.
+     d. **GEO Zone Redundant Storage (GZRS):** Data is replicated between three separate zones within your primary region, then one copy is replicated to a single location in a secondary region.