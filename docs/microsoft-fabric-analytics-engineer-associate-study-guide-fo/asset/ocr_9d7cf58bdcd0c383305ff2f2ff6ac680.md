Notice that the EpisodeId column is now sorted in the default descending (desc) order. To change that, add asc to the end of the line.

# Getting a Column Subset of Data

To select a column subset of the data, you have two functions to choose from:

+   - project
+   - project-away

The project function actively selects the columns you want in your results set. Here’s an example of how you can use it:

```
Weather
| project StartTime, EpisodeId, EventType
```

This example script returns only the listed columns: StartTime, EpisodeId, and EventType. Figure 4-26 shows a subset of the results.