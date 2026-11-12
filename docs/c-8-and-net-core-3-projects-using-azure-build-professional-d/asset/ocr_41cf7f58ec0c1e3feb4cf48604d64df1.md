Before getting the properties of the specific file, the tuple is initialized by doing the following:

```
var returnTuple = (created: DateTime.MinValue, lastDateAccessed:
DateTime.MinValue, name: "", ext: "", fileSize: 0L, error: false);
```

If there is an exception, I can return default values. Reading the file properties is simple enough using the FileInfo class. I can then assign the file properties to the tuple by doing this:

```
returnTuple = (fi.CreationTime, fi.LastAccessTime, fi.Name, fi.Extension,
fi.Length, false);
```

The tuple is then returned to the calling code, where it will be used as required. We will have a look at the calling code next.

# The ImportBooks form

The ImportBooks form does exactly what the name suggests. It allows us to create virtual storage spaces and to import books into those spaces. The form design is as follows:

![](bbox=[0.15, 0.488, 0.85, 0.862])