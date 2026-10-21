tour DVGA's web interface. Does anything jump out at you? How about the Import a Paste page, shown in Figure 9-6?

![](bbox=[0.22, 0.11, 0.86, 0.45])

Figure 9-6: DVGA's Import a Paste page

The Import from URL form takes a URL and attempts to import the paste from it. To see what happens when you submit a URL, toggle on the Intercept mode in Burp Suite, enter any URL into the search bar, and click Submit. (Here is an example paste you could import: https://pastebin.com/raw/LQ6u1qyi.) You should see a request like the following in Burp:

```
POST /graphql HTTP/1.1
Host: localhost:5013
Content-Length: 302
Accept: application/json
Content-Type: application/json
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)
AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.45 Safari/537.36
--snip--
Origin: http://localhost:5013
Referer: http://localhost:5013/import_paste
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: env=graphql:disable
Connection: close

{"query":"mutation ImportPaste ($host: String!, $port: Int!, $path: String!, $scheme: String!) {\n        importPaste(host: $host, port: $port, path: $path, scheme: $scheme) {\n            result\n        }\n}","variables":{"host":"pastebin.com","port":443,"path":"/raw/LQ6u1qyi", "scheme":"https"}}
```