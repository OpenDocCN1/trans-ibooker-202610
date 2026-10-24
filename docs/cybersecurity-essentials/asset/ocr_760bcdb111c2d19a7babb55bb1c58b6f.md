+   - Removing or stopping unnecessary services, applications and protocols.
+     - File and print-sharing services functions
+     - Wireless networking services
+     - Directory services
+     - Email services
+   - Closing any open TCP/UDP network ports.

Each step requires the administrator to balance security requirements with users’ needs. Ideally, the server would be locked down tight and serve a single function. However, as security increases, functionality decreases.

# Logical Server Access Control

The network administrator is also responsible for determining who gains access to the operation of the network’s servers, network connectivity devices, and data. In most organizations, they implement the organization’s policy statements through the network operating system’s management tools that control “who” has access to the network and its resources and what they can do with them.

Three standard types of network access control strategies are available to administrators:

**Mandatory Access Control (MAC)** The system establishes which users or groups may access files, folders, and other resources.

**Discretionary Access Control (DAC)** The user has the discretion to decide who has access to their objects and to what extent.

**Nondiscretionary, Role-Based Access Control (RBAC)** Control is determined by the job roles each user has within the organization.

MAC strategies assign sensitivity labels to network objects such as files and folders and granting access to the users based on their permission level, as illustrated in Figure 14.4.

As the figure shows, each user’s range of access depends on the clearance level they have been given by their administrators. Personnel with a Top Secret clearance level have access to all objects that have been assigned a Top Secret label. In addition, they are permitted to access all objects labeled with a lesser label—such as Secret and Classified objects.