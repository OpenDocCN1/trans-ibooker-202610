3. Enter a resource prefix name, such as `CloudConfig`:

# Deploy Windows 10 in cloud configuration

Introduction  **2 Basics**  3 Apps  4 Assignments  5 Review + deploy

# Autopilot device name template

Devices will be configured to enroll with Windows Autopilot. You can apply a device name template to organize your devices.

Apply device name template — No / Yes

Create a unique name for your devices. Names must be 15 characters or less, and can contain letters (a-z, A-Z), numbers (0-9), and hyphens. Names must not contain only numbers. Names cannot include a blank space. Use the `%SERIAL%` macro to add a hardware-specific serial number. Alternatively, use the `%RAND:x%` macro to add a random string of numbers, where x equals the number of digits to add.

Enter a name * — `Win-%RAND:5%`

# Resource name prefix

Give a name to the resources that will be created and deployed as part of cloud config. The list at the bottom of this page shows how the name will appear next to each resource.

Enter a resource prefix name * — `CloudConfig`

![](bbox=[0.145, 0.128, 0.869, 0.586])

# Resources to be created

All these profiles will have the prefix selected:

+   - CloudConfig M365 (Teams)
+   - CloudConfig Microsoft Edge
+   - CloudConfig security baseline
+   - CloudConfig Autopilot profile
+   - CloudConfig ESP
+   - CloudConfig OneDrive Known Folder Move settings
+   - CloudConfig Microsoft Edge app settings