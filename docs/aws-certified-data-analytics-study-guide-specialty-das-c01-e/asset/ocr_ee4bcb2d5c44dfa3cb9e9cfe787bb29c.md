+   - HOP – Log the action and move the query to another appropriate queue if it exists; otherwise, terminate the query.
+   - PRIORITY – Change the query priority.

# Security in Redshift

Redshift host’s your company’s most valuable resource, data, and hence security becomes a day zero job. Access to Amazon Redshift is controlled at four levels:

Cluster Management – AWS IAM (Identity and Access Management) is used to control the AWS user’s ability to create, configure, and delete Amazon Redshift clusters. Once users have the ability to access a Redshift cluster via IAM, they will connect via JDBC/ ODBC or the query editor in the Redshift console.

Cluster Connectivity – Security groups are used to allow other AWS instances to connect to the Redshift cluster.

Database Access – Access to database objects such as tables and views is controlled by user accounts within the Amazon Redshift database.

Temporary DB Credentials and Single Sign-on – Leverage IAM credentials for logging onto the DB or to sign on using federated Single Sign-On (SSO) through a SAML 2.0– compliant identity provider.

# Redshift Cluster VPCs

Compute nodes reside in a private, Amazon-managed, internal VPC that is only accessible by the Redshift leader node. The leader node resides in both the private/internal VPC for compute nodes and the customer’s VPC to allow access to the Redshift cluster and to handle data distribution among the compute nodes. High-bandwidth connections, close proximity, and custom communication protocols provide private, very high-speed network communication between the leader node and compute nodes. Compute nodes interact with other AWS services such as S3, EMR, and DynamoDB for ingestion, backup, and so on.

Figure 4.22 indicates Redshift VPCs and their interaction with other Amazon Web Services.

# Redshift Network Topology and Security Groups

Cluster subnet groups indicate which subnets within a VPC a Redshift cluster can run; however, most customers configure Redshift with multiple subnets so that Redshift can choose the subnet. An Amazon Redshift cluster exists in only one subnet/AZ at a time.

Amazon Redshift clusters are locked down by default, which means no users can connect to the cluster. To grant users access to the cluster, a security group must be associated with the cluster that opens up the appropriate ports and accepts incoming connections from the white-listed IP addresses, CIDR block ranges, or security groups.