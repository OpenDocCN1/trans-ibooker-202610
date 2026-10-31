+   - **Client-side only checks:** If the application verifies that the user is actually executing certain actions only using client-side code, an attacker can still bypass those checks using JavaScript, be it via an XSS exploitation, or in the attacking page, or simply by replaying the final request.

# Exploiting a CSRF flaw

Exploiting this vulnerability through a GET request (parameters sent within the URL) is as easy as convincing the user to browse to a malicious link that will perform the desired action. On the other hand, to exploit a CSRF vulnerability in a POST request requires creating an HTML page with a form or script that submits the request.

# Exploiting CSRF in a POST request

In this section, we will focus on exploiting a POST request. We will use Peruggia's user-creation functionality for this exercise. The first step is that you need to know how the request that you want to replicate works; if you log in as admin to Peruggia and create a new user while capturing the traffic with Burp Suite, you can see that the request appears as follows:

![](bbox=[0.13, 0.5, 0.87, 0.76])