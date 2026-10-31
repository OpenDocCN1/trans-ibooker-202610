Here, elements can be a wildcard (for example, * to select everything), or the list of columns you want to retrieve. table is the table(s) from which you want to retrieve the information. The WHERE clause is optional, and if used, the query will only return the rows that fulfill the condition. For example, you can select the name, description, and price columns of all products below $100 (USD):

```
SELECT name,description,price FROM products WHERE price<100
```

The WHERE clause can also use Boolean operators to make more complex conditions:

```
SELECT columnA FROM tableX WHERE columnE='employee' AND columnF=100;
```

The preceding SQL statement will return the values in columnA from a table named tableX if the condition following the WHERE clause is satisfied; that is, columnE has a employee string value and columnF has the 100 value.

# Vulnerable code

Similar to the command injection flaw discussed earlier, the variable passed using the GET method is also often used to build a SQL statement. For example, the /books.php?userinput=1 URL will display information about the first book.

In the following PHP code, the input provided by the user via the GET method is directly added to the SQL statement. The MySQL_query() function will send the SQL query to the database and the MySQL_fetch_assoc() function will fetch the data in an array format from the database:

```
<?php
$stockID = $_GET["userinput"];
$SQL= "SELECT * FROM books WHERE ID=" . $stockID;
$result= MySQL_query($SQL);
$row = MySQL_fetch_assoc($result);
?>
```

Without proper input validation, the attacker can take control over the SQL statement. If you change the URL to /books.php?userinput=10-1, the following query will be sent to the backend database:

```
SELECT * FROM books WHERE ID=10-1
```