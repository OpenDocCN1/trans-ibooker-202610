```
(
    AlbumName NVARCHAR(50) '$.Album',
    AlbumYear SMALLINT '$.Year',
    IsVinyl    BIT '$.IsVinyl'
);
```

The result of the previous action is a table defined with the WITH statement:

| AlbumName | AlbumYear | IsVinyl |
|---|---|---|
| Wish You Were Here | 1975 | 1 |

You can add a fourth column to show band members. Here is the code:

```
SELECT * FROM OPENJSON(@json)
WITH
(
    AlbumName NVARCHAR(50) '$.Album',
    AlbumYear SMALLINT '$.Year',
    IsVinyl   BIT '$.IsVinyl',
    Members   VARCHAR(200) '$.Members'
);
```

Here is the resultant table:

| AlbumName | AlbumYear | IsVinyl | Members |
|---|---|---|---|
| Wish You Were Here | 1975 | 1 | NULL |

The result might be unexpected, but the value for the Members property is an object, and therefore the function returns NULL since the JSON path is in default lax mode. If you specified the strict mode, the returned table would be empty and an error would be raised. To solve the problem and show the value of the Members property you need to use the AS JSON option to inform SQL Server that the expected data is properly JSON-formatted, as shown in the following code:

```
SELECT * FROM OPENJSON(@json)
WITH
(
    AlbumName NVARCHAR(50) '$.Album',
    AlbumYear SMALLINT '$.Year',
    IsVinyl   BIT '$.IsVinyl',
    Members   VARCHAR(MAX) '$.Members' AS JSON
);
```