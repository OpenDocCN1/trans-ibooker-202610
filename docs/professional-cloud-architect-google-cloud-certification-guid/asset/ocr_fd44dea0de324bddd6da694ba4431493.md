We can also use the gcloud CLI to add permissions. This command line allows us to manage interactions with GCP APIs. As an example, if I want to assign the editor role to brian@brgerrard.co.uk within the redwing project, then I can run the following command:

```
gcloud projects add-iam-policy-binding redwing -
member='user:brian@brgerrard.co.uk' -role='roles/editor'
```

The output of this command will confirm that brian@brgerrard.co.uk has been granted the role of editor.

This gcloud projects command line can also be used for many more functions. Please note that we will take a deeper dive into the gcloud command line in Chapter 16, Google Cloud Management Options.

> **Important Note**
> Please refer to the official documentation if you are keen on using this in more depth: https://cloud.google.com/sdk/gcloud/reference/projects/.

Finally, regarding IAM, we have spoken about who has permission to perform actions on resources. We should also be mindful of what organizational policies focus on. These policies allow us to set restrictions on resources. An organizational policy would allow us to disable certain options that are available to a user. These restrictions are done using constraints and can be applied to a GCP service or a list of GCP services. For example, let's say that we didn't want a default network to be available. We could set a constraint to skip default network creation, thus preventing any new VM instances on this network.

Previously, we mentioned the importance of service accounts. We will look at these in more depth now.

# Service accounts

Service accounts are used to call the API of a service, hence removing users from any direct involvement. They belong to an application or VM instance. By default, every GCP project we create will have a default service account created when we enable our projects to use Compute Engine:

+   1. We can create a new service account by navigating to **IAM & Admin | IAM | Service accounts** on our GCP console. We can also create a new service account using the gcloud CLI. As an example, let's say we need to have a VM that has access to Cloud Storage. The following screenshot shows us creating a new service account named AccessCloudStorage: