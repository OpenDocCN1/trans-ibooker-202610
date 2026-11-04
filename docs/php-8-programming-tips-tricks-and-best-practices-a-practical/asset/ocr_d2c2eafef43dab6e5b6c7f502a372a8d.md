You now have an idea of how to use the Reflection extension and what to expect after a PHP 8 migration. It's time to have a look at a number of other extensions that saw changes in PHP 8.

# Working with other extension gotchas

PHP 8 introduced a number of other noteworthy changes to several PHP extensions other than the ones already discussed in this chapter. As we have stressed time and again in this book, it's extremely important for your future career as a PHP developer to be aware of these changes.

Let's first have a look at changes to database extensions.

# New database extension operating system library requirements

Any developer using MySQL, MariaDB, PostgreSQL, or PHP Data Objects (PDO) needs to be aware of new requirements for supporting operating system libraries. The following table summarizes the new minimum versions required in PHP 8:

| Extension | Library | Minimum Vers |
|---|---|---|
| PgSQL | libpq | 9.1 |
| PDO_PGSQL | libpq | 9.1 |
| MySQLi | libmysqlclient | 5.1 |
| PDO_MySQL | libmysqlclient | 5.1 |

Table 7.4 – PHP 8 database library requirements

As you can see from the preceding table, there are two main library changes. libpq affects both the PostgreSQL extension and the driver for the PDO extension. libmysqlclient is the library used by both the MySQL Improved (MySQLi) extension and as the MySQL driver for the PDO extension. It should also be noted that if you are using MariaDB, a popular open source version of MySQL, the new minimum MySQL library requirement applies to you as well.

Now that you are aware of database extension changes, we next turn our attention to the ZIP extension.