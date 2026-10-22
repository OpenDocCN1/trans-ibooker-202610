The Cloud Console to manage cloud storage is a great option to perform management tasks. Typical management tasks include:

+   - Creating and deleting buckets
+   - Uploading, downloading, and deleting objects
+   - Managing Identity and Access Management policies such as allowing allUsers access to a bucket that provides public access to an object

As mentioned earlier object-based storage uses a flat namespace to store your data. The key functionality of the Cloud Console interface is that it translates this flat namespace to replicate a folder hierarchy. Cloud Storage has no notion of folders.

# sample-42892

Objects Overview Permissions Bucket Lock

Upload files Upload folder Create folder Manage holds Delete

Filter by prefix...

Buckets / sample-42892 / folder

| Name | Size | Type | Storage class | Last modified | Public access | Encryption | Retention expiration date | Holds |
|---|---|---|---|---|---|---|---|---|
| image.png | 2.52 MB | image/png | Standard | 6/21/20, 8:56:39 PM UTC-4 | Not public | Google-managed key | - | None |

*Figure 2-1. Console Storage Folder Screenshot*

Figure 2-1 is a screenshot that displays a bucket called sample-42892, a folder called folder, and a file within this folder called image.png. As mentioned, a file and a folder is an object and it's contained within a flat namespace. Figure 2-2 and Figure 2-3 show the gsutil command-line replicating the screenshot in Figure 2-1.