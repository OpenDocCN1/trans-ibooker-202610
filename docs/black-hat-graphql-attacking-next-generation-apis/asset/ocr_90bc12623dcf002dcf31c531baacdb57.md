If luck isn’t on our side and introspection was disabled on the server, we can lean on tools such as Clairvoyance, which we touched on in Chapter 6, to reconstruct the schema and discover the various available inputs. Clairvoyance will fuzz the GraphQL document inputs to discover all of its various operations, fields, types, and arguments necessary to rebuild the complete schema view, which we can then use to identify all possible inputs. We will next explore how common types of injections could look in the GraphQL world by performing some injection testing against DVGA.

# SQL Injection

*SQL injection* is one of the oldest types of vulnerabilities out there. SQLi vulnerabilities happen when client input is directly inserted into a SQL command without proper character escaping. This condition allows a hacker to close out the intended SQL query and introduce their own SQL command, effectively interfering with the query the application makes to its database.

A SQLi vulnerability in GraphQL APIs could have devastating consequences. Full or even partial access to a database could result in any of the following consequences:

+   - **Impact to data integrity.** A SQLi vulnerability could allow us to manipulate data, such as by altering data within a database table.
+   - **Impact to data confidentiality.** SQLi could allow us to leak information from a database, either from the application’s specific SQL tables or from other tables in the same database. This information could include PII, password hashes, sensitive tokens, and so on.
+   - **Impact to data availability.** SQLi could allow us to delete segments of the database or completely drop its tables, resulting in data loss and application instabilities.

In recent years, modern web frameworks have gotten better at mitigating SQLi vulnerabilities by offering out-of-the-box defense mechanisms such as parameterized queries. Utilizing audited and vetted frameworks enables developers to write code more securely by using the framework’s built-in security features, such as through functions and libraries.

# Understanding the Types of SQL Injection

There are two categories of SQLi vulnerabilities, each of which has a few subcategories.

# Classic SQL Injection

You know you’ve run into *classic SQLi* when the application returns SQL query errors during injection testing. These errors can be displayed directly in the web page or become apparent through network inspection. Two techniques are used to identify a classic SQLi vulnerability: error based and union based.

*Error-based SQLi* is used to identify SQLi vulnerabilities through error observation. Applications that throw SQL errors to the client as a result of