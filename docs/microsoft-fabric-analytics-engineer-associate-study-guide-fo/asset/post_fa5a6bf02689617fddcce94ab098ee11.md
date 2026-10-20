
you have a clear understanding of your user roles and data. The management of this security needs to be part of your overall governance strategy.

## Object-Level Security in a Warehouse

Object-level security is about controlling access to entire data objects. This is the broadest level of security you can implement in a warehouse, and you handle it with the same GRANT and DENY statements you use in column-level security. But while column-level security is about a specific column, object-level security is about the whole object, like a table, a view, or even a schema.

The most basic object-level security scenario is giving a user or a group access to a table. If they don’t have this permission, they can’t even know whether the table exists. Implementing object-level security is a simple and effective way to manage broad access rights. You can grant different permissions, such as SELECT, INSERT, UPDATE, or DELETE, to give yourself very granular control over what a user can do with an entire object.

Here’s a code example showing how to grant and deny access to an entire table:

```
-- Grant SELECT permission on the FactSales table to a role
GRANT SELECT ON FactSales TO SalesTeamRole;
-- Deny all permissions on the sensitive Customers table to a user
DENY ALL ON Customer.DimCustomer
```

The approach is simple but powerful. By combining object-level, column-level, and row-level security, you can build a very robust security model that can fit almost any scenario. It is a bit of a burden to manage, but it gives you complete control over your data.

## Lakehouse Security

In the Microsoft Fabric ecosystem, not all security is handled at the SQL or semantic model layer. A core part of the platform is the lakehouse, which sits on top of OneLake.

Here, the data is just files, plain-old files in folders. This is where you’ll do your data engineering, load raw data, and transform it before it ever sees a SQL endpoint. And because it’s just files, you handle the security at a much lower level.

The security model here is all about access to files and folders, and it’s a lot simpler than the complex T-SQL statements we’ve been looking at. You’re not defining complex predicates or GRANT statements for individual columns. Instead, you’re granting permissions at the file or folder level.

You manage this security through the Fabric user interface by assigning permissions to users or groups. The permissions themselves are pretty straightforward and are built around the three core rights:

### Read

With this permission, a user can see a file or folder and read its contents. They can’t make any changes, rename anything, or delete anything. This is the simplest form of access and is often used for consumers who only need to read a dataset.

### Read and write

This permission gives a user the ability to not just read the data but also to create new files, modify existing ones, or delete them. It’s a key permission for data engineers and developers who are responsible for maintaining the data in the lakehouse.

### Read, write, and execute

The term *execute* might seem odd when we’re talking about files, but think of it as *full control*. A user with this permission can do everything a read and write user can do, but they also have permission to execute other commands like managing permissions on the files. This is often reserved for administrators or owners of the data.

You assign these permissions directly in the lakehouse files and folders, and you’ll typically manage this through the main Fabric portal. You can find the permissions dialog for any file or folder in the UI, and it’s a quick process of assigning users and their roles.

> **NOTE**
> It’s important to remember that these permissions are different from the workspace roles (administrator, member, contributor, and viewer). While a workspace role provides overall access to the items in a workspace, file-level access controls what a user can actually do with the files inside the lakehouse. A user with a viewer role in a workspace, for example, might be given “Read and write” access to a specific folder in the lakehouse, allowing them to load data but not modify other items in the workspace.

You manage the permissions directly on the objects. Simply navigate to the object (like a folder) in the lakehouse explorer and set the permissions. This contrasts with the semantic model, where security is defined by roles and is a part of the model itself. In the lakehouse, the permissions are a separate layer that sits on top of the files. The two layers of security work together to give you a very robust model, but they’re completely independent of each other.

## Applying Sensitivity Labels to Items

While row, column, and object security are all about controlling who can see what data, there’s a whole other layer of security that deals with the data itself, no matter where it is. This is where *sensitivity labels* come into the picture. Think of sensitivity labels as a way to classify your data and apply a set of rules and policies to it.

The concept is simple: you label data based on how sensitive it is, and then, the label enforces protection settings. It’s about data governance and ensuring that sensitive information is treated correctly throughout its entire lifecycle. This isn’t just a Fabric-only thing; these labels are part of a broader platform called Microsoft Purview Information Protection, which means the labels you use in Fabric are the same ones your organization uses in SharePoint, Teams, and Power BI. The goal is to have a consistent and unified approach to data classification.

### The Different Sensitivity Labels

The labels you can apply aren’t on a fixed list from Microsoft. They are defined by your organization’s governance team, and they reflect your company’s specific needs for data classification. A typical set of labels might look something like this:

*Public*

This is for data that can be shared freely, such as a public-facing report or a dataset that has no sensitive information.

*General*

This is the default label for most internal data. It’s for everyday business data that isn’t highly sensitive but shouldn’t be shared with external parties.

### Confidential

This is for data that contains information that is not for general consumption, such as internal financial reports and personal data.

### Highly confidential

This is for your most sensitive data, such as trade secrets, employee records, and business acquisition plans. This label often comes with strict protection rules.

Some of these labels may even have sublabels. For example, a Confidential label may have sublabels like Confidential - Finance or Confidential - HR that allow for even more granular classification without creating a long list of top-level labels. The key here is that the labels are tools a developer can use to tag data, and the rules are then enforced by the system.

### How to Use Them

The process of applying a sensitivity label is very straightforward, and it’s a fundamental step that a developer should perform whenever they create a new item. For example, when you create a new semantic model in your workspace, there will be a drop-down box in the properties where you can choose a sensitivity label. It’s a simple, manual process.

The magic happens behind the scenes. Once you label an item, that label “cascades” down to all connected items. If you create a semantic model and label it Confidential, any reports you build on top of that semantic model will automatically inherit the Confidential label. This is a critical point. A developer might not even need to think about applying a label to a report; it’s inherited from the underlying data source.

This inheritance is what makes sensitivity labels so powerful for governance. If an organization has a policy that a Confidential report can’t be exported to a PDF file or printed, Fabric will enforce that rule for all items tagged with that label. The developer’s job is to apply the correct label, and the platform will take care of the rest. This creates a very robust governance model that is hard to circumvent.

> **NOTE**
> 
> Sensitivity labels are all about classification and governance. They do not replace row-level or column-level security. A label can enforce a policy that prevents a user from exporting a report, but it will not filter the data that the user can see. The two layers of security work together: one controls who can see what, and the other controls how the data can be used.

## Endorsing Items

In an organization where everyone is creating reports, datasets, and other items, it can quickly become hard to figure out what’s what. How do you know if a specific report is the “official” one you should be using for your business decisions? How do you know it’s accurate and trustworthy? This is where endorsements come in.

*Endorsements* provide a way to signal the quality and reliability of an item in Fabric. They are simple visual cues that tell users this specific report, semantic model, or other artifact has been vetted and approved. Think of them as stamps of approval from the person or team who owns the
