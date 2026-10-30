One final piece of information you should be aware of before the exam is the use of signed URLs. This option allows you to grant access to a visitor so that they can upload or download from storage without the need for a Google account. Access can be time-limited, and access can be granted for reads or writes to anyone who has the URL. We can create a signed URL with gsutil by running the following command:

```
gsutil signurl -d 10m Desktop/private-key.json gs://example-bucket/cat.jpeg
```

In this example, we are using private-key.json as our service account private key to expose the cat.jpeg bucket object from example-bucket.

> Exam Tip
> Remember that we should never grant a user or service more permissions than what's required. Always use the principle of least privilege.

Next, we will look at Organization Policy Service.

# Organization Policy Service

Organization Policy Service gives us programmatic control over our organization's cloud resources. This allows for centralized control to configure how our organization's resources are configured. Additionally, it helps our development teams stay within compliance boundaries and helps project owners move quickly without worrying about or breaking compliance.

It is important to highlight that Organization Policies are different from IAM, which we have discussed previously. While IAM focuses on allowing administrators to authorize who can act on specific resources based on permissions, Organization Policies focuses on allowing administrators to set restrictions on specific resources to determine how they can be configured. An organization policy is a set of restrictions and as an administrator, we define the policy to enforce restrictions on organizations, folders, or projects.