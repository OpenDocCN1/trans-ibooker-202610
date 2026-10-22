| API or Feature | Class A Operations | Class B Operations | Free Operations |
|---|---|---|---|
| JSON API | storage.*.insert storage.*.patch storage.*.update storage.*.setIamPolicy storage.buckets.list storage.buckets.lockRetentionPolicy storage.notifications.delete storage.objects.compose storage.objects.copy storage.objects.list storage.objects.rewrite storage.objects.watchAll storage.projects.hmacKeys.create storage.projects.hmacKeys.list storage.*AccessControls.delete | storage.*.get storage.*.getIamPolicy storage.*.testIamPermissions storage.*AccessControls.list storage.notifications.list Each object notification | storage.channels.stop storage.buckets.delete storage.objects.delete storage.projects.hmacKeys.delete |
| XML API | GET Service GET Bucket PUT POST | GET Bucket GET Object HEAD | DELETE |

Let's take a step back, and cover file storage, block storage, and object storage.

# File Storage

File storage stores data in a hierarchical structure. The data is saved in files and folders, and it is presented identically whether from the system serving the view or from the system retrieving data view. File storage is accessed traditionally via the Network File System protocol or Server Message Block protocol.

# Block Storage

Block storage is presented as either Storage Area Networks or locally attached disks. Block storage is usually mounted on an operating system as if the block device was locally attached storage. Once mounted locally, the user can create a file system to be used by the operating system: application or databases.

# Object-Based Storage

Object-based storage is accessible programmatically versus block storage which can only be accessed via an operating system. Object-based storage is also flat in nature; it does not have the hierarchical structure of file systems. Cloud Storage has no notion of folders, a folder is an object that happens to have "/" character in its name. Each object contains the data as well as the associated metadata which is distributed across multiple hardware systems to provide scalability and reliability.

# Options for Getting Data to Cloud Storage

> NOTE
> You can practice along now or at a later time using Qwiklabs. The Qwiklabs Quest for this book is coming available soon.