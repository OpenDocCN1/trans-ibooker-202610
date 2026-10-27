# Invalid Addresses

Not only should you be familiar with the private address ranges in TCP/IP, but you should also be able to identify invalid addresses. An invalid address is an address that is not allowed to be assigned to a host on the network such as a system or router. From a certification exam point of view, you need to be able to identify these invalid addresses. The following are considered invalid addresses:

+   - **Any address starting with 127** An IP address that starts with 127 is reserved for the loopback address and cannot be assigned to a system. An example of this type of invalid address is 127.50.10.23.
+   - **All host bits set to 0** You are not allowed to assign a system an IP address that has all of the bits in the host ID portion set to 0 because this is the network ID. An example of this type of invalid address is 131.107.0.0.
+   - **All host bits set to 1** You are not allowed to assign a system an IP address that has all the host bits set to 1 because this corresponds to the broadcast address of the network. An example of this type of invalid address is 131.107.255.255.
+   - **A duplicate address** You are not allowed to assign a system an address that another system is using because this results in a “duplicate IP address” error.

# EXERCISE 1-2

# Understanding Valid Addresses

In this exercise, you will practice identifying valid addresses by recording whether each of the following addresses is valid. A valid address is an address that can be assigned to a system on the network. If an address is invalid, you must specify why.

| Address | Valid? |
|---|---|
| 10.0.40.10 | |
| 127.54.67.89 | |
| 131.107.34.0 | |
| 45.12.0.0 | |
| 216.83.11.255 | |
| 63.256.4.78 | |
| 200.67.34.0 | |
| 131.107.23.255 | |