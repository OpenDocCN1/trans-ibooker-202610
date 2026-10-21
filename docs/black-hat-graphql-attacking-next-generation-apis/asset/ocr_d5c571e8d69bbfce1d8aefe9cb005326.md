automatically grab information from the hosts. For example, information such as the <title> HyperText Markup Language (HTML) tag, the entire <body> tag, and even the server HTTP response header could all hint at specific applications that the remote server is running.

It’s important to note that web applications may not always have a user interface, meaning they may not serve any HTML content related to the application or even expose HTTP headers by which we can detect them. They will often act as standalone API servers that expose data only through designated APIs. So, how can we detect GraphQL in those cases? Luckily, GraphQL APIs often return predictable responses under certain conditions, such as the HTTP method in use or the payload sent to the server. Listing 4-2 shows a common GraphQL response returned when a client makes a GET request.

```
# curl -X GET http://localhost:5013/graphql

{"errors":[{"message":"Must provide query string."}]}
```

Listing 4-2: A GraphQL response to an HTTP GET request

The string Must provide query string is often used in GraphQL implementations, such as Python- and Node.js-based ones. (Keep in mind that GET-based queries are often not supported by GraphQL servers. Rest assured: we have many other ways of detecting GraphQL should we run into such a situation.)

With this information, we now have the ability to automate a scan and pick up any other GraphQL servers that may exist on a network. Listing 4-3 shows how to do this with Nmap, using the http-grep NSE script, which uses pattern matching to look for keywords in a given web page.

```
# nmap -p 5013 -sV --script=http-grep
--script-args='match="Must provide query string", ① http-grep.url="/graphql"' localhost ②

PORT     STATE SERVICE VERSION
5013/tcp open  http    Werkzeug httpd
| http-grep:
|   (1) http://localhost:5013/graphql:
|   (1) User Pattern 1:
|     + Must provide query string
```

Listing 4-3: A GraphQL response to word-matching using Nmap’s http-grep

At ① we specify a script argument to http-grep called match with a value of Must provide query string (the message we received in our GraphQL response). At ② we define another argument, called http-grep.url, with a value of /graphql, which instructs Nmap to search a specific page within the web application. Under the hood, Nmap will make an HTTP GET request to localhost and use the argument string value we defined as the pattern for its search within the text it extracts from the web server’s response. In its output, Nmap shows that a pattern was found on the web page and indicates the string for which it found a match.