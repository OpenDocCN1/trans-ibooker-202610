While this is a test system in our environment, you may want to avoid adding additional tools to a system that you have compromised in a test. In this case, jq helps us to get a well-formatted JSON output. Here is a screenshot of the command and its output:

![](bbox=[0.21, 0.18, 0.77, 0.47])

Figure 4.27 – Example metadata service reply

The output will contain information about the VM's compute, network, disk, and other configurations.

+   5. Check whether the VM has an associated managed identity using the following command. If one is used, an access_token will be returned and this could present an opportunity to escalate privileges:

```
root@LinuxVM:~# curl -H Metadata:true -s 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' | jq
```

Here is a screenshot of the command and its output:

![](bbox=[0.21, 0.73, 0.82, 0.87])

Figure 4.28 – Example metadata token output