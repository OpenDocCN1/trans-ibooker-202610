```
// SQL Server database provider-generated code
[StringLength(15)]
public string CategoryName { get; set; } = null!;
```

Neither database provider will mark non-nullable string properties as required:

```
// no runtime validation of non-nullable property
public string CategoryName { get; set; } = null!;
// nullable property
public string? Description { get; set; }

// decorate with attribute to perform runtime validation
[Required]
public string CategoryName { get; set; } = null!;
```

We will make some small changes to improve the entity model mapping and validation rules for SQLite:

+   1. Open the Customer.cs file and add a regular expression to validate its primary key value to only allow uppercase Western characters, as shown highlighted in the following code:
+   2. Activate your code editor's find and replace feature (in Visual Studio 2022, navigate to Edit | Find and Replace | Quick Replace), toggle on Use Regular Expressions, and then type a regular expression in the search box, as shown in the following expression:
+   3. In the replace box, type a replacement regular expression, as shown in the following expression:
+   4. Set the find and replace to search files in the current project.
+   5. Execute the search and replace to replace all, as shown in Figure 13.4:

```
[Key]
[Column(TypeName = "nchar (5)")]
[RegularExpression("[A-Z]{5}")]
public string CustomerId { get; set; }
```

```
\[Column\(TypeName = "(nchar|nvarchar) \((.*)\)"\)\]
```

```
$&\n    [StringLength($2)]
```

> After the newline character, \n, I have included four space characters to indent correctly on my system, which uses two space characters per indentation level. You can insert as many as you wish.