4. All the profiles will now be created:

Home > Troubleshooting + support >

# Deploy Windows 10 in cloud configuration

Deployment succeeded.

# Deployment details

| Resource | Resource Type | More | Resource | Assignment |
|---|---|---|---|---|
| CloudConfig | AAD Security Group | Docs | Created | -- |
| CloudConfig M365 (Teams) | M365 App Suite | Docs | Created | Assigned |
| CloudConfig Microsoft Edge | App | Docs | Created | Assigned |
| CloudConfig security baseline | Windows 10 security baseline | Docs | Created | Assigned |
| CloudConfig Autopilot profile | Autopilot profile | Docs | Created | Assigned |
| CloudConfig ESP 20210419_0:04:18 | Enrollment Status Page | Docs | Created | Assigned |
| CloudConfig OneDrive Known Folder Move | Administrative template | Docs | Created | Assigned |
| CloudConfig Microsoft Edge app settings | Administrative template | Docs | Created | Assigned |
| CloudConfig compliance policy | Compliance policy | Docs | Created | Assigned |
| CloudConfig built-in app removal script | Script | Docs | Created | Assigned |
| CloudConfig update ring 20210419_0:04:18 | Windows 10 update ring | Docs | Created | Assigned |

Figure 7.30 – Cloud configuration deployment succeeded

Here is what can you do next:

+   - Add devices to the group you configured.
+   - Add your preregistered Autopilot devices or other existing devices to the group you configured. For existing devices, we recommend removing other profiles and apps and resetting them, so they start fresh with just the cloud configuration applied.
+   - Deploy essential line-of-business apps and configurations.

We recommend keeping additional essential configurations to a minimum, including the number of line-of-business apps you deploy on top of the cloud configuration. This helps keep device management and troubleshooting simpler.