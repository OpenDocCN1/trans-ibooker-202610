Once all attack parameters have been configured, you are ready to start the attack. It doesn't take much time for error to start getting matches. You can see that table is matched by every response, so it was not a good choice. SQL and select get no matches, at least in the first responses. If you select one of the responses that have error checked, you will see that there is a message System error. at the top of the page, which seems to be triggered when the payload contains a single quote.

This can be an indicator of SQL injection, and it may worth digging into a little more:

![](bbox=[0.13, 0.27, 0.87, 0.71])