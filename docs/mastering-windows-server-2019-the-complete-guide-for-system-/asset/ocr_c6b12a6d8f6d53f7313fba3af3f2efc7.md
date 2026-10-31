Now that we have created this new host record, we should immediately be able to start resolving this name inside our domain network. Moving back to the client machine from which I was trying to ping RA1 earlier, I'll try the same command again, and this time it does resolve and reply successfully:

![](bbox=[0.13, 0.16, 0.86, 0.38])

Figure 4.4: Successfully pinging RA1

# Alias record – CNAME

Another useful type of DNS record is CNAME, which more commonly these days is called an alias record. This is a record that you can create that takes a name and points it at another name. It sounds a little silly at first glance, because, in the end, you are still going to have to resolve your final name to an IP address by using a host record in order to get the traffic where it needs to go, but the purposes of an alias record can be vast. A good example to portray the usefulness of an alias record is when you are running a web server that is serving up websites within your network. Rather than force all of your users to remember a URL like http://web1.contoso.local in order to access a website, we could create an alias record called intranet, and point it at WEB1. This way, the more generalized intranet record can always be utilized by the client computers, which is a much friendlier name for your users to remember.