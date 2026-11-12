the VTP mode in which the switch is operating, the configuration revision number, and the VTP domain to which the switch belongs.

Use the **show vtp counters** command to display VTP statistics concerning VTP messages sent and received:

```
switch # show vtp counters
VTP statistics:
    Summary advertisements received : 12
    Subset advertisements received : 0
    Request advertisements received : 0
    Summary advertisements transmitted : 7
    Subset advertisements transmitted : 0
    Request advertisements transmitted : 0
    Number of config revision errors : 0
    Number of config digest errors : 0
    Number of V1 summary errors : 0
.
```

In this example, you can see that the switch has sent and received VTP summary advertisements.

# Exercise 10-1: Working with VTP

In this exercise, you will use VTP to send a list of VLANs from one switch to another so that there is a consistent list of VLAN across all your switches. Use the following network diagram to represent the two switches in the Vancouver office: