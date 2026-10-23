# EXERCISE 3.10 (continued)

8. Click the + Add A Rule button, enter a rule name (for example, CSHARPGUITAR-WAF-HTTP80), enter a listener name (for example, CSHARPGUITAR-LISTENER), select Public from the Frontend IP drop-down list, select the Backend Targets tab, select CSHARPGUITAR-WAF-BACKEND-POOL from the Backend Target drop-down list, click the Create New link under the HTTP Setting drop-down list, provide the setting a name (for example, CSHARPGUITAR-HTTP-SETTING), and click the Add button.

9. Click the Next: Tags button, click the Next: Review + Create button, and click the Create button.

10. Once that’s created, open the Application Gateway blade for CSHARPGUITAR-AGWAF, copy its IP address and place it into a browser, and attempt to connect. If IIS is not installed on the VM in the backend pool, make a remote desktop connection using RDP and execute the following PowerShell cmdlets:

```
Install-WindowsFeature -name Web-Server -IncludeManagementTools
remove-item C:\inetpub\wwwroot\iisstart.htm
Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" `
    -Value $("Hello from " + $env:computername)
```

11. The request to the public IP address of the WAF routes to the Azure VM configured into the backend pool and responds with the “Hello from CSHARPGUITAR-V2” string.

There is one more configuration that needs more discussion—Firewall mode. You might have noticed it on the Basics tab in step 5 of Exercise 3.10. Firewall mode has two options: Detection and Prevention. As the names would lead one to believe, when detection is selected and an access matches a pattern similar to what is considered a malicious intrusion (for example, some JavaScript in a web form), it is logged. The request can flow through and be processed. I assume someone would want to do this if their application is for some reason designed in a way that an expected post matches a malicious pattern. Other times, you want to let the bad actors think they are going undetected but come back and get them later. Lastly, perhaps the application under attack has strong enough resistance to known vulnerabilities. As you can see, there are certainly numerous additional reasons for allowing the traffic through.

Prevention, on the other hand, will not let a request/post pass through the WAF if it is flagged as malicious. Instead, the client is sent an HTTPS status code of 403 – Forbidden. The attempt is still logged for later review and analysis, but the bad actor is immediately informed of their denial or failed attempt at potentially causing harm. Chapter 9 contains more details of monitoring. Azure does provide another firewall product called Azure Firewall; read on to learn about it.