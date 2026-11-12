Now we’ll use Figure 13-7 to see how PAT is configured. In this example, 200.200.200.1 is being used in the static translation for the internal server. Only a single, additional, IP address is placed in the address pool (200.200.200.2) and the address translation is restricted to performing PAT for only two devices: 192.168.1.10 and 192.168.1.11. Here’s the configuration:

```
Router(config)# ip nat inside source list 1 pool
                nat-pool overload
Router(config)# access-list 1 permit host 192.168.1.10
Router(config)# access-list 1 permit host 192.168.1.11
Router(config)# ip nat pool nat-pool 200.200.200.2
                200.200.200.2
                netmask 255.255.255.0
Router(config)# interface ethernet 0
Router(config-if)# ip nat inside
Router(config-if)# exit
Router(config)# interface serial 0
Router(config-if)# ip nat outside
```

# Address Translation Verification

Once you have configured address translation, you can use many different commands to verify and troubleshoot the operation of address translation on your router. For instance, if you want to see the address translation table on your router, use the show ip nat translations command:

```
Router# show ip nat translations
Pro  Inside global  Inside local  Outside local  Outside global
---  200.200.200.1   192.168.1.1   ---            ---
---  200.200.200.2   192.168.1.2   ---            ---
```

In this example, two addresses are being translated: 192.168.1.1 (inside local) to 200.200.200.1 (inside global) and 192.168.1.2 (inside local) to 200.200.200.2 (inside global). Notice that no protocol is listed (Pro) or port numbers, indicating that these are NAT translations, not PAT.

Here’s an example of the show command with PAT translations in the translation table: