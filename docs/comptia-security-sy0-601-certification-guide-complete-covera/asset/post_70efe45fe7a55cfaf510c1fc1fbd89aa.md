
Microsoft produced a JEA toolkit using Windows PowerShell so that each type of admin has a certain number of privileges for the tasks that they are allowed to carry out.

The company will use an identity management solution such as Identity Manager that holds the different policies for each type of privileged account.

**Example:** Fred is the SQL database administrator who wants to complete an update to the SQL database. In the following diagram, you can see that he first of all logs in to **Identity Manager (IM)** using **Multi-Factor Authentication (MFA)**. IM looks at the policies that it holds relating to Fred's account and contacts the bastion domain that produced an admin Kerberos ticket that is then used to give Fred admin access to the SQL database to perform admin tasks, see Figure 3.5 below:

![](img/adfe46a7d545f23d50e4108d31e60de2_102_0.png)

Figure 3.5 – Obtaining admin privileges

Admin privileges cannot be obtained for day-to-day tasks such as accessing the internet or sending emails.

## Mandatory Access Control

**Mandatory Access Control (MAC)** is based on the classification level of the data.

MAC looks at how much damage they could cause to the interest of the nation. These are as follows:

- **Top Secret**: Highest level, exceptionally grave damage
- **Secret**: Causes serious damage
- **Confidential**: Causes damage
- **Restricted**: Undesirable effects

Examples of **Mandatory Access Control (MAC)** are as follows:

| Data types | Classification |
|---|---|
| Nuclear energy project | Top secret |
| Research and development | Secret |
| Ongoing legal issues | Confidential |
| Government payroll | Restricted |

There are different roles associated with classifying, storing, and giving access to data, and these are as follows:

- **Owner**: This is the person who writes the data and determines the classification. For example, if they are writing a secret document, they will pitch it at that level, no higher.
- **Steward**: The steward is the person who is responsible for the quality and labeling of the data.
- **Custodian**: The custodian is the person who stores, manages, and protects the classified data. They will probably encrypt the data to protect it.
- **Security Administrator**: The security administrator is the person who gives access to classified data once clearance has been approved.

## Discretionary Access Control

Discretionary access control involves **New Technology File System (NTFS)** file permissions, which are used in Microsoft operating systems. The user is only given the access that is required to perform their job. These could be classified as user-based or user-centric.

These permissions are shown in the following screenshot:

![](img/adfe46a7d545f23d50e4108d31e60de2_104_0.png)

Figure 3.6 – DAC permissions

The permissions are as follows:

- **Full control**: Full access.
- **Modify**: Change data, read, and read and execute.
- **Read & execute**: Read the file and run a program if one is inside it.
- **List folder contents**: Expand a folder to see the subfolders inside it.
- **Read**: Read the contents.
- **Write**: This allows you to write to the file.
- **Special permissions**: Allows granular access; for example, it breaks each of the previous permissions down to a more granular level.
- **Data Creator/Owner**: The person that creates the unclassified data is called the owner and they are responsible for checking who has access to that data.

The highest would be 777 as the owner, group, and other all have full control. The lowest would be 644 as the owner would have read and write, the group would be read, and the other would also be read.

Another way the permissions can be set is by alphabetical values, as shown:

- a. R: Read
- b. W: Write
- c. X: Execute

When using alphabetical values, each set of permissions is shown as three dashes.

Full control for the three entities is as follows:

- Owner full control would be shown as rwx --- ---.
- Group full control --- rwx ---.
- User full control --- --- rwx.

You can also change permissions in Linux. If the permission to File C is 654 and we wish to change these permissions to give full control, we will run the chmod 777 File C command, which changes the permissions to File C.

## Role-Based Access Control

This is a subset of duties within a department. An example would be two people within the finance department who only handle the petty cash. In IT terms, it could be that only two of the IT team administer the email server.

## Rule-Based Access Control

In Rule-Based Access Control (RBAC), a rule is applied to all of the people within a department, for example, contractors will only have access between 8 a.m. and 5 p.m., and the help desk people will only be able to access Building 1, where their place of work is. It can be time-based or have some sort of restriction, but it applies to the whole department.

## Attribute-Based Access Control

In Attribute-Based Access Control (ABAC), access is restricted based on an attribute in the account. John could be an executive and some data could be restricted to only those with the executive attribute. This is a user attribute from the directory services, such as a department or a location. You may wish to give different levels of control to different departments.

## Group-Based Access

When a company has a large number of users, it is difficult to give each user access to the resources that they need to perform their job. Groups are created, and they will contain all users in that department. For example, the sales group will then contain all of the people working in sales, and the group will be used to allow access to resources such as a printer or file structure. If you decide to use group-based access control and you have new employees or interns, you may create another group for them with lower permissions.

For example, in a large corporation, 25 employees work in marketing and require full access to the marketing file share. Next week, they will have three new interns start with the company, but they need only read access to the same share. Therefore, the following steps need to be taken:

- A global group called **marketing** is created. All 25 employees are added to the group. The group is given full control access to the data.
- A global group called **marketing interns** is created. The three interns are added to the group. The group is given read access to the data.

**Example:** Two people worked in **Information Technology (IT)** who needed access to the IT data. These people are called Bill and Ben, and we add them to the IT Group that in turn is given full control access to the IT Data folder.

![](img/adfe46a7d545f23d50e4108d31e60de2_107_0.png)

Figure 3.7 – Group-based access

## Summarizing Authentication and Authorization Design Concepts

We are going to look at authentication and authorization design concepts that are used by corporate environments. We are going to look at directory services, federation services, biometrics, and multifactor authentication. Let's look at each of these in turn.

The way it is stored in the Active Directory can be viewed using a tool called **ADSI Edit**:

![](img/adfe46a7d545f23d50e4108d31e60de2_109_0.png)

Figure 3.9 – ADSI Edit

LDAP is the Active Directory protocol responsible for storing the X.500 objects. When the Active Directory is searched, then LDAP provides the information required. LDAPS is the secure version of LDAP.

Here are some examples.

- **Example 1:** If I want to know how many people are in the IT OU, I can search the Active Directory. LDAP provides the search and returns a reply saying that the IT department has 10 members.
- **Example 2:** I am searching the Active Directory for a user called Fred. Once again, LDAP finds the user. If you have 10,000 people in your domain, you will have them in different OUs to make it easier to find and manage them. However, if you need to find someone, this will still be difficult. That is why we need LDAP to perform the search. It saves time.

## Kerberos

Kerberos authentication uses tickets, **Updated Sequence Numbers (USN)**, and is time-stamped. The process of obtaining your service ticket is called a **Ticket Granting Ticket (TGT)** session. The times on all servers and computers must be within 5 minutes of each other; time can be synchronized by using a time source such as the Atomic Time clock.
