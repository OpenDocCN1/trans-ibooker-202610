
运行收集脚本：

```powershell
powershell -ExecutionPolicy Bypass -File .\mfit-windows-collect.ps1
```

该脚本将在当前目录下生成一个名为 `m4a-collect-<MACHINE_NAME>-<TIMESTAMP>.tar/zip` 的 tar 或 zip 文件（取决于操作系统版本）。

## 导入收集的数据文件

在被评估的虚拟机（VM）上运行收集脚本后，通过任意方式将其下载到安装了 *mfit* 的工作站。

然后，使用以下命令将其导入 *mfit* 的本地数据库：

```
./mfit discover import m4a-collect-<MACHINE_NAME>-<TIMESTAMP>.tar
```

## 自动 Guest 级数据收集

*mfit* 内嵌了 guest 收集脚本，可以在以下场景中自动运行这些脚本并获取结果。

### VMware Tools

如果被评估的 VM 运行在 vSphere 上且已安装 VMware Tools，*mfit* 可以使用 vSphere API 自动执行收集脚本（与 VM 操作系统类型相匹配的脚本）并获取结果。

要通过 VMware Tools 运行 guest 级收集，请运行以下命令：

```
./mfit discover vsphere guest -u <vcenter username> --url <https://vcenter.example.com>
```

系统会提示您输入 vCenter 密码和 VM/OS 密码。

### SSH

如果运行 *mfit* 的 Linux 机器可以 SSH 访问被评估的 VM，*mfit* 可以利用这一点自动执行收集脚本并获取结果。

- 要使用当前本地用户的 SSH 密钥（位于 ~/.ssh）通过 SSH 运行 guest 级收集，请运行以下命令：

  ```
  ./mfit discover ssh <vm-ip-or-hostname>
  ```

- 要通过 SSH 并使用其他身份验证选项运行 guest 级收集，请运行以下命令：

  ```
  ./mfit discover ssh -i </path/to/ssh_private_key> -u <remote-user>
  ```

有关通过 SSH 运行 guest 级收集的其他选项，请查阅官方文档或运行 `./mfit discover ssh -help`。

## 评估

要检查发现的 VM 和收集的数据，请运行以下命令：

```
./mfit discover ls
```

要对此数据执行评估，请运行以下命令：

```
./mfit assess
```

这将创建评估结果并将其存储在 mfit 的本地数据库中，以供生成报告时使用。

## 报告生成

完成评估后，我们就可以生成报告了。

要生成独立的 HTML 报告，请运行以下命令：

```
./mfit report --format html > REPORT_NAME.html
```

要生成 JSON 报告，请运行以下命令：

```
./mfit report --format json > REPORT_NAME.json
```

然后可以使用 Google Cloud 控制台显示该报告：

https://console.cloud.google.com/anthos/migrate/report

在本节中，我们讨论了用于确定工作负载是否适合迁移到容器的 fit 评估工具。完成评估并选择适合迁移的工作负载后，我们就可以开始迁移过程本身。这是下一节的主题。

## 18.4.2 基本迁移示例

在这个基本示例中，我们将使用命令行界面（CLI）在 GCE 上设置一台 Compute Engine 虚拟机，然后使用 M4A 将 VM 迁移到 GKE 集群。请注意，我们需要创建另一个 Kubernetes“处理集群”，用于驱动迁移过程本身。处理集群将负责从 VM 中提取应用程序，并将所有工件生成为容器。我们开始吧。

首先，我们创建一个源 VM。在这个基本示例中，VM 将托管 Apache Web 服务器。可以使用以下命令创建 VM：

```
gcloud compute instances create http-server-demo --machine-type=n1-standard-1
```

然后，让我们使用以下命令确保可以从互联网访问 VM：

```
gcloud compute firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0
```

现在，我们可以使用 GUI（见图 18.4）登录刚刚配置的 VM，并使用以下命令安装 Apache：

```
sudo apt-get update && sudo apt-get install apache2 -y
```

**图 18.4：连接迁移集群**

现在，我们有了一个运行 Apache Web 服务器的源虚拟机。下一步是创建 Kubernetes 处理集群，我们可以使用以下命令来完成：

```
gcloud container clusters create migration-processing --machine-type=n1-standard-1
```

接下来，让我们确保为处理集群授予正确的处理权限。我们需要为 M4A 创建一个特定的服务账号，添加具有 `storage.admin` 权限的策略绑定，创建一个用于访问处理集群的 JSON 密钥，并获取处理集群的凭据。我们可以通过以下四条命令来完成此操作 - 请注意，`named-tome-295414` 是我的项目名称，您应将其替换为您自己的项目名称：

```
gcloud iam service-accounts create m4a-install
gcloud projects add-iam-policy-binding named-tome-295414 --member=serviceAccount:m4a-install@named-tome-295414.iam.gserviceaccount.com --role=roles/storage.admin
gcloud iam service-accounts keys create m4a-install.json --iam-account=m4a-install@named-tome-295414.iam.gserviceaccount.com
gcloud container clusters get-credentials migration-processing
```

获得凭据后，我们可以登录处理集群并安装 M4A。我们使用以下命令来执行此操作 - 请注意，`m4a-install.json` 是我们刚刚创建的 JSON 密钥：

```
migctl setup install --json-key=m4a-install.json
```

我们可以使用 migctl 通过以下命令检查部署是否成功：

```
migctl doctor
[✓] Deployment
```

之后，我们可以设置迁移源，同时需要创建一个特定的服务账号 `m4a-ce-src`、添加迁移过程中所需的 `compute.viewer` 和 `compute.storageAdmin` 策略绑定，并创建 JSON 密钥 `m4a-ce-src.json`。

可以使用以下命令完成此操作：

```
gcloud iam service-accounts create m4a-ce-src
gcloud projects add-iam-policy-binding named-tome-295414 --member=serviceAccount:m4a-ce-src@named-tome-295414.iam.gserviceaccount.com --role=roles/compute.viewer
gcloud projects add-iam-policy-binding named-tome-295414 --member=serviceAccount:m4a-ce-src@named-tome-295414.iam.gserviceaccount.com --role=roles/compute.storageAdmin
gcloud iam service-accounts keys create m4a-ce-src.json --iam-account=m4a-ce-src@named-tome-295414.iam.gserviceaccount.com
```

为源创建凭据后，我们可以继续使用以下命令设置源 - 请注意，`ce` 代表 Google Compute Engine（GCE）：

```
migctl source create ce http-source --project named-tome-295414 --service-account-key-file m4a-ce-src.json
```

创建迁移源后，我们现在可以使用以下命令创建迁移计划，将 VM 容器化：

```
migctl migration create my-migration --source http-source --vm-id=http-server-demo
```

如果您想查看迁移计划（例如修改它），可以使用以下命令：

```
migctl migration get my-migration
```

可以使用以下命令启动实际迁移：

```
migctl migration generate-artifacts my-migration
```

结果，您应该会看到类似如下的输出：

```
running validation checks on the Migration...
migration.anthos-migrate.cloud.google.com/my-migration created
```

迁移启动后，可以使用以下命令检查进度 - 请注意，`-v` 标志会输出详细的状态信息，这在出现问题时非常有用：

```
migctl migration status my-migration
```

迁移结束后，您将看到类似于以下输出的内容：

| 名称 | 当前操作 | 进度 | 步骤 |
| --- | --- | --- | --- |
| my-migration | GenerateMigrationPlan | [2/2] | CreatePvc |

下一步是使用以下命令获取生成的工件：

```
migctl migration get-artifacts my-migration
```

生成完成后，您应该会看到类似如下的内容：

```
Downloaded artifacts for Migration my-migration. The artifacts are available in the current directory.
```

由于我们希望访问已迁移容器内的 Web 服务器，让我们编辑 deployment_spec.yaml 并添加一个特定的节来完成该任务：

```
apiVersion: v1
kind: Service
metadata:
  name: hello-service
spec:
  selector:
    app: http-server-demo
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: LoadBalancer
```

现在，我们可以将工件部署到 Kubernetes 集群上：

```
kubectl apply -f deployment_spec.yaml
```

```
deployment.apps/app-source-vm created
service/app-source-vm created
service/my-service created
```

使用以下命令检查一切是否正常可能会很有用：

```
kubectl get service
```

