Try loading a file as follows:

```
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd" >]>
```

![](bbox=[0.12, 0.18, 0.87, 0.56])

Using SYSTEM, you are defining an external entity. This loads a file (/etc/passwd), and the server displays the result in its response.

If the parser is not properly configured, and the expect PHP module is loaded, you can also gain remote execution through XXEs:

```
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "expect://uname -a" >]>
```

# The Entity Expansion attack

Even if external entities are not allowed by the parser, the permitting of internal entities can still be exploited by a malicious user and cause a disruption in the server. As all XML parser replaces entities with their defined values, a set of recursive entities can be created so that the server can process a huge amount of information until it is unable to respond.