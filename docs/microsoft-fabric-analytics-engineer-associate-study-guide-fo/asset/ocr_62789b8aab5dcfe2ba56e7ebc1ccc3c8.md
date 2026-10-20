This is the eventhouse’s name.

*DP-600-Eventhouse (item 2)*

This is the KQL database’s name.

*DP-600-Eventhouse_queryset (item 3)*

This is the KQL querysets.

As you can see, the item names are reused across the eventhouse and the first KQL database, which is created in the same step in which you create the eventhouse. Also, if you wish, you can create a new KQL database in the eventhouse and delete the default one so you can have a different name for the KQL database. The eventhouse acts as a form of server for your KQL databases.

> **NOTE**
> The KQL database name can contain alphanumeric characters, underscores, periods, and hyphens. Special characters aren’t supported.

Along with the autocreation of a KQL database comes the autocreation of a KQL querysets within each database (item 3 in Figure 4-17).

# Creating a Standalone KQL Queryset

The second option is to create a KQL querysets as a standalone item in your Fabric workspace. If you click the “+ New item” button in the workspace menu, you’ll find KQL Queryset as a new item on the list (see Figure 4-18).