If you have a kiosk device, an ATM, or another device with no user on it, it is recommended to set it to **Turn off all notifications, including restart warnings**, as there are no end users to respond to the notifications on those devices. You can find the settings in the following screenshot:

Change notification update level ①  Turn off all notifications, including restart warnings

Figure 6.20 – Update ring policy notification level

**Use deadline settings** allows you to leverage deadline settings. The recommend setting is **Allow**. Leveraging the **Allow** setting gives you, as an IT administrator, the option to set deadlines for both feature and quality updates.

For **Deadline for feature updates**, the number of days allowed is 2 to 30. It is recommended to set it to **7**, so a Windows feature update will have a deadline of 7 days before it is forced to do the installation and reboot.

For **Deadline for quality updates**, the number of days allowed is 2 to 30. It is recommended to set it to **5**, so a Windows quality update will have a deadline of 5 days before it is forced to do the installation and reboot. This will not prevent the end user from installing the quality updates before the deadline has reached the end.

For **Grace period**, the number of days allowed is 0 to 7. It is recommended to set it to **2** days, so if an end user comes back from vacation, they will have 2 days to get the updates installed and restart their device.

For **Auto reboot before deadline**, the recommended setting is **Yes**. This specifies whether the device should auto-reboot before the deadline. **Yes** will ensure that the reboot happens with as little end user interaction as possible. **No** will ensure that the end user is always present on the devices for a reboot.

Use deadline settings ①  Allow  Not configured
Deadline for feature updates ①  7
Deadline for quality updates ①  5
Grace period ①  Number of days, 0 to 7
Auto reboot before deadline ①  Yes  No

Figure 6.21 – Update ring policy Use deadline settings