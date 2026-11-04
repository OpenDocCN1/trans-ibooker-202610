# Reduce the number of higher IOPS volumes

Volumes with higher IOPS guarantees (e.g., AWS provisioned IOPS volumes or Azure premium disk) aren’t cheap, and it’s relatively easy to change them. Using historic metrics to locate underutilized disks and having engineers change them where possible can greatly reduce disk costs.

# Take advantage of elastic volumes

When you use AWS EBS, a volume can increase in size, adjust performance, or change volume type while the volume is in use. This can be done while an application is actively driving I/O to your volume. There’s no need for a maintenance window or downtime. There’s a big cost savings benefit, because you no longer have to overprovision your storage. Often engineering organizations will be far more open to taking recommendations that require no downtime, as they don’t have to deal with change tickets, downtime windows, or the risk of having to take additional downtime if they want to revert the size change.

# Object Storage

Often referred to as unlimited storage, object storage services are highly likely to be the location with the majority of your data. Object storage services include AWS’s S3, Google Cloud’s Cloud Storage buckets, and Azure’s Blob Storage.

# Implement data retention policies

The ease of storing data into object storage services and the fact that these data stores are theoretically limitless makes it easy for engineers to store data forever. In the data center, there was available disk space as a constraint and data retention policies had to be implemented to avoid unbounded storage growth. Moving into the cloud removes the challenges of making storage capacity available, but it should not remove the need for data retention policies.

# Choose a storage tier/class that matches the data

By default, data sent to object storage services is often stored in the standard/hot storage class. Standard storage is usually the most expensive storage class but has the most availability and durability. Classifying your data and selecting the right storage class for the data type can save you significant amounts on object storage. Some cloud service providers provide automated classification and data class selection, such as AWS’s Intelligent-Tiering.