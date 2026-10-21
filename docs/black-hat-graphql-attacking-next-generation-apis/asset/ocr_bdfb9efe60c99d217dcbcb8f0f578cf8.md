You can see the title and content of the paste, as well as its author (Darcee) and some metadata about them, such as their internet protocol (IP) address (215.0.2.85) and web browser (Mozilla/5.0).

# Testing DVGA

Now that you have a target application in your lab environment, verify that the application is up and its GraphQL API is accessible on the network with a simple GraphQL query. For this, we’ll use the Altair client we installed earlier.

Open the Altair client and enter http://localhost:5013/graphql in the address bar. Next, enter the following GraphQL query in the left pane:

```
query {
  systemHealth
}
```

This query should result in output similar to that shown in Figure 2-8.

![](bbox=[0.242, 0.38, 0.903, 0.708])

Figure 2-8: The DVGA response in Altair

Names in GraphQL are case-sensitive, so make sure you follow the capitalization in systemHealth; otherwise, this query will result in an error.

# Installing GraphQL Hacking Tools

In Chapter 1, we highlighted the differences between REST and GraphQL APIs. These differences required the security industry to build GraphQL support into existing tools. In some cases, hackers also created new tools