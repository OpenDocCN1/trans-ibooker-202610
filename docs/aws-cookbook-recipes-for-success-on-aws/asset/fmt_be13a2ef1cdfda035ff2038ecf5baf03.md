
# 3.2 Using S3 Intelligent-Tiering Archive Policies to Automatically Archive S3 Objects

## Problem

You need to automatically transition infrequently accessed objects to a different archive storage class without impacting performance or adding operational overhead.

## Solution

Create a policy to automate the archival of `S3` objects to the `S3 Glacier` archive based on access patterns for objects that are more than 90 days old. Apply it to your `S3` bucket, as shown in [Figure 3-2](#ch03.html#sthree_intelligent_tiering_archive).

![](assets/awsc_0302.png)

### Prerequisite

*   An `S3` bucket

### Preparation

Follow the steps in this recipe’s folder in the [chapter code repository](https://github.com/AWSCookbook/Storage).

### Steps

1.  Create a file named `tiering.json` for the configuration (file provided in the repository):

    ```json
    {
      "Id": "awscookbook302",
      "Status": "Enabled",
      "Tierings": [
        {
          "Days": 90,
          "AccessTier": "ARCHIVE_ACCESS"
        }
      ]
    }
    ```

2.  Apply the Intelligent-Tiering configuration:

    ```
    aws s3api put-bucket-intelligent-tiering-configuration \
        --bucket awscookbook302-$RANDOM_STRING \
        --id awscookbook302 \
        --intelligent-tiering-configuration "$(cat tiering.json)"
    ```

    #### Warning

    Ensure that your use case and applications can support the increased retrieval time associated with the `S3 Glacier` archive storage tier. You can configure your application to use an expedited retrieval mechanism supported by `S3 Glacier` archive to decrease the retrieval time but increase cost. For a complete list of archive times and how to configure expedited access, please refer to [the support document](https://oreil.ly/tru0v).

#### Validation checks

Get the configuration of Intelligent-Tiering for your bucket:

```
aws s3api get-bucket-intelligent-tiering-configuration \
    --bucket awscookbook302-$RANDOM_STRING \
    --id awscookbook302
```

Copy an object to the bucket:

```
aws s3 cp ./book_cover.png s3://awscookbook302-$RANDOM_STRING
```

Check the storage class for the object:

```
aws s3api list-objects-v2 --bucket awscookbook302-$RANDOM_STRING
```

### Cleanup

Follow the steps in this recipe’s folder in the [chapter code repository](https://github.com/AWSCookbook/Networking).

## Discussion

An `S3 Intelligent-Tiering` archive provides an automatic mechanism to transition “cool” (less frequently accessed) objects to an [`S3 Glacier` archive](https://aws.amazon.com/s3/glacier). You can define the length of time required for an object to transition to the archive (between 90 and 730 days). This feature helps with meeting long-term retention requirements that you may have for compliance. The storage tiers available within `S3 Intelligent-Tiering` map directly to `S3` tiers:

`Frequent Access`
:   Optimized for frequent access (`S3 Standard`)

`Infrequent Access`
:   Optimized for infrequent access (`S3 Standard-IA`)

`Archive Access`
:   Archive purposes (`S3 Glacier`)

`Deep Archive Access`
:   Long-term retention purposes (`S3 Glacier Deep Archive`)

This archive configuration is separate from the main `S3 Intelligent-Tiering` tier configuration that you place on objects, as this is a bucket-specific configuration. In the previous recipe, you configured a Lifecycle rule to configure all objects within a bucket to transition to the `S3 Intelligent-Tiering` storage tier. This recipe adds additional configuration to transition objects to `S3` archive tiers based on your configuration. You can use either of these methods separately or both concurrently to meet your own requirements.

#### Note

`S3` tiers are object-specific, which differs from the `Intelligent-Tiering` archive being bucket-specific. You can filter an archive configuration to apply only to certain prefixes, object tags, and object names if you wish to include or exclude objects in a configuration. For more information, see the [support document](https://oreil.ly/WIpqW).

### Challenge 1

Configure the `Intelligent-Tiering` archive to send objects that are older than one year to the `Glacier Deep Archive` tier.

### Challenge 2

Configure the `Intelligent-Tiering` archive to use object-level tags and configure an object with a tag that matches your configuration.

# 3.3 Replicating S3 Buckets to Meet Recovery Point Objectives

## Problem

Your company’s data security policy mandates that objects be replicated within the same Region to meet a recovery point objective of 15 minutes.

## Solution

First, create source and destination `S3` buckets with versioning enabled. Then create an `IAM` role and attach an `IAM` policy that allows `S3` to copy objects from the source to the destination bucket. Finally, create an `S3` replication policy that references the `IAM` role, and apply that policy to the source bucket, as shown in [Figure 3-3](#ch03.html#sthree_bucket_replication).

![](assets/awsc_0303.png)

### Prerequisite

*   An `S3` bucket with versioning enabled that you will use as your source

### Preparation

Follow the steps in this recipe’s folder in the [chapter code repository](https://github.com/AWSCookbook/Networking).

### Steps

1.  Create the destination `S3` bucket:

    ```
    aws s3api create-bucket --bucket awscookbook303-dst-$RANDOM_STRING
    ```

2.  Enable versioning for the destination `S3` bucket:

    ```
    aws s3api put-bucket-versioning \
        --bucket awscookbook303-dst-$RANDOM_STRING \
        --versioning-configuration Status=Enabled
    ```

3.  Create a file named `s3-assume-role-policy.json` with the following content (file provided in the repository):

    ```json
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Effect": "Allow",
          "Principal": {
            "Service": "s3.amazonaws.com"
          },
          "Action": "sts:AssumeRole"
        }
      ]
    }
    ```
