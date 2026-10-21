```
{'alias10': {'data': {'accessToken': 'eyJoeXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJoeXB1joiYWNjZXNZIiwi aW aWFOIjoxNjU3MDQ2NjI5LCJcUjYmYioJE2N TcWnNDY2MjksImpoa0SiGIjvKmZhkM2Y5LWNjNTUtNDcyYyYi1nZRhlTh iN2F1M2EyNGF1MiIsImIkzW5o aXR5Ijoib3B1cmF0 b3JilLCJleH a0iOjE2NTcWNTM4Mjl9.Ba3zfvZqjDmyLFdx71WCs-7vidaxpUfs2X3UK3zZBA'}, 'inputs': {'password': 'password123', 'username': 'operator'}}] Errors: [{'alias1': {'error': 'Authentication Failure', 'inputs': {'password': 'admin', 'username': 'admin'}}}, {'alias2': {'error': 'Authentication Failure', 'inputs': {'password': 'admin', 'username': 'password'}}}, --snip-- {'alias9': {'error': 'Authentication Failure', 'inputs': {'password': 'password123', 'username': 'operator'}}}] [*] Writing to directory results/localhost:5013_5bab6e
```

In cases where GraphQL query cost controls prevent the execution of large query batches, CrackQL has an optional -b (batch) argument, which you can use to define a more limited set of aliased operations, allowing your attack to fly under the radar.

You could also use CrackQL for a variety of other attacks. Using a list of possible one-time password tokens, CrackQL could brute-force two-factor authentication. It can also perform account enumeration attacks, by automating the scanning for valid emails or usernames, or fuzz for unique object identifiers to exploit insecure direct object reference (IDOR) vulnerabilities, where, by directly referencing an object identifier, we are able to access the object without being authorized to do so.

When performing attacks against authenticated queries, you’ll likely need to pass it authentication headers and possibly cookies. CrackQL allows you to do so using the config.py file, which accepts COOKIES and HEADERS variables. Here is an example of how to supply the tool with custom headers and cookies:

```
# cat config.py

HEADERS = {"Authorization": "Bearer mytoken"}
COOKIES = {"session":"session-secret"}
```

When performing a penetration test, you can obtain these headers by inspecting the network traffic with tools such as the Firefox Developer Tools’ Network tab. Look at any GraphQL requests that are sent after you perform an initial login to a website. At that point, you should see unique authentication headers or session cookies.

# Using Allow-Listed Operation Names

Certain in-band GraphQL implementations may make some queries and mutations publicly available for unauthenticated clients, such as those for login or account registration. Some of these deployments use operation name-based allow lists, a weak enforcement control, to reject all unauthenticated