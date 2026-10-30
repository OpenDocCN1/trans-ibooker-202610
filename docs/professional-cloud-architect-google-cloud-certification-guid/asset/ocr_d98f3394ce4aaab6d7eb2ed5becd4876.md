Service accounts are managed using service account keys. There are two types of service account keys:

+   - **Google Cloud-managed**: Google Cloud-managed keys are used by services such as Compute Engine and App Engine and cannot be downloaded.
+   - **User-managed**: User-managed keys are created and can be downloaded and managed by users of GCP. You are fully responsible for user-managed keys.

We will speak about key management in the *Encryption* section of this chapter.

Before we finish talking about service accounts, one final thing to note is that service accounts should still follow the same principles we learned about for IAM. Service accounts should only get the minimum set of permissions required for that service.

> **Exam Tip**
> For the exam, this topic is key so that you understand identities, roles, and resources. We have briefly touched on IAM in previous chapters, but now, we want to look at it in a bit more detail. Some principles to note before you read any further, which will be explained in more detail in this chapter, are as follows: IAM roles are groups of permissions that can be assigned to users, groups, or service accounts; there are different types of roles; we should follow the principle of least privilege; and use groups to control access rather than granting access to individual users, if possible.

In this section, we covered service accounts. We explained that they are a special type of account that removes a user from direct involvement and that they are an important part of Google IAM. In the next section, we will look at access control lists, which are an alternative to IAM.

# Cloud Storage access management

On top of using IAM permissions to restrict access to Cloud Storage, it can also be secured using **Access Control Lists (ACLs)**. ACLs should be used when you want to set permissions on objects rather than the whole bucket, for example, to gain access to an individual object in a specific bucket. This is because Cloud IAM would apply permissions to all of the objects in a bucket. It's vital to understand this because, from a manageability perspective, it is not ideal to manage these individually, and if there is a commonality of permissions across all objects in the bucket, then you should use IAM to control access.