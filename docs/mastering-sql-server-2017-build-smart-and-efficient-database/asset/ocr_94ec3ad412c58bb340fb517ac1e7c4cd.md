7. Now we'll test if the cluster creation works. Right-click on the task and select Execute Task. The cluster creation starts. This task might take several minutes to complete. In our case, it takes up to 15 minutes.

8. Once completed, open a bash terminal. We will use the one that comes with Windows 10. Go to the Azure portal and look for the HDInsight cluster that has been created. In the overview, there is an option to connect using SSH. Click on it and copy the SSH command. Paste it in the bash terminal. It should consist of a command similar to the following:

```
ssh User@yourcluster-ssh.azurehdinsight.net
```

It is also shown in the following screenshot:

![](bbox=[0.105, 0.34, 0.895, 0.72])