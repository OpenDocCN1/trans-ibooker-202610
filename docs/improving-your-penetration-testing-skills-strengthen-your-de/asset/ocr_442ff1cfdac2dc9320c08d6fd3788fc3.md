# Burp Intruder

You have already used Intruder for various tasks in previous chapters, and you are aware of its power and flexibility. Now we will use it to fuzz the BodgeIt login page looking for SQL injection vulnerabilities. The first thing that you need to do is to send a valid login request from the proxy history to Intruder. This is accomplished by right-clicking on the request and selecting Send to Intruder.

Once in Intruder, you will clear all of the insertion points and add one in the username value, as shown in the following screenshot:

![](bbox=[0.13, 0.31, 0.87, 0.66])

The next step is to set the payloads. To do this, go to the Payloads tab, click on Load... to load a file, and go to /usr/share/wfuzz/wordlist/injections/SQL.txt: