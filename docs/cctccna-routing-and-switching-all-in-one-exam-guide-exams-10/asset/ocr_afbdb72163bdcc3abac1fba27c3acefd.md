Here is an example of loading an IOS image into your router:

```
Router# copy tftp flash
Address or name of remote host []? 192.168.1.10
Source filename []? c3640-js-mz.120-7
Destination filename [c3640-js-mz.120-7]?
%Warning:There is a file already existing with this name
Do you want to over write? [confirm] y
Accessing tftp://192.168.1.1/c3640-js-mz.120-7...
Erase flash: before copying? [confirm] y
Erasing the flash filesystem will remove all files! Continue?
[confirm] y
Erasing device... eeeeeeeeeeeeeeeeeeeeeeeeeeeeee ...erased
Erase of flash: complete
Loading c3640-js-mz.120-7 from 192.168.1.1 (via FastEthernet0/0):
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
.
.
.
[OK - 6754416/13508608 bytes]

Verifying checksum... OK (0xCAF2)
6754416 bytes copied in 66.968 secs (102339 bytes/sec)
Router#
```

In this example, the router noticed that the name of the image that exists on the TFTP server is the same name that is in flash and verifies that you want to overwrite it. Also notice that the router prompts you to erase flash—this is the default with IOS and will delete all files. Answer n if you don’t want to completely erase flash.

![](bbox=[0.14, 0.64, 0.25, 0.75])

**NOTE** When copying files into flash, remember that the default is to erase flash before the copy is performed.

After the router copies the IOS image to flash, you must reboot your router in order for it to use the new image. However, by default, the Cisco