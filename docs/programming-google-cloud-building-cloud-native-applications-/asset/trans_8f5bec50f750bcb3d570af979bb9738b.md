

| Name | Type | Description |
|---|---|---|
| archive | boolean | 存储类别为 Archive |
| coldline | boolean | 存储类别为 Coldline |
| dra | boolean | 存储类别为 Durable Reduced Availability。 |
| multiRegional | boolean | 存储类别为 Multi-Regional。 |
| nearline | boolean | 存储类别为 Nearline。 |
| regional | boolean | 存储类别为 Regional。 |
| standard | boolean | 指定存储类别为 Standard。 |

创建存储桶时，一个重要的选项是选择类别以及是否将为 multiRegional。让我们回顾一下 Google Cloud 可用的存储类别，因为这将是从成本角度来看的一个重要选择，并且也会确保您为数据访问配置文件选择合适的类别。

下表取自 Google Cloud 存储文档。截至撰写本文时为最新，但我们建议您检查 Google Cloud 文档以确认自本书出版日期以来 SLA 是否有任何变化：

| Storage Class | Minimum storage duration | SLA |
|---|---|---|
| Standard Storage | None | >99.99% in multi-regions and dual-regions 99.99% in regions |
| Nearline Storage | 30 days | 99.95% in multi-regions and dual-regions 99.9% in regions |
| Coldline Storage | 90 days | 99.95% in multi-regions and dual-regions 99.9% in regions |
| Archive Storage | 365 days | 99.95% in multi-regions and dual-regions 99.9% in regions |

最小存储时长是在选择类别时需要考虑的关键指标。请注意，所有类别在延迟方面表现相同；您需要考虑的是访问配置文件。在我们的框架中，我们的文件将被相当频繁地访问，因为我们为用户提供播放音频文件的能力。由于其访问配置文件，我们框架的标准类别满足我们的要求并且是最有效的。如果文件在存储达到类别时长之前被删除，Google Cloud 将向您收取提前删除费。

> 例如，假设您在 US multi-region 中存储 1,000 GB 的 Coldline Storage 数据。如果您在第 1 天添加数据，然后在第 60 天将其删除，您将被收取 $14 ($0.007/GB/mo. * 1,000 GB * 2 mo.) 用于第 1 天至 60 天的存储费用，然后收取 $7 ($0.007/GB/mo. * 1,000 GB * 1 mo.) 用于第 61 天至 90 天的 30 天提前删除费用。引自 Google Cloud Storage 文档：https://cloud.google.com/storage/pricing#archival-pricing

让我们继续使用 Node.js 创建我们的第一个存储桶。

- 1. 在您的本地工作站上，git clone 以下仓库：https://gitlab.com/building-cloud-native-applications-google-cloud/chapter-two.git
- 2. 从克隆的仓库，进入 chapter-two 文件夹。
- 3. 在您喜欢的编辑器中打开 createBucket.js 文件。
- 4. 在第 6 行，将 "key.json" 替换为您下载的服务账号密钥。
- 5. 在第 7 行，将 "ruicosta-blog" 替换为您自己的项目 ID。
- 6. 在此文件夹的终端中，运行 npm install
- 7. 在终端中，运行 node createBucket.js
- 8. 执行成功后，它应如下图所示 Figure 2-6

![](img/eac8762c24ec77948aa0c3277f8e6eb3_53_0.png)

Figure 2-6. Code Screenshot for createBucket.js

您可以前往 Google Cloud Storage 控制台，您新创建的存储桶应列出。恭喜！您以编程方式创建了您的第一个存储桶。

## Create a Bucket with Bucket Locks

存储桶锁定允许您创建保留策略以锁定数据，防止其被删除或覆盖。您也可以锁定保留策略。一旦锁定，您将无法解锁；您只能增加保留期。请参阅 Cloud Console 中 Figure 2-7 的警告。这是在警告您，如果您锁定设置为 5 天的保留策略，您将失去缩短或删除它的能力。如果您想删除对象或存储桶本身，您只能在 5 天到期后执行此操作。

## Lock retention policy for this bucket?

Permanent. By locking this bucket's retention policy, you'll lose the ability to shorten or remove it. You'll incur storage costs for the full retention duration of uploaded objects.

RETENTION POLICY DURATION
5 days

A retention policy protects a bucket's objects from deletion or modification regardless of whether it's locked. Only lock the policy if you want to be sure no one (including you) can remove it or shorten its duration.

Confirm you want to lock this policy by entering the bucket name: sample-42892

sample-42892

CANCEL   LOCK POLICY

Figure 2-7. Locking the Cloud Storage Policy

如果您尝试删除保留期内较新的对象，将导致 PERMISSION_DENIED 错误。

让我们以编程方式为 6 个月创建保留策略。此存储桶中的对象将无法被覆盖或删除。查看第 5 行。这是您可以设置存储桶锁定持续时间的位置。

```
// Imports the Google Cloud client library
const {Storage} = require('@google-cloud/storage');
// Creates a client from a Google service account key.
const storage = new Storage({
  keyFilename: "key.json",
  projectId: "ruicosta-blog"
});
const retentionPeriod = 15780000; // 6 months.
const bucketName = 'rui-costa-bucket-name-2020-01';
async function createBucket() {
  await storage
    .createBucket(bucketName)
    .setRetentionPeriod(retentionPeriod);
  console.log(`Bucket ${bucketName} created.`);
}
createBucket().catch(console.error);
```

要在桌面上本地运行代码片段，请按照以下步骤操作：

- 1. 如果您之前已克隆仓库，可以跳过此步骤：在您的本地工作站 git clone 以下仓库：https://gitlab.com/building-cloud-native-applications-google-cloud/chapter-two.git
- 2. 从克隆的仓库，进入 chapter-two 文件夹。
- 3. 在您喜欢的编辑器中打开 createBucketLock.js 文件。
- 4. 在第 5 行将 "key.json" 替换为您下载的服务账号密钥。
- 5. 在第 6 行将 "ruicosta-blog" 替换为您自己的项目 ID。
- 6. 如果您在此文件夹中之前步骤已运行 npm install，可以跳过此步骤：在此文件夹的终端中运行 npm install
- 7. 在终端中运行 node createBucketLock.js
- 8. 执行成功后，它应如下图所示 Figure 2-8

![](img/eac8762c24ec77948aa0c3277f8e6eb3_55_0.png)

Figure 2-8. Code Screenshot for createBucketLock.js

您创建了一个具有 6 个月保留锁定期的新存储桶。如果您尝试向存储桶上传新文件并删除上传的文件，您将收到错误消息。

## List Buckets

让我们现在列出我们以编程方式创建的存储桶。这是一个有用的命令，因为它允许您列出项目中的所有存储桶，这可以让您执行诸如数据丢失预防等操作。我们可以遍历所有存储桶和对象，然后使用 Google Cloud Data Loss Prevention API 扫描它们以查找 PII 信息。第 12 行我们使用 forEach()，它对数组中的每个元素调用提供的回调函数。数组中的每个元素都是我们项目中的存储桶列表。

```
// Imports the Google Cloud client library
const {Storage} = require('@google-cloud/storage');
// Creates a client
const storage = new Storage({
keyFilename: "key.json",
projectId: "ruicosta-blog"
});
```

```
async function listBuckets() {
    // Lists all buckets in the current project
    const [buckets] = await storage.getBuckets();
    console.log('Buckets:');
    buckets.forEach(bucket => {
        console.log(bucket.name);
    });
};
listBuckets().catch(console.error);
```

要在桌面上本地运行代码片段，请按照以下步骤操作：

- 1. 如果您之前已克隆仓库，可以跳过此步骤：在您的本地工作站 git clone 以下仓库：https://gitlab.com/building-cloud-native-applications-google-cloud/chapter-two.git
- 2. 从克隆的仓库，进入 chapter-two 文件夹。
- 3. 在您喜欢的编辑器中打开 listBuckets.js 文件。
- 4. 在第 5 行将 “key.json” 替换为您下载的服务账号密钥。
- 5. 在第 6 行将 “ruicosta-blog” 替换为您自己的项目 ID。
- 6. 如果您在此文件夹中之前步骤已运行 npm install，可以跳过此步骤：在此文件夹的终端中运行 npm install
- 7. 在终端中运行 node listBuckets.js
- 8. 执行成功后，它应如下图所示 Figure 2-9

