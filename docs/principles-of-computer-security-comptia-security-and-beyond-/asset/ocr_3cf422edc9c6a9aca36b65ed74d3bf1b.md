k. See all the databases:

```
' UNION SELECT SCHEMA_NAME, NULL FROM information_schema.schemata#
```

information_schema is a database of views (virtual tables that have contents defined dynamically through queries) that give metadata information about all tables, views, columns, and procedures in an RDBMS. The name of the table queried is schemata.

l. See all the tables:

```
' UNION SELECT TABLE_NAME, NULL FROM information_schema.tables#
```

The output on the DVWA page cuts off at a certain point, but if you entered the full statement in the MariaDB console, there would be many more rows of output.

m. See all tables in the dvwa database:

```
' UNION SELECT TABLE_NAME, NULL FROM information_schema.tables where TABLE_SCHEMA='dvwa'#
```

n. Display the names of all the columns, the datatype of all the columns, and the maximum character length of each column in the users table:

```
' UNION SELECT CONCAT(COLUMN_NAME, 0x0A, DATA_TYPE), CHARACTER_MAXIMUM_LENGTH FROM information_schema.columns WHERE table_name='users'#
```

https://mariadb.com/kb/en/concat/

The CONCAT() function combines two or more strings.

0x means that base 16 (hexadecimal) follows. 0A is the hexadecimal representation of the ASCII/Unicode newline character escape sequence, \n, as shown at https://www.asciitable.com.

Remember that when using UNION here, exactly two columns must be returned by each query. If you wanted to return more than two columns, that’s where the CONCAT() function comes in handy. Notice in the output that the name of the column is put in the First name: output; then, because of the 0x0A, the output goes to the next line, which outputs the column’s datatype. Finally, the Surname: