# Adding work items

Let's add work items by going through the following steps:

+   1. To add work items, let's start off by adding a class to our Models folder called AddWorkItem, as shown in the following screenshot:

![](bbox=[0.305, 0.235, 0.69, 0.68])

+   2. Modify the code in the class to essentially look like the WorkItem class:

```
public class AddWorkItem
{
    public string Title { get; set; }
    public string Description { get; set; }
    public int Severity { get; set; }
    public string WorkItemType { get; set; }
    public string AssignedTo { get; set; }
}
```