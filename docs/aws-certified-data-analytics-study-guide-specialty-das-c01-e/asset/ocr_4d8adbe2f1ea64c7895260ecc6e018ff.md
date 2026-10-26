# How to Use a File Gateway

To use a file gateway, you start by downloading a VM image for the file gateway. You then activate the file gateway from the AWS Management Console or through the Storage Gateway API. You can also create a file gateway using an Amazon EC2 image. After the file gateway is activated, you create and configure your file share and associate that share with your Amazon S3 bucket. Doing this makes the share accessible by clients using either the NFS or SMB protocol. Files written to a file share become objects in Amazon S3, with the path as the key.

There is a one-to-one mapping between files and objects, and the gateway asynchronously updates the objects in Amazon S3 as you change the files. Existing objects in the bucket appear as files in the file system, and the key becomes the path.

Objects are encrypted with Amazon S3–server-side encryption keys (SSE-S3). All data transfer is done through HTTPS. The service optimizes data transfer between the gateway and AWS using multipart parallel uploads or byte-range downloads, to better use the available bandwidth. Local cache is maintained to provide low latency access to the recently accessed data and reduce data egress charges.

CloudWatch metrics are available and can provide insight into resource use on the VM and data transfer to and from AWS. CloudTrail tracks all API calls. With file gateway storage, you can do such tasks as ingesting cloud workloads to S3, performing backups and archiving, tiering, and migrating storage data to the AWS Cloud.[1]

Figure 3.9 shows the file storage deployment of a storage gateway.

# FIGURE 3.9 File Storage Deployment – storage gateway

![](bbox=[0.115, 0.6, 0.88, 0.75])

# Volume Gateway

A volume gateway provides AWS storage-backed storage volumes that you can mount as iSCSI devices from your on-premises application services. There are two types of configurations for volume gateway, cached and stored.

# Cached Volumes

As the name indicates, cached volumes store the data in Amazon S3, but the frequently accessed data is stored locally. While providing the cost benefits of Amazon S3, it also provides a low latency access for applications that need lower SLAs. Cached volumes range