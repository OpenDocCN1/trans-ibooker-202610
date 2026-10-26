```
Songs NVARCHAR(MAX) '$.Songs' AS JSON,
Members NVARCHAR(MAX) '$.Members' AS JSON

) a
CROSS APPLY OPENJSON(Songs)
WITH
(
SongTitle NVARCHAR(200) '$.Title',
SongAuthors NVARCHAR(200) '$.Writers'
)s;
```

This time the result meets expectations. No hidden catch! Here is the result:

| SongTitle | SongAuthors | AlbumName |
| Shine On You Crazy Diamond | Gilmour, Waters, Wright | Wish You Were Here |
| Have a Cigar | Waters | Wish You Were Here |
| Welcome to the Machine | Waters | Wish You Were Here |
| Wish You Were Here | Gilmour, Waters | Wish You Were Here |

# Import the JSON data from a file

Importing JSON data from a file and converting it into a tabular format is straightforward in SQL Server 2016. To import data from a filesystem (local disk or network location) into SQL Server, you can use the OPENROWSET (BULK) function. It simply imports the entire file contents in a single-text value.

To demonstrate this, use your knowledge from the previous section and generate content for a JSON file. Use the following query to create JSON data from the Application.People table:

```
USE WideWorldImporters;
SELECT PersonID, FullName, PhoneNumber, FaxNumber, EmailAddress, LogonName, IsEmployee, IsSalesperson FROM Application.People FOR JSON AUTO;
```

You then save the resulting JSON text in a file named app.people.json in the C:\Temp directory. Now import this JSON file into SQL Server.