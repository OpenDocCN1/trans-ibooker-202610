You can see in Figure 7.7 that I have ping statistics from both DC1 and DC2, to each of the WEB1 and BACK1 servers in my network. Test-Connection has the potential to be a very powerful monitoring tool:

![](bbox=[0.13, 0.15, 0.87, 0.44])

Figure 7.7: Multiple sources and destinations using Test-Connection

One more useful function to point out is that you can clean up the output of the command pretty easily by using the -Quiet switch. By adding -Quiet to a Test-Connection command, it sanitizes the output and only shows you a simple True or False for whether the connection was successful, instead of showing you each individual ICMP packet that was sent. Unfortunately, you cannot combine both the -Source switch and the -Quiet switch, but if you are using Test-Connection from the original source computer that you are logged in to, like most of us will be doing anyway, -Quiet works great. Most of the time, all we really care about is Yes or No as to whether these connections are working, and don't necessarily want to see all four attempts. By using -Quiet, we get exactly that:

```
Test-Connection -Quiet -ComputerName WEB1, BACK1, DC2, CA1
```