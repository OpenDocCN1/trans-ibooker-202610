# Automating Testing with BatchQL and GraphQL Cop

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

# Preventing CSRF

In the years since CSRF was discovered, browser vendors such as Mozilla and Google have significantly improved their CSRF mitigations. Various open source web server frameworks have also made CSRF vulnerabilities tremendously harder to exploit. This section explains the CSRF mitigations that exist today at the browser and server levels.

# The SameSite Flag

Browsers have started supporting a special HTTP cookie attribute called SameSite. This attribute allows developers to decide whether the client browser should attach the cookie when making cross-site requests. To set this cookie attribute, the application needs to set a Set-Cookie response header. This interferes with a CSRF attack’s attempt to send a request from the attacker website (say, attacker.com) to a target website of interest (banking.com).

One challenge with using the SameSite attribute is that older browsers may not support it. However, most of the modern browsers do. Mozilla’s Developer website has a dedicated section about SameSite browser support that developers can use as a reference.