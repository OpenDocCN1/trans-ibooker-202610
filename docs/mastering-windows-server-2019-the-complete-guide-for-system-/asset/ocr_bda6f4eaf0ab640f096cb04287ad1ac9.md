Figure 12.11: Updating your binding so the website runs on your new VIP

![](bbox=[0.13, 0.08, 0.87, 0.36])

Now we come to the last piece of the puzzle: DNS. Remember, we want users to have the ability to simply enter http://intranet into their web browsers to browse this new NLB website, so we need to configure a DNS host A record accordingly. That process is exactly the same as any other DNS host record; simply create one and point intranet.contoso.local to 10.10.10.42:

![](bbox=[0.13, 0.5, 0.87, 0.84])

Figure 12.12: Configuring the DNS record