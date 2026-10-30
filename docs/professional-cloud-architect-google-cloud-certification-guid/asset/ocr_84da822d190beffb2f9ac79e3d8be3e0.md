# Understanding IAM

Access to Google Cloud Storage is secured with IAM. Let's have a look at the following list of predefined roles and their details:

+   - **Storage Object Creator**: Has rights to create objects but does not give permissions to view, delete, or overwrite objects
+   - **Storage Object Viewer**: Has rights to view objects and their metadata, but not the ACL, and has rights to list the objects in a bucket
+   - **Storage Object Admin**: Has full control over objects and can create, view, and delete objects
+   - **Storage Admin**: Has full control over buckets and objects
+   - **Storage HMAC Key Admin**: Has full control over HMAC keys in a project and can only be applied to a project

# Quotas and limits

Google Cloud Storage comes with predefined quotas. These default quotas can be changed via the **Navigation** menu, under the **IAM & Admin | Quotas** section. From this menu, we can review the current quotas and request an increase to these limits. We recommend that you familiarize yourself with the limits of each service as this can have an impact on your scalability. For Cloud Storage, we should be aware of the following limits:

+   - Individual objects are limited to a maximum size of 5 TiB.
+   - Updates to an individual object are limited to one per second.
+   - There is an initial limit of 1,000 writes per second per bucket.
+   - There is an initial limit of 5,000 reads per second per bucket.
+   - There is a limit of 100 ACL entries per object.
+   - There is a limit of one bucket creation operation every 2 seconds.
+   - There is a limit of one bucket deletion operation every 2 seconds.