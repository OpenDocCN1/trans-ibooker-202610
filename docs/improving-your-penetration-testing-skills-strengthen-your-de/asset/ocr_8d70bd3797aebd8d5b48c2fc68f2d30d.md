# How to do it...

Post modules are categorized based on their behavior, as shown in the following list from the official documentation:

| Category | Description |
|---|---|
| gather | Modules that involve data gathering/collecting/enumeration. |
| gather/credentials | Modules that steal credentials. |
| gather/forensics | Modules that involve forensics data gathering. |
| manage | Modules that modify/operate/manipulate something on the system. Session management-related tasks such as migration, injection also go here. |
| recon | Modules that will help you learn more about the system in terms of reconnaissance, but not about data stealing. Understand that this is not the same as gather type modules. |
| wlan | Modules that are for WLAN related tasks. |
| escalate | This is deprecated, but the modules remain there due to popularity. This used to be the place for privilege escalation modules. All privilege escalation modules are no longer considered as post modules, they're now exploits. |
| capture | Modules that involve monitoring something for data collection. For example, keylogging. |

Since our module will enumerate domain users, we should place it in the gather category, so our destination directory should be:

```
/usr/share/metasploit-framework/modules/post
/windows/gather/
```