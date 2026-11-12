+   - Does your router have the minimal amount of required RAM?
+   - Does your router have the minimal amount of required flash memory?

You can see the amount of installed RAM and flash with the **show version** command (see Chapter 6). Also remember to verify that your IOS device has enough free flash memory to add the additional device (**show flash** or **dir** command); if not, you’ll need to delete unneeded files to make room for the new image.

![](bbox=[0.14, 0.32, 0.24, 0.39])

**TIP** Before upgrading IOS on a Cisco device, you should verify the flash and RAM requirements of the new IOS: does your IOS device support the new image’s requirements? You can verify the amount of RAM and flash installed on your IOS device with the **show version** command. You can verify the amount of free flash memory with the **show flash** or **dir** command.

If you want to upgrade your IOS or load a previously saved IOS image, you’ll need to place the IOS image on an external server (such as TFTP or FTP) and use the **copy URL flash** command. You’ll be prompted for the same information you needed when you used the **copy flash tftp** command; however, the process that takes place after you enter your information is different. After you enter your information, IOS first verifies that the image exists on the TFTP server. If the file exists on the server, IOS then asks you if you want to erase flash. Answer **y** if you don’t have enough space in flash for the older image(s) as well as the new one. If you answer **y**, flash is erased and reprogrammed; as this step proceeds, you will see a line of e’s on the screen.

After flash is initialized, your router pulls the IOS image from the TFTP server. Just as in the copy operations with configuration files, a bunch of exclamation marks indicates successful copies, while periods indicate unsuccessful copies.