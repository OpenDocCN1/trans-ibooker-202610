Here is a screenshot of the command and its output:

![](bbox=[0.17, 0.13, 0.83, 0.305])

Figure 8.13 – Verifying the unauthenticated session

13. Download and verify the exfiltrated context file using the following command. Replace the <blob_container_url> placeholder with the value of Exfiltration Storage Location that you copied from the script's output:

```
Invoke-WebRequest -Uri "<blob_container_url>" -OutFile azureprofile.zip
```

```
Get-ChildItem azureprofile.zip
```

Here is a screenshot of the command and its output:

![](bbox=[0.17, 0.515, 0.83, 0.765])

Figure 8.14 – Downloading the extracted credential