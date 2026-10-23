6. An image may have multiple versions stored in the container registry. We can enumerate the versions using tags. List the tags for a specific image in the container registry using the following command:

```
root@PentestVM:~# az acr repository show-tags -n $acr --repository nodeapp-web
```

You will see a single tag called v1. This information is useful as there may be multiple versions of the same image in the repository. It is not unusual for previous versions to contain sensitive credentials, but they have just not been cleaned up in the registry.

7. Make a note of the registry credentials that we will need in subsequent tasks. Run the following commands in the WSL session and make a note of them:

```
root@PentestVM:~# echo $loginserver
```

```
root@PentestVM:~# echo $accesstoken
```

Here is a screenshot of the command and its output:

![](bbox=[0.16, 0.45, 0.78, 0.73])

Figure 4.45 – Example variable output