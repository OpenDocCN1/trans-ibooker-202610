To uninstall (remove) a license from a router, you need to perform two actions:

+   1. Disable the technology package.
+   2. Clear the license.

To disable the technology package, use the **license boot module** command. Here’s an example of disabling the Unified Communications package on a 3900 series router:

```
```
Router(config)# license boot module c3900 technology-package uck9 disable
Router(config)# exit
Router# copy running-config startup-config
Router# reload
```
```

Once the router has rebooted, you need to clear the license with the **license clear** command. Here’s an example:

```
```
Router# license clear uck9
*Jul  7 00:34:23.691: %SYS-5-CONFIG_I: Configured from console by console
clear uck9
Feature: uck9
  1   License Type: Permanent
      License State: Active, Not in Use
      License Addition: Exclusive
      License Count: Non-Counted
      Comment:
      Store Index: 15
      Store Name: Primary License Storage
Are you sure you want to clear? (yes/[no]) : yes
*Jul  7 00:34:31.223: %LICENSE-6-REMOVE: Feature uck9 1.0 was removed
from this device.
UDI=C3900-SPB150/K9:FHH12250057; StoreIndex=15:Primary License Storage
```
```

Once done, you can re-enable the technology package:

```
```
Router(config)# no license boot module c3900 technology uck9 disable
Router(config)# exit
Router# reload
```
```

Upon rebooting, use the **show version** command to verify the license change.