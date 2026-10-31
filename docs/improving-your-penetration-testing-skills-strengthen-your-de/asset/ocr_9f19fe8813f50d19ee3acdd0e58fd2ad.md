When you send a request from Burp Suite to the browser, you get a URL starting with http://burp/repeat/ that you need to copy and paste into the browser that you want to replay the request on. Burp Suite doesn't launch the browser like ZAP does:

![](bbox=[0.15, 0.17, 0.85, 0.32])

The following screenshot shows how the request in the example appears in the browser. It definitely looks like the System error. message should not be there, and you should look deeper into that request and manually try variants in order to gain SQL injection:

![](bbox=[0.13, 0.45, 0.86, 0.64])