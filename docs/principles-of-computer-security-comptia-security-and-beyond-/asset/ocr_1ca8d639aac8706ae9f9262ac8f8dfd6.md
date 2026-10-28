```
c:\wamp64\bin\mariadb\mariadb10.4.10\bin\mysql.exe
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 30
Server version: 10.4.10-MariaDB mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| dvwa               |
| information_schema |
| mysql              |
| performance_schema |
| test               |
+--------------------+
5 rows in set (0.001 sec)

MariaDB [(none)]> USE dvwa;
Database changed
MariaDB [dvwa]> SHOW TABLES;
+------------------+
| Tables_in_dvwa   |
+------------------+
| guestbook        |
| users            |
+------------------+
2 rows in set (0.001 sec)

MariaDB [dvwa]> DESCRIBE users;
+--------------+---------------+------+-----+-------------------+----------------------------+
| Field        | Type          | Null | Key | Default           | Extra                      |
+--------------+---------------+------+-----+-------------------+----------------------------+
| user_id      | int(6)        | NO   | PRI | NULL              |                            |
| first_name   | varchar(15)   | YES  |     | NULL              |                            |
| last_name    | varchar(15)   | YES  |     | NULL              |                            |
| user         | varchar(15)   | YES  |     | NULL              |                            |
| password     | varchar(32)   | YES  |     | NULL              |                            |
| avatar       | varchar(70)   | YES  |     | NULL              |                            |
| last_login   | timestamp     | NO   |     | current_timestamp() | on update current_timestamp() |
| failed_login | int(3)        | YES  |     | NULL              |                            |
+--------------+---------------+------+-----+-------------------+----------------------------+
8 rows in set (0.045 sec)

MariaDB [dvwa]>
```

FIGURE 19-9 Examining the users table in the dvwa database