The SameSite cookie attribute accepts three values:

+   - **Strict** Send the cookie only when the user is browsing within the same origin
+   - **Lax** Send cookies only when the request uses HTTP GET and was not initiated by a script, such as by top-level navigation
+   - **None** Send the cookie on cross-site requests, effectively providing no protection

GraphQL servers that set cookies with the SameSite attribute will return a Set-Cookie HTTP response header:

```
Set-Cookie: session=mysecretsession; SameSite=Strict
```

When a website sets a cookie without specifying the SameSite attribute, modern browsers such as Chrome assume it is set to Lax. When a cookie is set with a value of Strict, the cookie won’t be sent on cross-site requests if a CSRF attack takes place.

# Anti-CSRF Tokens

To protect against CSRF vulnerabilities at the server level, web frameworks introduced *anti-CSRF tokens*. These are hard-to-guess, cryptographically strong, and unique strings generated on the server. The server expects the client to pass an anti-CSRF token on every request. When a server sees an incoming request without such a token, the server rejects that request.

Servers can generate anti-CSRF tokens per request or once for the lifetime of a user session. Generating a token per request is stronger mitigation and more difficult to defeat because it reduces the amount of time an attacker has to obtain a valid token. Once a token gets invalidated, the server should no longer accept it.

Clients typically send anti-CSRF tokens to the server by using an HTTP request header, such as X-CSRF-TOKEN, or in an HTTP body parameter, such as csrf-token. Many web frameworks have built-in support for CSRF protection, allowing developers to build secure applications without requiring them to implement CSRF defenses from scratch. Here is an example HTTP request that contains an anti-CSRF token:

```
POST /graphql HTTP/1.1
Host: localhost:5013
Content-Length: 19
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.45 Safari/537.3

query=mutation%7BcreatePaste%28title%3A%22CSRF%22%2Ccontent%3A%22content%22%2Cpublic%3Atrue%2Cburn%3Afalse%29%7Bpaste%7Bid+content+title+burn%7D%7D&csrf-token=asj2nrcs82kssadis
```