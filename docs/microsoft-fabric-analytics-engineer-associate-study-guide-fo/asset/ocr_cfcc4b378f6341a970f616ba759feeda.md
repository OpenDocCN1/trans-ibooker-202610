any of our other tables in the semantic model. We could add this target as a slicer and then use the following DAX to create a measure that corresponds to these targets:

```
Adjusted Target Sales =
    [Total Sales] * Target
```

This works because the disconnected table provides us with the targets, the slicer lets us select these values, and the DAX measure then dynamically changes the values in the visuals based on the input. It doesn’t matter that the table is disconnected, because you’re not trying to filter other tables directly. Instead, you’re just capturing the user’s selection and using it inside your DAX measures. This makes using disconnected tables a powerful way to add flexible, interactive features to your reports without adding unnecessary complexity or relationships to your model.

# Implementing Calculation Groups, Field Parameters, and Dynamic Format Strings

Now, let’s have a look into two Power BI features that help you build more flexible reports: calculation groups and field parameters. They let us reuse logic across multiple measures, and they allow users to switch between fields or visuals. When we know how to implement these options, we can not only increase the performance of our reports but also make them easier to understand and less complex, due to the reusable logic.

Let’s start with calculation groups.