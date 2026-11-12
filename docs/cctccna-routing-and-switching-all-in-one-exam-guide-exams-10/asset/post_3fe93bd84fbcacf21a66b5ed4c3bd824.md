
```
Router(config)# access-list 1 permit 192.168.1.1
Router(config)# access-list 1 deny 192.168.1.2
Router(config)# access-list 1 permit 192.168.1.0 0.0.0.255
Router(config)# access-list 1 deny any
Router(config)# interface serial 0
Router(config-if)# ip access-group 1 in
```

In this example, the first ACL statement in ACL #1 says that in order to execute the permit action, the IP packet must have a source address of 192.168.1.1—if it doesn’t, IOS proceeds to the second statement. Remember that if you omit the wildcard mask on a standard ACL, it defaults to 0.0.0.0—an exact match of the corresponding address in the ACL statement. The second ACL statement says that in order to execute the deny action, the IP packet must have a source address of 192.168.1.2; if it doesn’t, IOS proceeds to the third statement. The third ACL statement says that in order to execute the permit action, the IP packet must have a source address between 192.168.1.0 and 192.168.1.255—if it doesn’t, IOS proceeds to the fourth statement. The fourth statement is actually not necessary: it drops any packet. You don’t need this statement since an invisible implicit deny any statement occurs at the end of every ACL. The last two commands in the ACL example activate ACL #1 on serial 0 as traffic comes into the interface.

Actually, you could have written the preceding ACL like this:

```
Router(config)# access-list 1 deny 192.168.1.2
Router(config)# access-list 1 permit 192.168.1.0 0.0.0.255
Router(config)# interface serial 0
Router(config-if)# ip access-group 1 in
```

This example reduces your configuration from four ACL statements in the list down to two.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_1005_0.png)

VIDEO 17.09. The digital resources that accompany this book contain a multimedia demonstration of configuring a standard numbered ACL on a router.

Here’s another example of a standard ACL:

```
Router(config)# access-list 2 deny 192.168.1.0
Router(config)# access-list 2 deny 172.16.0.0
Router(config)# access-list 2 permit 192.168.1.1
Router(config)# access-list 2 permit 0.0.0.0 255.255.255.255
Router(config)# interface ethernet 0
Router(config-if)# ip access-group 2 out
```

This ACL example has a few problems. Examine it and see if you can spot them.

The first ACL statement appears to deny all traffic from 192.168.1.0/24. In reality, it will accomplish nothing. Remember that if you omit the wildcard mask for the address, it defaults to 0.0.0.0—an exact match. The problem is that you’ll never have a packet with a source address of 192.168.1.0, since this is a network number, and not a host address. The second statement has the same problem. The third and fourth statements are okay.

As you can see, configuring ACLs can be tricky. For the preceding example, here’s the updated configuration:

```
Router(config)# access-list 2 deny 192.168.1.0 0.0.0.255
Router(config)# access-list 2 deny 172.16.0.0 0.0.255.255
Router(config)# access-list 2 permit 192.168.1.1
Router(config)# access-list 2 permit 0.0.0.0 255.255.255.255
Router(config)# interface ethernet 0
Router(config-if)# ip access-group 1 out
```

In this example, the first statement now says that any packet with a source address from network 192.168.1.0/24 should be dropped. The second statement will drop any traffic from the Class B network 172.16.0.0/16. The third statement will permit traffic from 192.168.1.1. The fourth statement will permit traffic from anywhere. Actually, there is still a problem with this configuration—look at the first and third statements. Will the third statement ever be executed? If you answered no, you are correct. In this situation, you need to put the more specific entry before the less specific one. Another minor point to make is that the fourth statement in the list could represent the address as the keyword any. Here’s the updated configuration:

```
Router(config)# access-list 2 permit 192.168.1.1
Router(config)# access-list 2 deny 192.168.1.0 0.0.0.255
Router(config)# access-list 2 deny 172.16.0.0 0.0.255.255
Router(config)# access-list 2 permit any
Router(config)# interface ethernet 0
Router(config-if)# ip access-group 1 out
```

There’s actually one more problem with this ACL. If you guessed the ACL number used on the interface is not correct, then you guessed correctly. Notice that the ACL created has a number of 2, while the application of the ACL on the interface uses 1. To fix this, use the following configuration:

```
Router(config)# interface ethernet 0
Router(config-if)# no ip access-group 1 out
Router(config-if)# ip access-group 2 out
```

Note that you must first remove the old ACL from the interface before applying the new ACL.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_1007_0.png)

**EXAM TIP** For the CCNA exam, be able to troubleshoot ACL configurations by examining the order of statements in the list. The preceding example with misconfigured ACL statements is an excellent example of issues to look for in configuring ACLs.

## Restricting VTY Access to the Router

In addition to using standard IP ACLs to filter traffic as it enters and/or leaves an interface, you can also use them to restrict VTY access (telnet and SSH) to your router. You may want to do this to allow only network administrators to access the CLI of your IOS device remotely. Setting this up is almost the same as setting up restricted access on an interface.

First, you need to create a standard ACL that has a list of **permit** statements that allow your corresponding network administrators remote access; include the IP addresses of their PCs in this list. Next, you need to activate your ACL. However, you will not do this on any of the router’s interfaces. If you were to activate this ACL on an interface, it would allow any type of traffic from your administrators but drop all other traffic. Remember that when someone accesses your router via telnet or SSH, the router associates this connection with a VTY line. Therefore, you’ll apply your standard ACL to the VTYs, like this:

```
Router(config)# line vty 0 4
Router(config-line)# access-class standard_ACL_# in|out
```

Remember that your router supports five telnets by default (0–4), and more on certain IOS devices. You can configure all VTYs simultaneously by specifying the beginning and ending line numbers after the vty parameter. If you don’t apply the restriction to all of your VTYs, you are leaving a back-door into your router, which may cause a security problem.

Also notice the command used to apply the ACL to the line: access-class. This is different from activating an ACL on a router’s interface. If you use the in parameter, you are restricting telnet and SSH access to the router itself. The out parameter is kind of unique. By using this parameter, you are restricting what destinations this router can telnet or SSH to when someone uses the telnet, connect, or ssh command. This creates an exception to a standard ACL and has the router treat the address in the ACL statements as a destination address; it causes the router to compare this address to the address in the telnet command before allowing the user on the router to telnet to the specified destination.

Here’s a simple example of using a standard ACL to filter telnet traffic to a router:

```
Router(config)# access-list 99 permit 192.168.1.0 0.0.0.255
Router(config)# line vty 0 4
Router(config-line)# access-class 99 in
```

In this example, only traffic from 192.168.1.0/24 is allowed to telnet or SSH into this router. Because of the implicit deny at the end of access-list 99, all other connections to this router (via the VTYs) will be dropped.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_1008_0.png)

**VIDEO 17.10.** The digital resources that accompany this book contain a multimedia demonstration of configuring a standard numbered ACL to restrict remote access to a router.

As you will see in the next section, you can also use extended ACLs to restrict access to the IOS device, but this configuration is much more complex. Second, extended ACLs are applied to interfaces and thus won’t be able to restrict telnet access from the router to a remote destination. And third, whenever you apply an ACL to an interface on the router, you’ll affect the performance of the router on that interface. Depending on the router model, the IOS version, and the features you have enabled, the degradation in performance will vary (today’s most current IOS versions take a very small performance hit when using ACLs on interfaces). Therefore, if you only want to restrict telnet or SSH access to or from the router, using a standard ACL and the **access-class** statement on your VTYs is the best approach.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_1009_0.png)

**EXAM TIP** For the CCNA exam, remember that you can restrict remote access to your IOS device by requiring user login, using only SSH, and applying a standard ACL to the VTY lines. You need to apply the ACL with the **access-class** mode command on the VTY lines. Note that you can also do this with an extended ACL, but this requires more configuration on your part.

## Exercise 17-5: Configuring Standard Numbered ACLs

In this exercise you will create two standard access lists on the VAN-R1 router. The first ACL will block all traffic from the 14.0.0.0 network except traffic from one IP address, and the second ACL will be used to
