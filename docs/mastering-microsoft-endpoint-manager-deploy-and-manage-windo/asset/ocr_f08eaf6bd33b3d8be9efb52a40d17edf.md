# Add Apps

Search by name, publisher

| Name | Publisher | Version | Superseded Count |
| Notepad++ | Contoso | 7.8.5 | 0 |
| Remote Desktop | Contoso | 1.2.1755.0 | 0 |

Selected supersedence:

| Notepad++ | Contoso | 7.8.5 | 0 | Remove |

Figure 8.32 – Add Apps supersedence

Assign the application to an (AAD) group or all devices, to enforce deployment to your physical Windows PCs/endpoints:

Home > Apps > Windows >
Add App
Windows app (Win32)

Detection rules
Dependencies
Supersedence (preview)
Assignments

Any Win32 app deployed using Intune will not be automatically removed from the device when the device is retired. The app and the data it contains will remain on the device. If the app is not removed prior to retiring the device, the end user will need to take explicit action on the device to remove the app.

Required

| Group mode | Group | End user notifications | Availability | Installation deadline | Resta |
| No assignments | | | | | |

+ Add group + Add all users + Add all devices

Available for enrolled devices

| Group mode | Group | End user notifications | Availability | Restart grace period |
| No assignments | | | | |

+ Add group + Add all users

Previous Next

Figure 8.33 – Add App – Assignments