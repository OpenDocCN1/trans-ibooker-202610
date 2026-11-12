![](bbox=[0.29, 0.1, 0.7, 0.36])

The `Document` class will represent a single eBook. When thinking of a book, we can have multiple properties representing a single book, but that would be representative of all books. An example of this would be the author. All books must have an author; otherwise, they would not exist.

The properties I have added to the class are merely my interpretation of what might represent a book. Feel free to add additional code to make this your own. Open the `Document.cs` file and add the following code to the class:

```
public class Document
{
    public string Title { get; set; }
    public string FileName { get; set; }
    public string Extension { get; set; }
    public DateTime LastAccessed { get; set; }
    public DateTime Created { get; set; }
    public string FilePath { get; set; }
    public string FileSize { get; set; }
    public string ISBN { get; set; }
    public string Price { get; set; }
    public string Publisher { get; set; }
    public string Author { get; set; }
    public DateTime PublishDate { get; set; }
    public DeweyDecimal Classification { get; set; }
    public string Category { get; set; }
}
```