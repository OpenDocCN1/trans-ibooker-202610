![](bbox=[0.14, 0.1, 0.25, 0.2])

NOTE You should not use an IOS device as a local master clock, because IOS doesn’t support a method of connecting to an externally reliable time source, such as GPS. However, many operating systems, such as Windows and Linux, support NTP server applications that support external clock connections such as GPS.

Here is the router command to define NTP servers on an IOS device:

```
IOS(config)# ntp server IP_address [version number]
            [key keyid] [source interface] [prefer]
```

You first need to define the IP address of the remote NTP server. If you don’t specify the version number for NTP, it defaults to 3 (NTPv3). The optional key parameter references authentication information to be used to verify the server’s or peer’s timing communications: this must match what the server is using when NTPv3 authentication is performed (the use of this parameter is covered in the next paragraph). The source parameter specifies what IP address on the IOS device to use as the source address in the IP packet header when sending communications to the remote NTP server (note that you identify the layer 3 interface on the IOS device to reference the IP address to use). If you omit this parameter, it defaults to the address of the outgoing layer 3 interface. When you are entering multiple NTP servers, you can use the prefer parameter, which specifies that this NTP server is preferred over other servers for synchronization purposes; otherwise, the first server configured is the first one that the IOS device will query.

You need to configure three commands to set up authentication:

```
IOS(config)# ntp authenticate
IOS(config)# ntp authentication-key key_# md5 key_value
IOS(config)# ntp trusted-key key_#
```