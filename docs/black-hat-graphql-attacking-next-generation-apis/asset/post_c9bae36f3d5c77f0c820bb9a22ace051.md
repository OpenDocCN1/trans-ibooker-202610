
EyeWitness iterates over the URLs specified in the file, scans them, and saves its output into the dvwa-report folder for further inspection.

In this example, we used a file that contains only a single URL. Often, you may want to search for any additional web paths beyond the /graphql endpoint to check whether GraphQL lives in an alternative location, particularly one that’s obscure. You could create a list of URLs to use with EyeWitness in multiple ways. The first option is to use the list of common GraphQL endpoints mentioned in “Common Endpoints” on page 73.

Alternatively, use one of Kali’s built-in directory wordlists, located at /usr/share/wordlists. One such example is the dirbuster wordlist. EyeWitness needs full URLs, and this wordlist contains only web paths, so we’d first need to format it using a Bash script, as shown in Listing 4-16.

```
# for i in $(cat /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt);
do echo http://localhost:5013/$i >> urls.txt; done

# cat urls.txt

http://localhost:5013/api
http://localhost:5013/apis
http://localhost:5013/apidocs
http://localhost:5013/apilist
```

Listing 4-16: Using Bash and a directory wordlist to build a list of URLs

This Bash for loop ensures that the directories in the wordlist directory-list-2.3-small.txt are appended to our target host (http://localhost:5013) so EyeWitness can use them in its scan. All that’s left is to run EyeWitness with our new wordlist file, urls.txt.

## Attempting a Query Using Graphical Clients

Finding instances of GraphiQL Explorer or GraphQL Playground in a penetration test doesn’t guarantee that the GraphQL API itself will allow you to make unauthorized queries. Because both GraphiQL Explorer and GraphQL Playground are simply frontend interfaces to a GraphQL API, they are effectively HTTP clients that interact with a GraphQL server.

In some cases, these graphical interfaces might fail to query the API for multiple reasons. An authentication or authorization layer might be implemented in the GraphQL API that prevents unauthorized queries. The API might also restrict queries based on client properties, such as geographical location or an IP address–based allow list. Client-side mitigations could also prevent clients from running queries through GraphiQL Explorer or GraphQL Playground.

> NOTE The specification doesn’t describe how to implement security measures in GraphQL or whether authorization and authentication should exist at the GraphQL layer. Chapter 7 covers how to identify these mechanisms when they are implemented in GraphQL and how to test them in black-box penetration tests.

To confirm that we can use the interface to query the GraphQL server, we will need to send some form of an unauthenticated GraphQL query. The query must be one that will work on any GraphQL API. Think of this query as a way to confirm that the remote GraphQL API is accepting unauthenticated queries from clients. We might call it a canary GraphQL query.

Open the Firefox web browser in your lab machine and navigate to http://localhost:5013/ to access the DVWA. You should see the DVWA's main page. Next, browse to the GraphiQL Explorer panel we discovered earlier at http://localhost:5013/graphiql. You will notice that we get an immediate error, indicating that our access was rejected, with the message 400 Bad Request: GraphiQL Access Rejected, as shown in Figure 4-2.

![](img/2f244b00cd9f8f37cfc22f928aaa471a_110_0.png)

Figure 4-2: The GraphiQL Explorer rejecting client access

As hackers, it's important to look at how things work under the hood. Click the Docs button located at the top right of the window. You should see an error message, No Schema Available. This error means that GraphiQL Explorer wasn't able to retrieve schema information from the API. Because GraphiQL Explorer automatically sends an introspection query to the GraphQL API to populate the documentation section with schema information on every page load, it relies on this documentation being available.

You can see this behavior by using the Developer Tools in Firefox. Press SHIFT-F9 or right-click anywhere in the web page and select Inspect Element to open the Developer Tools console. Click the Network tab; then reload the page by pressing F5.

You should be able to see a POST request sent to the /graphiql endpoint. Figure 4-3 shows this introspection query.

![](img/2f244b00cd9f8f37cfc22f928aaa471a_111_0.png)

Figure 4-3: A GraphiQL Explorer introspection query shown in Firefox Developer Tools

If the introspection query was successfully sent, what could possibly be rejecting our access to GraphiQL Explorer? Let’s continue to explore the Developer Tools in Firefox for clues. Click the Storage tab, shown in Figure 4-4.

![](img/2f244b00cd9f8f37cfc22f928aaa471a_111_1.png)

Figure 4-4: The Developer Tools Storage tab in Firefox

The Storage tab gives us a view of the HTTP cookies that were set up by the application, as well as access to the browser's local and session storage. On the left pane, click the Cookies drop-down menu and select http://localhost:5013 to see the specific cookies for the domain, as shown in Figure 4-5.

![](img/2f244b00cd9f8f37cfc22f928aaa471a_112_0.png)

Figure 4-5: HTTP cookies

You’ll notice that, in the right pane, we have two keys set in our HTTP cookies: env and session. The env key in particular is interesting, because it appears to have the string graphql:disable set as its value. As hackers, this should ring a bell or two. Is it possible that this cookie value is responsible for GraphiQL Explorer’s denying access? We can find out by tampering with its value.

Double-click the text graphql:disable, which will allow you to modify it; then simply remove disable and replace it with enable. Next, refresh the web page. You’ll notice that we no longer see the rejection message in GraphiQL Explorer. To confirm that tampering with the cookie actually works, attempt to run a GraphQL query. You should be able to get a response from the GraphQL API! This is an example of a weak client-side security control that can easily be circumvented.

Developers often create web applications with the mindset that clients are to be trusted, but not everyone will play by the rules. Threat actors who are interested in finding loopholes will tamper with applications and attempt to defeat any countermeasures in place. It’s important to remember that anything an attacker can directly control can potentially be circumvented. Yet controls implemented on the client are not a rare thing to see; you may find applications implementing input validation or file upload validation only on the client side. These can often be bypassed. In Chapter 7, you’ll learn more about defeating GraphQL authorization and authentication mechanisms.

## Querying GraphQL by Using Introspection

Introspection is one of GraphQL’s key features, as it provides information about the various types and fields the GraphQL schema supports. A self-documenting API is very useful for anyone who needs to consume it, such as third-party businesses or other clients.

As hackers, one of the first things we want to test when we run into a GraphQL application is whether its introspection mechanism is enabled. Many GraphQL implementations enable introspection by default. Some implementations might have an option to disable introspection, but others might not. For example, the Python GraphQL implementation Graphene does not provide the option to disable introspection. To do so, the consumer would have to dig into the code and identify ways to prevent introspection queries from being processed. On the other hand, the GraphQL PHP implementation graphql-php enables introspection by default but also documents how to completely disable this feature. Table 4-2 shows the state of introspection in some of the popular GraphQL server implementations.

### Table 4-2: The State of Introspection in GraphQL Implementations

| Language | Implementation | Introspection configuration | Disable introspection option |
|---|---|---|---|
| Python | Graphene | Enabled by default | Not available |
| Python | Ariadne | Enabled by default | Available |
| PHP | graphql-php | Enabled by default | Available |
| Go | graphql-go | Enabled by default | Not available |
| Ruby | graphql-ruby | Enabled by default | Available |
| Java | graphql-java | Enabled by default | Not available |

Any default setting that directly impacts security is always good news for hackers. Application maintainers rarely change these default settings. (Some maintainers may not even be aware of them.) In Table 4-2, you can see that in some cases—such as in graphql-go, graphql-java, and Graphene—introspection can be disabled only if the application maintainers code the solution into the GraphQL API themselves; there is no official, vendor-vetted solution to disable it.

While opinions on this matter vary, especially in security circles, introspection in GraphQL is widely considered a feature and not a vulnerability. Companies that adopt GraphQL may choose to keep it enabled, while others may disable it to avoid disclosing information that could be leveraged in attacks. If no external consumers integrate with a GraphQL API, it's possible that developers could disable introspection altogether without impacting normal application flows.

Depending on your target, the response to an introspection query could be fairly large. Also, if you're attacking a target with a mature security program, these queries may be monitored for any attempts from untrusted clients, such as those in new geographical locations or with new IP addresses.

To experiment with the introspection query by using our vulnerable server, open the Altair client in your lab and ensure that the address bar is set to http://localhost:5013/graphql. Next, enter the introspection query shown in Listing 4-17 and execute it in Altair.
