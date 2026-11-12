Your Views folder should now look as follows:

![](bbox=[0.315, 0.145, 0.68, 0.565])

+   8. Now, we need to make a small change to our IWorkItemService interface. Modify the code in the interface to look as follows:

```
public interface IWorkItemService
{
    IEnumerable<WorkItem> GetAllWorkItems();
    void InsertWorkItem(WorkItem workItem);
}
```