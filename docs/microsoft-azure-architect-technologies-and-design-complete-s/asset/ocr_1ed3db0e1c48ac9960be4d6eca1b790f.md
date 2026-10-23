It is possible to have multiples of each service types within a single storage account.
A *blob* is typically a file, for example, an image, a video, or an audio file. These and any other file type can be stored in Azure Blob storage. The following are some common uses of Azure blobs:

+   - Support the upload of large files from a website.
+   - Back up and restore, archival or disaster recovery.
+   - Writing logs

There are numerous reasons to support the uploading of files from a form on a website. Consider YouTube, which supports the upload of very large files—files so large that an HTTP connection made for the upload would likely time out before the transmission of the data completes. Using the System.Web.UI.WebControls.FileUpload class isn’t an option in this scenario. Instead, one would use Microsoft.WindowsAzure.Storage.Blob.CloudBlobClient and upload the large file to an Azure Storage blob container that is designed specifically for this purpose, instead of storing it directly on the hard drive running the app.

Performing backups of a database or website and storing a VHD image of an IaaS instance as a block blob for quick recovery are also common scenarios for blobs. In case of a disaster situation, pulling data, content, or an image from a blob is a feasible recovery strategy. Writing logs to a blob file makes a lot of sense if there is a need to expose them to some other entity. For example, accessing IIS logs typically requires access to the server and then performing a manual effort to consolidate them. It is possible to configure Azure App Service to write IIS logs to a blob’s storage container. Accessing/analyzing them is then much more efficient, because the logs are in a single location and globally accessible.

The following are important topics to know in this context:

+   - Locally redundant storage (LRS)
+   - Zone-redundant storage (ZRS)
+   - Georedundant storage (GRS)
+   - Read-access geo-redundant storage (RA-GRS)
+   - Encryption at rest
+   - Immutable blobs
+   - Page and block blobs

Azure File storage is somewhat synonymous with what we commonly refer to as a *file share* that uses the Server Message Block (SMB) protocol. Historically in Windows Explorer, when you right-click a folder and select Share, then all those who are given access can read, write, and/or delete the content of that directory. That scenario, however, is restricted to those within an intranet. True, there are some configurations using WebDav via IIS that could provide access to internal shares externally. However, that configuration can be complicated to configure and maintain. Azure files simplify that by providing the same mapping capabilities as one would expect from doing what was just described globally.