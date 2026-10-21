parameter used to enable debug mode is the debug query parameter with a value of 1 (for true), for example:

```
http://example.com/graphql?debug=1
```

Developers will most likely use debug mode in their staging or development environments. You can use the list of nonproduction GraphQL URLs (https://github.com/dolevjf/Black-Hat-GraphQL/blob/master/resources/non-production-graphql-urls.txt) to test for verbose debug error messages across multiple GraphQL subdomains and endpoints.

Many developers may also write debug messages to a browser’s console by using the console.log function in JavaScript. In the browser’s developer tools, use the Console tab to inspect console messages for possible debug logs that may be attributed to GraphQL functionality.

# Inferring Information from Stack Traces

Stack traces (also known as stack backtraces and stack tracebacks) are function calls that an application executes when an exception error occurs. This breadcrumb trail is extremely useful for developers trying to identify failure conditions in their source code. But if these stack traces are made available to hackers, we could use the sensitive information about the system and source code to extract data and tailor future attacks.

As mentioned earlier, various GraphQL endpoints on the same server could have different configuration settings. For example, DVGA’s /graphql endpoint does not throw stack traces to client requests that raise an error. However, the /graphiql endpoint, which provides access to graphical query tools, is configured to return stack traces when an error is raised.

If you think about it, having different settings for each endpoint makes sense. The assumption is that developers use graphical interfaces for debugging and testing, so they might require verbose error messages to identify bugs, something that isn’t necessary in production endpoints such as /graphql.

Let’s practice taking advantage of this configuration. Using the browser, navigate to DVGA at http://localhost:5013 and toggle on the Beginner mode via the cubes menu icon. Next, to gain access to DVGA’s /graphiql endpoint as a client, we’ll need to modify the env cookie from its default value of graphiql:disable to graphiql:enable by using the browser’s developer tools. You can access these by pressing CTRL-SHIFT-I or by right-clicking anywhere in the browser window and selecting Inspect. Figure 6-3 shows the Inspect window in Firefox.

You can modify the env cookie directly from the browser by clicking the Storage tab, then Cookies, and selecting http://localhost:5013 from the drop-down menu. You will need to double-click the value field.