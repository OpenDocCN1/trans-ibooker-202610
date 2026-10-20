
# Dataflow Gen1

Dataflow Gen1 is a UI-assisted approach that uses the Power Query language to read, transform, and save data in Fabric. Dataflow Gen1 consists of an ingestion engine with almost the same capabilities as the Power BI Desktop. It uses Power Query, and the destination resides inside the dataflow. It also uses a storage account behind the scenes, and you can configure this storage account to be an explicit one that you manage and create yourself from the Azure portal.

When you’re creating new items, you’ll find Dataflow Gen1 in the Get data and Prepare data sections of the Fabric menu (see Figure 2-3).

When you start to create a Dataflow Gen1 in Fabric, you’ll see a screen asking you if you really want a Dataflow Gen1 and not a Dataflow Gen2 (see Figure 2-4).

## New item

### Get data

Ingest batch and real-time data into a single location within your Fabric workspace.

### Dataflow Gen1

Prep, clean, and transform data.

### Dataflow Gen2

Prep, clean, and transform data.

### Prepare data

Clean, transform, extract, and load your data for analysis and modeling tasks.

### Dataflow Gen1

Prep, clean, and transform data.

### Dataflow Gen2

Prep, clean, and transform data.

Figure 2-3. Fabric menu item for Dataflow Gen1

## Want to create a Dataflow Gen2 instead?

Dataflow Gen2 significantly enhances the data ingestion and transformation capabilities available in Dataflow Gen1, with new features such as Output Destinations, Copilot, Fast Copy (for large-scale data ingestion), a new High-Scale Dataflows Transformation engine, VNET Gateway support, enhanced Refresh History and Diagnostics experiences and more.

Learn more

Don't show this again, always create a Dataflow Gen1

Yes, create a Dataflow Gen2

No, create a Dataflow Gen1

*Figure 2-4. Splash screen when you start to create a Dataflow Gen1*

Even though there’s a newer version of Dataflow with more capabilities (as seen in Figure 2-4), we’ll show you in what situations Gen1 can still be useful. Then, we’ll detail the additional features of Dataflow Gen2.

Dataflow Gen1 is useful when you need to ingest data directly from an external source but can’t use the shortcut method as described previously. This is because Dataflow Gen1 has a special capability to read data in direct query mode from the source and present it to the end user or application, which Dataflow Gen2 doesn’t have. And if the shortcut feature doesn’t work or doesn’t apply to the scenario, then Dataflow Gen1 is a good choice.

In Table 2-2, you can see the differences between Dataflow Gen1 and Dataflow Gen2.

Table 2-2. Differences between Dataflow Gen1 and Dataflow Gen2

| Features | Dataflow Gen1 | Dataflow Gen2 |
|---|---|---|
| Author dataflows with Power Query | Yes | Yes |
| Shorter authoring flow | No | Yes |
| Autosave and background publishing | No | Yes |
| Data destinations | No | Yes |
| Improved monitoring and refresh history | No | Yes |
| Integration with data pipelines | No | Yes |
| High-scale compute | No | Yes |
| Getting data via Dataflows connector | Yes | Yes |
| Direct query via Dataflows connector | Yes | No |
| Incremental refresh | Yes | Yes |

If you click the No, create a Dataflow Gen1 button on the screen shown in Figure 2-4, it will bring up the screen in Figure 2-5.

There, you have the following four options:

- Defining new tables
- Linking tables from other dataflows
- Importing a model
- Attaching a Common Data Model folder (preview)

# NOTE

Note that the fourth option, Attach a Common Data Model folder, is in preview. It’s been in preview for quite some time now, and because Dataflow Gen2 has been introduced, we’re not sure if it will ever come out of preview. If it’s still in preview when you take the exam, you won’t be given any questions on it.

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_62_0.png)

Figure 2-5. Welcome screen for Dataflow Gen1

## Defining new tables

If you click the option to define new tables, you’ll see the well-known UI for Power Query, and you’ll be able to continue with the sources listed in Figure 2-6.

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_63_0.png)

We’ve highlighted some specific areas (numbered 1, 2 and 3):

### View more (area 1)

You can click this option to view the entire selection of possible sources you can use. This list keeps expanding as Microsoft expands the options in Power BI and Fabric.

### Upload file (area 2)

You can use this interface to upload a single file and use it as the source for your dataflow.

### x (area 3)

We’ve highlighted this because it can be hard to find. You click it to close the UI if you don’t want to save your work.

Once you’ve selected the source you want to use, you can connect to that source on the provided screen.

> **TIP**
> 
> Each source has its own settings, so we won’t dive into each of them here. It’s also outside of the scope of the certification to know the network setup and the connectivity of the sources.

Next, we’ll dive into the screen for data manipulation (see Figure 2-7).

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_64_0.png)

Figure 2-7. The Power Query UI in Dataflow Gen1

We’ve highlighted four areas of interest (numbered 1–4).

### Menu bar (area 1)

This is almost the same menu bar you’ll find in the Power BI Desktop version of Power Query (see later in this chapter). Here, you can select Power Query, Transform, Add column, View, and Help.

### List of queries (area 2)

Here, you can find all the queries you have in the dataflow.
Each dataflow can contain more than one query, and the end user of the dataflow will see them all. Then, the end user can connect to the item in Fabric.

### Data preview (area 3)

In the data preview pane, you can see the current state of the data as a preview of the first 99 rows, and you can also scroll down with your mouse to see more rows. Here, you can modify your data by right clicking on a cell to replace values and right clicking on a column to execute any command shown in Figure 2-8.

Figure 2-8. Complete context menu retrieved by right-clicking on a column in Dataflow Gen1

### Query settings (area 4)

Every time you make changes to the data in the Power Query view, either with your mouse or through the formula

bar, you’ll see a new applied step in the “Query settings” area at the far right of the screen. You’ll see the source you’re connected to, the steps you’ve been creating, and the name of the table you’re working with. This view also lets you click on any step and see the preview of that step in the preview pane, and that in turn enables you to work dynamically with your needs for data manipulation. It can also help you with debugging and further development of the data.

From some of the context menu items, you can also access a drop-down list of more options to choose from, and you can click each option to see details on it. For example, Figure 2-9 shows the details on the Split column action.

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_67_0.png)

Figure 2-9. Details on Split column action

If you want to do your own coding and not use your mouse, you can write the M scripts you need in the formula bar just above the data preview.

NOTE
In this book, we won’t dive into the details of the M language. If you want to read more about it, you can find a complete reference guide on Microsoft Learn.

## Linking tables from other dataflows

The “Link tables from other dataflows” section gives you the option to read data from already existing dataflows and use them as the source for your dataflow. This can be useful if you have some data you want to use and you also want to add more reference data, clean it, or aggregate it.

When you select this option, it will bring up the screen in Figure 2-10.

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_68_0.png)

Figure 2-10. Screen for connecting to tables from other dataflows

Notice that the screen indicates you are signed in. This also means that you as the developer need to have access to the dataflow to be able to read it.

## Importing a model

The Import Model option gives you the ability to import a predefined dataflow model into your dataflow and use it as your starting point. The format for Dataflow Gen1 is JSON, and you can get a copy of a dataflow by exporting it directly from the UI on an existing item (see Figure 2-11).

![](img/8e59e0a64e4cc0c26d8e7f463c62aaad_69_0.png)

Figure 2-11. Exporting an existing Dataflow Gen1

This concludes the run-through of Dataflow Gen1. Next up is Dataflow Gen2.
