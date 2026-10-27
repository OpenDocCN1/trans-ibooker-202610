gateway will forward it to the my-vmware-cluster. This means that no hybrid connectivity like Cloud VPN or Interconnect is required to deploy your Anthos cluster outside of GCP.

# Triggering Cloud Build

In the previous section we have learned how to deploy the application to any Anthos clusters. Now let’s have a look at how we can trigger a Cloud Build pipeline either by using gcloud command (which we have already looked at in the previous section) or automatic triggers. With Cloud Build you can use three types of repositories. Those are:

+   - Cloud Source Repositories
+   - GitHub
+   - Bitbucket

To create a trigger you can use both Google Cloud Console and command line. First add the repository to Cloud Build

```
gcloud beta builds triggers create cloud-source-repositories \\
    --repo=[REPO_NAME] \\
    --branch-pattern=".*" \\
    --build-config=[BUILD_CONFIG_FILE] \
```

and then add the trigger.

```
gcloud beta builds triggers create github \\
    --repo-name=[REPO_NAME] \\
    --repo-owner=[REPO_OWNER] \\
    --branch-pattern=".*" \\
    --build-config=[BUILD_CONFIG_FILE] \
```

With the --branch-pattern you can specify which branch will trigger the build. In this case it will be all branches.

If you would like know how to create triggers from the other repositories consult the documentation: [https://cloud.google.com/cloud-build/docs/automating-builds/create-manage-triggers](https://cloud.google.com/cloud-build/docs/automating-builds/create-manage-triggers)