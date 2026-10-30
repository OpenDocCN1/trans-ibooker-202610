# Overloading Indexer Methods

Indexer methods may be overloaded on a single class or structure. Thus, if it makes sense to allow the caller to access subitems using a numerical index or a string value, you might define multiple indexers for a single type. By way of example, in ADO.NET (.NET’s native database-access API), the DataSet class supports a property named Tables, which returns to you a strongly typed DataTableCollection type. As it turns out, DataTableCollection defines three indexers to get and set DataTable objects—one by ordinal position and the others by a friendly string moniker and optional containing namespace, as shown here:

```
public sealed class DataTableCollection : InternalDataCollectionBase
{
...
    // Overloaded indexers!
    public DataTable this[int index] { get; }
    public DataTable this[string name] { get; }
    public DataTable this[string name, string tableNamespace] { get; }
}
```

It is common for types in the base class libraries to support indexer methods. So be aware, even if your current project does not require you to build custom indexers for your classes and structures, that many types already support this syntax.

# Indexers with Multiple Dimensions

You can also create an indexer method that takes multiple parameters. Assume you have a custom collection that stores subitems in a 2D array. If this is the case, you may define an indexer method as follows:

```
public class SomeContainer
{
    private int[,] my2DintArray = new int[10, 10];

    public int this[int row, int column]
    { /* get or set value from 2D array */ }
}
```

Again, unless you are building a highly stylized custom collection class, you won’t have much need to build a multidimensional indexer. Still, once again ADO.NET showcases how useful this construct can be. The ADO.NET DataTable is essentially a collection of rows and columns, much like a piece of graph paper or the general structure of a Microsoft Excel spreadsheet.

While DataTable objects are typically populated on your behalf using a related “data adapter,” the following code illustrates how to manually create an in-memory DataTable containing three columns (for the first name, last name, and age of each record). Notice how once you have added a single row to the DataTable, you use a multidimensional indexer to drill into each column of the first (and only) row. (If you are following along, you’ll need to import the System.Data namespace into your code file.)

```
static void MultiIndexerWithDataTable()
{
    // Make a simple DataTable with 3 columns.
    DataTable myTable = new DataTable();
    myTable.Columns.Add(new DataColumn("FirstName"));
```