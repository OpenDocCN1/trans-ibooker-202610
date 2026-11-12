# Example of a Named Access List

This example converts the extended IP numbered ACL from the “Extended IP ACL Example” section earlier in this chapter to a named ACL:

```
Router(config)# ip access-list extended do_not_enter
Router(config-ext-acl)# permit tcp any 172.16.0.0 0.0.255.255 established log
Router(config-ext-acl)# permit udp any host 172.16.1.1 eq dns log
Router(config-ext-acl)# permit tcp 172.17.0.0 0.0.255.255 host 176.16.1.2 eq telnet log
Router(config-ext-acl)# permit icmp any 176.16.0.0 0.0.255.255 echo-reply log
Router(config-ext-acl)# deny ip any any log
Router(config)# interface GigabitEthernet 0/0
Router(config-if)# ip access-group do_not_enter in
```

Both this example and the numbered example do the exact same thing. Therefore, it is a matter of personal preference whether you use a named ACL or a numbered ACL. (My preference is to use numbered ACLs, if only because I’ve been using them since they first came out in IOS version 7.)

![](bbox=[0.14, 0.53, 0.22, 0.6])

VIDEO 17.12. The digital resources that accompany this book contain a multimedia demonstration of configuring a named IP ACL on a router.

# ACL Remarks

Starting in IOS 12.0(2)T, you can embed remarks or comments within your ACL statements. Remarks work with named or numbered ACLs. Here’s the configuration, based on whether you’re using a numbered ACL or a named ACL: