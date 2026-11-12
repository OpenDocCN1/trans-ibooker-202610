```
IOS# dir
Directory of flash:/
  1 drw-           0  Sep 18 2007 15:42:20 +00:00  .Trash es
  2 -rw-        4096  Sep 18 2007 15:42:20 +00:00  .__.Trash es
  .
  .
  .
 16 -rw-    23787192  Sep 9 2007 17:45:30 +00:00
                    c1841-advipservicesk9-mz.124-6.T7.bin
63868928 bytes total (31946752 bytes free)
```

In this example, the amount of available flash is 31,946,752 bytes—you need to ensure that there is enough flash available before adding files to flash.

![](bbox=[0.14, 0.39, 0.22, 0.45])

**VIDEO 8.03.** The digital resources that accompany this book contain a multimedia demonstration of viewing the contents of flash memory.

# Naming Conventions for IOS Images

Cisco has implemented a naming convention for its IOS images, which enables you to see the platform, software version, and features included in the image just by looking at the image filename. As an example, consider the image name from the preceding show flash command, c1841-advipservicesk9-mz.124-6.T7.bin, which is from a router. Here’s an explanation of the nomenclature that Cisco uses for IOS image filenames:

+   - **c1841** Refers to the name of the platform on which the image will run. This is important, because different router models have different processors, and an image compiled for one processor or router model will typically *not* run on a different model. Therefore, it is very important that you load the appropriate image on your device.
+   - **advipservicesk9** Refers to the features included in this IOS version, commonly referred to as the *feature set*. In this example, IOS is the