The default enrollment status page applies to all users and all devices, but you can create multiple enrollment status page profiles with different configurations that are needed for your scenario.

These profiles are specified in priority order; the highest priority that is applicable will be used. Each enrollment status page profile can be targeted to groups containing devices or users. When determining which profile to use, the following criteria will be followed:

+   - The highest-priority profile targeted to the device will be used first.
+   - If there are no profiles targeted to the device, the highest-priority profile targeted to the current user will be used. (This only applies in scenarios where there is a user. In white-glove and self-deploying scenarios, only device targeting can be used.)
+   - If there are no profiles targeted to specific groups, then the default enrollment status page profile will be used.

The enrollment status page is not just applicable to Windows Autopilot devices but to all devices that are enrolled into Microsoft Intune that have it assigned either to users or devices.

We will cover Windows Autopilot in depth, including how to set it up and configure the enrollment status page for different scenarios, in the next chapter.

In the following screenshot, you can see how to capture devices on the Enrollment Status Page screen:

![](bbox=[0.16, 0.54, 0.8, 0.84])

Figure 6.3 – Enrollment Status Page screen