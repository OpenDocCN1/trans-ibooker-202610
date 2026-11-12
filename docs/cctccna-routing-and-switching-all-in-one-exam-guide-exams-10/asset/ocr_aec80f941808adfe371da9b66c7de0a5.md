For step 2 of the bootup process, you can use the following **boot system** commands to influence the order that the bootstrap program should use when trying to locate the IOS image:

```
Router(config)# boot system flash name_of_IOS_file_in_flash
Router(config)# boot system tftp IOS_image_name
               IP_address_of_server
Router(config)# boot system rom
```

![](bbox=[0.14, 0.28, 0.25, 0.37])

**NOTE** The order in which you enter the **boot system** commands is important, since the bootstrap program processes them in the order in which you configure them—once the program finds an IOS image, it does not process any more **boot system** commands in the configuration file. These commands are also supported on Catalyst IOS switches.

The **boot system flash** command tells the bootstrap program to load the specified IOS filename in flash when booting up. Note that, by default, the bootstrap program loads the *first* valid IOS image in flash. This command tells the bootstrap program to load an image that’s different from the first one. This might be necessary if you perform an upgrade and you have two IOS images in flash—the old one and new one. By default, the old one still loads first (because it appears first in flash) unless you override this behavior with the **boot system flash** command or delete the old IOS flash image.

![](bbox=[0.14, 0.75, 0.22, 0.82])

**VIDEO 6.02.** The digital resources that accompany this book contain a multimedia demonstration of using **boot system** commands on a router.