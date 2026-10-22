

用于管理云存储的 Cloud Console 是一个执行管理任务的绝佳选择。典型的管理任务包括：

- 创建和删除存储桶
- 上传、下载和删除对象
- 管理身份和访问管理策略，例如允许 allUsers 访问提供对象公共访问的存储桶

如前所述，基于对象的存储使用扁平命名空间来存储您的数据。Cloud Console 界面的关键功能是它将此扁平命名空间翻译为复制文件夹层次结构。Cloud Storage 没有文件夹的概念。

## sample-42892

Objects Overview Permissions Bucket Lock

Upload files Upload folder Create folder Manage holds Delete

Filter by prefix...

Buckets / sample-42892 / folder

| Name | Size | Type | Storage class | Last modified | Public access | Encryption | Retention expiration date | Holds |
|---|---|---|---|---|---|---|---|---|
| image.png | 2.52 MB | image/png | Standard | 6/21/20, 8:56:39 PM UTC-4 | Not public | Google-managed key | - | None |

*Figure 2-1. Console Storage Folder Screenshot*

Figure 2-1 是显示名为 sample-42892 的存储桶、名为 folder 的文件夹以及此文件夹内名为 image.png 的文件的屏幕截图。如前所述，文件和文件夹是一个对象，并且包含在扁平命名空间中。Figure 2-2 和 Figure 2-3 显示了 gsutil 命令行复制 Figure 2-1 中的屏幕截图。

```
ruicosta@Ruis-MBP-2 ~
ruicosta@Ruis-MBP-2 ~ gsutil stat gs://sample-42892/folder/
gs://sample-42892/folder/:
Creation time:      Mon, 22 Jun 2020 00:56:20 GMT
Update time:        Mon, 22 Jun 2020 00:56:20 GMT
Storage class:      STANDARD
Content-Length:     11
Content-Type:       text/plain
Hash (crc32c):      XkI+Dw==
Hash (md5):         apnFdauH+MfR7R5S5+NJzg==
ETag:               COPgrrCbI0oCEAE=
Generation:         1592787380973667
Metageneration:     1
ruicosta@Ruis-MBP-2 ~
```

Figure 2-2. Command Line Usage of gsutil

![](img/eac8762c24ec77948aa0c3277f8e6eb3_46_0.png)

Figure 2-3. Command Line Usage of gsutil

Review Figure 2-2 and 2-3. What is different between the two? Not much except their content-type. gsutil displays a hierarchical file structure but the underlying Cloud Storage is a flat namespace. To the service, the object gs://sample-42892/folder/image.png is just an object that happens to have "/" character in its name. There is no "folder" directory; just a single object with the given name. If we try to run gsutil stat gs://sample-42892/folder, what would happen? It would fail, since we did not include "/" in the name, see Figure 2-4.

![](img/eac8762c24ec77948aa0c3277f8e6eb3_47_0.png)

Figure 2-4. Command Line Usage of gsutil

## Create a Bucket in the Cloud Console

要创建存储桶：

1. 打开 Google Cloud Console 中的 Cloud Storage 浏览器：
   Storage >
2. 点击 Create bucket：
   CREATE BUCKET
3. 输入您的存储桶信息并点击 Continue：
   a. 输入存储桶的唯一 Name
   b. 为 **Location type** 选择 **Region**
   c. 为 **default storage class** 选择 **Standard**
   d. 为 **Access control** 选择 **Uniform**

4. 点击 Create：

![](img/eac8762c24ec77948aa0c3277f8e6eb3_48_0.png)

恭喜您创建了第一个存储桶！

## Upload a File in the Cloud Console

1. 在 Cloud Storage 浏览器中，点击您创建的存储桶的名称。
2. 点击标签中的 **Upload files** 按钮：
3. 在文件对话框中，导航到您想要上传的文件并选择它。
4. 上传完成后，您应该会看到文件名和有关文件的信息。

![](img/eac8762c24ec77948aa0c3277f8e6eb3_48_1.png)

## Download the file in the Cloud Console

要从存储桶下载图像：

1. 点击与文件关联的下拉菜单。下拉菜单显示为最右侧的三个垂直点。
2. 点击 **Download**。

![](img/eac8762c24ec77948aa0c3277f8e6eb3_48_2.png)

就这样，您已创建了一个存储桶，上传了一个文件并下载了它！

## Using the gsutil Tool

gsutil 是一个 Python 应用程序，可让您从命令行访问 Cloud Storage。您可以使用 gsutil 执行各种管理任务，包括：

- 创建和删除存储桶。
- 上传、下载和删除对象。
- 列出存储桶和对象。
- 移动、复制和重命名对象。
- 编辑对象和存储桶访问控制列表 (ACL)。
- ACL 允许您设置限制，例如读取、写入或删除对象。

如果您选择在桌面上本地运行它，则需要安装 gsutil。您可以访问以下链接了解如何安装 gsutil 应用程序：
https://cloud.google.com/storage/docs/gsutil_install

## Create a Bucket with gsutil

运行以下命令创建存储桶，请注意以下标志：

- -p: 为新存储桶设置项目 ID
- -c: 设置存储桶的默认存储类别
- -l: 设置存储桶的位置。例如，US-EAST1。
- -b: 为存储桶启用统一存储桶级访问。

```
gsutil mb gs://[BUCKET_NAME]/
```

## Upload a File with gsutil

以下命令将文件上传到相应的存储桶位置。

```
gsutil cp [LOCAL_OBJECT_LOCATION] gs://[DESTINATION_BUCKET_NAME]/
```

## Download the File with gsutil

以下命令将从命令行中设置的位置下载对象到指定的本地位置。

```
gsutil cp gs://[BUCKET_NAME]/[OBJECT_NAME] [SAVE_TO_LOCAL_LOCATION]
```

## NOTE

您会注意到我们没有使用 -p 标志，该标志说明要创建存储桶的项目。如果您已使用 gcloud 设置默认项目，则可以避免使用 -p 标志，如下所示：

```
gcloud config set project PROJECT
```

## Using Client Libraries

要在应用程序中集成执行管理任务以及用户任务的能力，您需要使用客户端库。Google Cloud 为 C++、C#、Go、Java、Node.js、PHP、Python 和 Ruby 提供客户端库。我们的框架将利用 Node.js 的客户端库。当音频文件上传到 Cloud Storage 时，它会触发 google.storage.object.finalize 事件，该事件执行我们的云函数（见 Figure 2-5）。

![](img/eac8762c24ec77948aa0c3277f8e6eb3_50_0.png)

## Upload Audio File

1

Cloud Function

## Trigger Function

2

## Cloud Storage

Figure 2-5. Cloud Storage Object Trigger

Follow along with instructions using client libraries

## NOTE

您现在可以在本地工作站或稍后使用 Qwiklabs 进行练习。本书的 Qwiklabs Quest 即将推出。

要在您自己的 Google Cloud 项目中跟进，请按照以下说明操作。

1. 创建一个 Cloud Platform 项目
2. 为您的项目启用计费
3. 启用 Google Cloud Storage API
4. 使用服务账号设置身份验证，以便您可以从本地工作站访问 API
   a. 在 Cloud Console 中，转到 IAM & Admin > Service Accounts
   b. 从 Service account 列表中，选择 New service account。
   c. 在 Service account name 字段中，输入名称。
   d. 从 Role 列表中，选择 Project > Owner。
   在生产环境中，您希望采用最小权限原则，并且不希望将服务账号分配为项目所有者。
   e. 点击 Create。一个包含密钥的 JSON 文件将下载到您的计算机。
   f. 我们将使用此 JSON 来授权我们的示例代码访问 Cloud Storage API。

## Create a Bucket

下面的代码提供了如何使用 Node.js cloud storage 客户端库创建存储桶的示例。它将在开发者定义的位置创建存储桶。在此代码片段中，存储桶名称定义为 "my-bucket"（第 1 行）。

```
const bucketName = 'my-bucket';
// Imports the Google Cloud client library
const {Storage} = require('@google-cloud/storage');
// Creates the client
const storage = new Storage();
async function createBucket() {
  const [bucket] = await storage.createBucket(bucketName, {
    location: 'US-CENTRAL1',
    regional: true,
  });
  console.log(`Bucket ${bucket.name} created.`);
}
createBucket().catch(console.error);
```

从第 7 行开始，您会注意到第二个参数是一个对象，其中包含我们传递给存储客户端的元数据，告诉存储客户端在相应位置创建我们的存储桶。您也可以按如下方式传递对象：

```
const bucketName = 'my-bucket';
// Imports the Google Cloud client library
const {Storage} = require('@google-cloud/storage');
// Creates the client
const storage = new Storage();
const metadata = {
  location: 'US-CENTRAL1',
  regional: true
};
async function createBucket() {
  const [bucket] = await storage.createBucket(bucketName, metadata);
  console.log(`Bucket ${bucket.name} created.`);
}
createBucket().catch(console.error);
```

在此代码块中，我们在第 6 行创建一个变量来保存要传递给存储客户端的参数。第 11 行现在包含第 6 行创建的 metadata 对象，该对象将被传递到我们的创建存储桶请求。以下是您可以在创建存储桶请求期间使用的元数据选项表：

