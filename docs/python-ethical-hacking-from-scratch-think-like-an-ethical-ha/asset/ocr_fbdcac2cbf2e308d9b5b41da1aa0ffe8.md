![](bbox=[0.18, 0.08, 0.79, 0.3])

Figure 3.12 – ifconfig command output

There is a lot to unpack here, so let's break it down. There are two values here called eth0 and lo. eth0 is the name of the NIC, whereas lo is the loopback adapter. For now, we can ignore the loopback adapter. The inet field represents the private IP address of the Kali machine. inet6 is the IPv6 address of the Kali machine. ether is the MAC address, and this is the field we want to change.

If you want to change the MAC address, you can't do so while the NIC is turned on. First, you have to shut down the network interface. To shut down the interface, you can use the following command:

```
sudo ifconfig eth0 down
```

This command will shut down the NIC named eth0. If you don't see an error in the command's output, this means that the command ran successfully.

Now, if you type in the ifconfig command again, you will see the following output:

![](bbox=[0.19, 0.59, 0.77, 0.76])

Figure 3.13 – Shutting down a network interface card