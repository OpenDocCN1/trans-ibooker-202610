interfaces. If you were to activate this ACL on an interface, it would allow any type of traffic from your administrators but drop all other traffic. Remember that when someone accesses your router via telnet or SSH, the router associates this connection with a VTY line. Therefore, you’ll apply your standard ACL to the VTYs, like this:

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

![](bbox=[0.14, 0.82, 0.22, 0.88])