```
Router# clear ip nat translation *
Router# clear ip nat translation inside
    global_IP_address local_IP_address
Router# clear ip nat translation outside
    global_IP_address local_IP_address
Router# clear ip nat translation protocol inside
    global_IP_address global_port
    local_IP_address local_port
```

The first command clears all dynamic entries in the table. Note that to clear static entries, you need to delete your static NAT configuration commands from within configuration mode.

In addition to **show** commands, you can also use **debug** commands for troubleshooting. The **debug ip nat** command, for instance, will show the translations the IOS is doing on every translated packet. This is useful in determining whether the IOS is translating your packet and segment header addressing information correctly. Please note that on a busy network, this command will require a lot of CPU cycles on your router.

![](bbox=[0.14, 0.49, 0.24, 0.56])

**EXAM TIP** Remember these commands for the exam. Use the **show ip nat translations** command to display the router’s translations. Use the **clear ip nat translation** command to clear dynamic translations from the translation table. The **debug ip nat** command shows the router performing address translation in a real-time fashion.

Here’s an example of the **debug ip nat** command:

```
Router# debug ip nat
05:32:23: NAT: s=192.168.1.10->200.200.200.2, d=201.1.1.1 [70]
05:32:23: NAT*: s=201.1.1.1, d=200.200.200.2->192.168.1.10 [70]
```

In the first line of this example, an internal machine (192.168.1.10), which is the source address in the packet (s=), is having its address translated to 200.200.200.2 where the packet is being sent to the destination of 201.1.1.1 (d=). The second line shows the returning traffic from 201.1.1.1 and the