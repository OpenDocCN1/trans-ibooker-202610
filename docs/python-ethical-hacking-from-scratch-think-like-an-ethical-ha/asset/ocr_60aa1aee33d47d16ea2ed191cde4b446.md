![](bbox=[0.267, 0.094, 0.699, 0.286])

Figure 8.20 – Connected access points

In order to get the passwords, you have to write the name of the access point and give an additional parameter, key=clear, to the command. The complete command will look like this:

```
netsh wlan show profiles "POCO X3 NFC" key=clear
```

You will see a similar output:

![](bbox=[0.211, 0.461, 0.756, 0.875])

Figure 8.21 – Retrieving passwords