
Inter-Domain Routing (CIDR) notation. For example, the IPv4 address 192.168.0.1 will become 192.168.0.1/32.

If introspection is enabled, GraphQL IDE tools such as GraphiQL Explorer or GraphQL Playground will auto-complete directive names and their arguments as soon as you start typing the at symbol (@) on a field. The same is true for fields and their arguments.

Query directive arguments can be prone to injections too. Attackers can use them to influence the way the server returns the response to the specific field. For instance, a query directive might use the argument where, which then gets translated to a SQL-matching pattern (for example, the LIKE operator).

You can use the introspection query shown in Listing 8-2 to get only the available directives by using the __schema meta-field with the directives field.

```
query GetDirectives {
  __schema {
    directives {
      name
      description
      locations
    }
  }
}
```

Listing 8-2: A GraphQL introspection query used to list directives

## Operation Names

Operation names are strings we can add to GraphQL operations such as queries, mutations, or subscriptions. They are often used to uniquely name each query when multiple queries are sent together. GraphQL graphical IDEs such as GraphiQL Explorer and GraphQL Playground use the operation name as a way to allow clients to choose which operation to run when more than one query exists in a document through a drop-down menu, as shown in Figure 8-2.

![](img/2f244b00cd9f8f37cfc22f928aaa471a_219_0.png)

Figure 8-2: Executing a selected query based on its operation name in GraphiQL Explorer

Operation names are also used for other purposes, such as debugging and logging. In fact, they are interesting potential injection vectors because applications can use them in many ways. For instance, some applications use operation names for analytics, to determine which queries clients use the most. The operation name string could end up in different systems, such as logging systems, relational databases, cache databases, and so on. It is important to check whether the GraphQL API allows special characters as part of the operation name, as this could turn out to be an injectable interface.

Operation names are typically alphanumeric, but some GraphQL server implementations are more permissive than others when it comes to the type of characters they permit.

## Input Entry Points

When attempting to perform injection testing against GraphQL APIs, we need to find a way to discover input entry points. If we are lucky and introspection is left enabled, we can often quickly access the various queries, mutations, and subscriptions that the API supports, along with information about its types, fields, arguments, and so on, using a GraphQL IDE tool like Altair, GraphiQL Explorer, or GraphQL Playground.

To view this information about DVGA in Altair, set the URL to http://localhost:5013/graphql and click the Save button (the diskette icon) located at the top right. Click the Refresh button located next to the Save button, and then click Docs. You should see a section for queries, mutations, and subscriptions. Click any of them to see the types of arguments that exist within each, as shown in the screenshot in Figure 8-3.

![](img/2f244b00cd9f8f37cfc22f928aaa471a_220_0.png)

Figure 8-3: Schema documentation in the Altair client

If luck isn’t on our side and introspection was disabled on the server, we can lean on tools such as Clairvoyance, which we touched on in Chapter 6, to reconstruct the schema and discover the various available inputs. Clairvoyance will fuzz the GraphQL document inputs to discover all of its various operations, fields, types, and arguments necessary to rebuild the complete schema view, which we can then use to identify all possible inputs. We will next explore how common types of injections could look in the GraphQL world by performing some injection testing against DVGA.

## SQL Injection

*SQL injection* is one of the oldest types of vulnerabilities out there. SQLi vulnerabilities happen when client input is directly inserted into a SQL command without proper character escaping. This condition allows a hacker to close out the intended SQL query and introduce their own SQL command, effectively interfering with the query the application makes to its database.

A SQLi vulnerability in GraphQL APIs could have devastating consequences. Full or even partial access to a database could result in any of the following consequences:

- **Impact to data integrity.** A SQLi vulnerability could allow us to manipulate data, such as by altering data within a database table.
- **Impact to data confidentiality.** SQLi could allow us to leak information from a database, either from the application’s specific SQL tables or from other tables in the same database. This information could include PII, password hashes, sensitive tokens, and so on.
- **Impact to data availability.** SQLi could allow us to delete segments of the database or completely drop its tables, resulting in data loss and application instabilities.

In recent years, modern web frameworks have gotten better at mitigating SQLi vulnerabilities by offering out-of-the-box defense mechanisms such as parameterized queries. Utilizing audited and vetted frameworks enables developers to write code more securely by using the framework’s built-in security features, such as through functions and libraries.

## Understanding the Types of SQL Injection

There are two categories of SQLi vulnerabilities, each of which has a few subcategories.

## Classic SQL Injection

You know you’ve run into *classic SQLi* when the application returns SQL query errors during injection testing. These errors can be displayed directly in the web page or become apparent through network inspection. Two techniques are used to identify a classic SQLi vulnerability: error based and union based.

*Error-based SQLi* is used to identify SQLi vulnerabilities through error observation. Applications that throw SQL errors to the client as a result of a failure in SQL query execution could allow us to find the right attack pattern to successfully exploit the SQLi vulnerability.

Union-based SQLi is used to identify SQLi vulnerabilities by leveraging the UNION SQL operator. UNION concatenates results of multiple SELECT statements, which can then be returned to the client.

## Blind SQL Injection

In blind SQLi, we are given no visible indications that a vulnerability exists. Applications could fail silently or redirect their errors to somewhere other than the client. Two discovery techniques apply to the blind SQLi category.

Time-based SQLi forces the application to wait for a certain amount of time before returning a response. By supplying a SQLi payload that instructs the database to wait for a certain number of seconds, we can infer that the application is vulnerable if a similar delay occurs in returning the final response.

Boolean-based SQLi allows us to infer whether the application is vulnerable to SQLi by constructing a payload that will return a Boolean result such as true or false. By using this testing technique, we could influence the way the application presents data to the client, which helps us identify whether the vulnerability exists.

## Testing for SQLi

While SQLi vulnerabilities are on the decline, they can still be found occasionally. As hackers, we should assume that the application we’re testing may not have the appropriate controls in place to prevent SQLi and test for it wherever and whenever possible.

Testing for SQLi can be done in a variety of ways, such as the following:

- Submitting characters like single (') or double (") quotes and observing how the application handles unexpected inputs and errors.
- Fuzzing input fields and observing application errors that may indicate database query failures.
- Submitting SQL commands that introduce delays, such as by using BENCHMARK and SLEEP for MySQL databases, WAITFOR DELAY and WAITFOR TIME for Microsoft SQL Server, or pg_sleep for PostgreSQL databases, and then performing a response-timing analysis to identify whether the injection was successful. This is especially helpful when we are performing blind SQLi testing, whereby application errors are invisible to us.

SQLi in GraphQL can be introduced through any interface that accepts client input. In this section, we will explore a SQLi example in GraphQL by using DVGA.

## Testing DVGA for SQLi with Burp Suite

The first step to injection testing in GraphQL is to find places where we can make alterations to queries. We can start by looking at the schema
