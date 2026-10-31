# Adding a route with PowerShell

Since PowerShell is king when it comes to most command-line-oriented tasks inside Windows Server, we should accomplish the same mission from this interface as well. You can utilize the same route add command from inside the PowerShell prompt and that will work just fine, but there is also a specialized cmdlet that we can use. Let's utilize New-NetRoute to add yet another subnet to our routing table. This time, we are going to add 192.168.17.0. The following is a command we can utilize:

```
New-NetRoute -DestinationPrefix "192.168.17.0/24" -InterfaceIndex 5
-NextHop 10.10.10.1
```

![](bbox=[0.13, 0.29, 0.86, 0.43])

Figure 7.18: Adding another subnet to our routing table

You can see that the structure is similar, but a little bit friendlier. Instead of having to type the word mask and specify the whole subnet mask number, you can use the slash method to identify the subnet and mask within the same identifier. Also, where before we were specifying the gateway, which is always a little confusing, with the New-NetRoute cmdlet, we instead specify what is called the NextHop. This makes a little bit more sense to me.