Exam Tip
At least one condition is required. If you enter an incorrect action or condition, then you will receive a 400 bad request error response.

Next, let's check out retention policies and locks.

# Retention policies and locks

Bucket locks allow us to configure data retention policies for a Cloud Storage bucket. This means we can govern how long objects in the bucket must be retained and prevent the policy from being reduced or removed.

We can create a retention policy by running the following command:

```
gsutil retention set 60s "gs://<bucketname>"
```

This sets the policy for 60 seconds. We can also set it to 60d for 60 days, 1m for 1 month, or 10y for 10 years. The output of setting this policy will be similar to the following:

```
Retention Policy (UNLOCKED):
    Duration: 60 Second(s)
    Effective Time: Thu, 01 Jul 2021 05:49:05 GMT
```

Note that if a bucket does not have a retention policy, then we can delete or replace objects at any time. However, if a policy is applied, then the objects in the bucket can only be replaced or deleted once their age is older than the retention policy period.

Exam Tip
Retention policies and object versioning are exclusive features, which means that for a given bucket, only one of these can be enabled at a time.

# Object holds

Object holds are metadata flags that can be placed on individual objects, which means they cannot be replaced or deleted. We can, however, still edit the metadata of an object that has been placed on hold.