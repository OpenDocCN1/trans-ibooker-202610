
7. 输入以下文本以创建 Dockerfile 的内容：

```
FROM mcr.microsoft.com/windows/servercore/iis:windowservercore-ltsc2019
RUN powershell -NoProfile -Command Remove-Item -Recurse C:\inetpub\wwwroot\*
WORKDIR /inetpub/wwwroot
COPY content/ .
```

8. 让我们通过运行以下命令来构建 Docker 镜像并将其存储在 Google Container Registry 中（请注意命令末尾的 .）：
`docker build -t gcr.io/my-project-id/iis-site-windows ...`

9. 当输出返回类似 `Successfully tagged gcr.io/my-project-id/iis-site-windows:latest.` 的内容时，您将看到构建已成功完成。

10. 如果您重新运行 Docker 镜像，您现在将看到新创建的镜像可用。

11. 让我们现在使用 `docker run -d -p 80:80 gcr.io/my-project-id/iis-site-windows` 命令运行一个 Windows 容器。

12. 最后，让我们浏览 VM 实例的外部 IP。

![](img/de2c97ab017f36b3750a5bade111edd7_121_0.png)

图 4.47 – Docker 上的 Web 服务器

13. 我们可以从前面的图像中看到，我们已成功运行了一个在 Windows 容器上运行的非常简单的 Web 服务器。

Tip
需要注意的是，在较早版本的 Windows 上构建的容器在具有较新版本 Windows 的 Compute Engine 实例中无法工作。

# 快速了解 Google Cloud VMware Engine

Google Cloud VMware Engine 于 2020 年 5 月宣布推出，这是一项完全托管的服务，允许我们在 Google Cloud 中运行 VMware 平台。Google 将管理基础设施、网络和管理服务，以便您可以高效、安全地使用 VMware 平台。简而言之，这是一种 VMware 即服务，可在大约 30 分钟内部署。它使用 VMware Cloud Foundation 在专用的 Google 裸机服务器上运行，并且可以与 Google 的大数据和 ML 服务集成。

该服务基于部署了 VMware 服务的超融合节点进行销售。ESXi、vSAN 和 NSX-T 位于裸机服务器之上，而 vCenter、NSX Manager 和 HCX 也已安装以实现部署、监控、修补和升级的自动化。Google 负责所有这些工作，而客户将保留对任何虚拟机以及 VMware 管理工具和服务的责任，例如 vRealize 套件。许可也完全集成到服务中；因此，消费者无需担心与每插槽核心相关的任何 VMware 许可问题。

> Tip
> Professional Cloud Architect 考试不会深入 Google Cloud VMware Engine 服务；但是，您应该知道该服务的存在，因为对于希望使用迁移和提升方法快速迁移到 Google Cloud 的公司来说，这可能是一个里程碑。

## 用例

Google Cloud VMware Engine 有许多用例，例如数据中心扩展或迁移，以允许公司在保持现有工作负载兼容性的同时扩展到公共云。灾难恢复也是该服务的另一个巨大优势，因为公司可以利用 Google 的全球基础设施来提高可用性。HCX 用于迁移和灾难恢复。本地数据中心与 Google Cloud 之间专用互连链路的存在允许在两个端点之间轻松迁移虚拟机和服务。Google 正在努力将其作为进入云的最快、最简单的方式进行销售。对于一些不想因迁移到云以实现目标而感到被迫重新培训员工或重构应用程序以保持运营连续性的公司来说，这可能是一件重要的事情。

需要注意的一件重要事情是，基于角色的访问控制（RBAC）和计费都归属于您的 Google 账户下。所有内容都在一个[content]
7. 输入以下文本以创建 Dockerfile 的内容：

```
FROM mcr.microsoft.com/windows/servercore/iis:windowservercore-ltsc2019
RUN powershell -NoProfile -Command Remove-Item -Recurse C:\inetpub\wwwroot\*
WORKDIR /inetpub/wwwroot
COPY content/ .
```

8. 让我们通过运行以下命令来构建 Docker 镜像并将其存储在 Google Container Registry 中（请注意命令末尾的 .）：
`docker build -t gcr.io/my-project-id/iis-site-windows ...`

9. 当输出返回类似 `Successfully tagged gcr.io/my-project-id/iis-site-windows:latest.` 的内容时，您将看到构建已成功完成。

10. 如果您重新运行 Docker 镜像，您现在将看到新创建的镜像可用。

11. 让我们现在使用 `docker run -d -p 80:80 gcr.io/my-project-id/iis-site-windows` 命令运行一个 Windows 容器。

12. 最后，让我们浏览 VM 实例的外部 IP。

![](img/de2c97ab017f36b3750a5bade111edd7_121_0.png)

图 4.47 – Docker 上的 Web 服务器

13. 我们可以从前面的图像中看到，我们已成功运行了一个在 Windows 容器上运行的非常简单的 Web 服务器。

Tip
需要注意的是，在较早版本的 Windows 上构建的容器在具有较新版本 Windows 的 Compute Engine 实例中无法工作。

# 快速了解 Google Cloud VMware Engine

Google Cloud VMware Engine 于 2020 年 5 月宣布推出，这是一项完全托管的服务，允许我们在 Google Cloud 中运行 VMware 平台。Google 将管理基础设施、网络和管理服务，以便您可以高效、安全地使用 VMware 平台。简而言之，这是一种 VMware 即服务，可在大约 30 分钟内部署。它使用 VMware Cloud Foundation 在专用的 Google 裸机服务器上运行，并且可以与 Google 的大数据和 ML 服务集成。

该服务基于部署了 VMware 服务的超融合节点进行销售。ESXi、vSAN 和 NSX-T 位于裸机服务器之上，而 vCenter、NSX Manager 和 HCX 也已安装以实现部署、监控、修补和升级的自动化。Google 负责所有这些工作，而客户将保留对任何虚拟机以及 VMware 管理工具和服务的责任，例如 vRealize 套件。许可也完全集成到服务中；因此，消费者无需担心与每插槽核心相关的任何 VMware 许可问题。

> Tip
> Professional Cloud Architect 考试不会深入 Google Cloud VMware Engine 服务；但是，您应该知道该服务的存在，因为对于希望使用迁移和提升方法快速迁移到 Google Cloud 的公司来说，这可能是一个里程碑。

## 用例

Google Cloud VMware Engine 有许多用例，例如数据中心扩展或迁移，以允许公司在保持现有工作负载兼容性的同时扩展到公共云。灾难恢复也是该服务的另一个巨大优势，因为公司可以利用 Google 的全球基础设施来提高可用性。HCX 用于迁移和灾难恢复。本地数据中心与 Google Cloud 之间专用互连链路的存在允许在两个端点之间轻松迁移虚拟机和服务。Google 正在努力将其作为进入云的最快、最简单的方式进行销售。对于一些不想因迁移到云以实现目标而感到被迫重新培训员工或重构应用程序以保持运营连续性的公司来说，这可能是一件重要的事情。

需要注意的一件重要事情是，基于角色的访问控制（RBAC）和计费都归属于您的 Google 账户下。所有内容都在一个横幅下。这允许 VMware 平台轻松集成到 Google 的原生服务中。

在使用该服务之前有一些先决条件和要求。在高层次上，这里有一些需要考虑的事项。首先，我们需要启用 **VMware Engine API**。我们可以直接从 GCP 主菜单中执行此操作：

![](img/de2c97ab017f36b3750a5bade111edd7_123_0.png)

图 4.48 – VMware Engine

然后我们必须设置私有服务访问，这是在您的虚拟私有云（VPC）与 VMware Engine 中的网络之间的私有连接。您还必须保留不会与任何本地子网、VPC 子网或计划的工作负载子网重叠的地址范围。最后，VMware Engine 有一套特定的 IAM 角色，因此应考虑确保应用最小权限原则。

## 配额和限制

Compute Engine 带有预定义的配额。虚拟机实例是区域配额的一部分，因此对可在给定区域中存在的虚拟机实例数量施加了限制。这些默认配额可以在控制台中通过汉堡菜单更改，然后进入 IAM & Admin | Quotas。从此菜单中，我们可以查看当前配额并请求提高这些限制。我们建议您了解每个服务的限制，因为这可能会影响您的可扩展性。

## IAM 角色

对 GCE 的访问通过 IAM 保护。让我们看一下预定义角色的列表，以及每个角色的简短描述：

- **Compute 管理员角色**：此角色有权访问所有 Compute Engine 资源。
- **Compute 镜像用户角色**：此角色有权列出和读取镜像。
- **Compute 实例管理员（v1）角色**：此角色有权完全管理 Compute Engine 实例、实例组、磁盘、快照和镜像。它还对所有 Compute Engine 网络资源具有读取访问权限。
- **Compute 实例管理员角色**：此角色有权创建、修改和删除虚拟机实例。此外，它有权创建、修改和删除磁盘，并配置受保护虚拟机设置。
- **Compute 负载均衡器管理员角色**：此角色有权创建、修改和删除负载均衡器及其相关资源。
- **Compute 网络管理员角色**：此角色有权创建、修改和删除网络资源，但防火墙规则和 SSL 证书除外。此外，该角色对防火墙规则、SSL 证书和实例（以查看其临时 IP 地址）具有只读权限。使用此角色，您无法创建、启动、停止或删除实例。
- **Compute 网络用户角色**：此角色有权访问共享 VPC 网络。
- **Compute 网络查看者角色**：此角色对所有网络资源具有只读权限。
- **Compute OS 管理员登录角色：** 此角色有权以管理员身份登录到 Compute Engine 实例。
- **Compute 组织防火墙策略管理员角色：** 此角色对 Compute Engine 组织防火墙策略具有完全控制权。
- **Compute 组织防火墙策略用户角色：** 此角色可以查看或使用 Compute Engine 防火墙策略将其与组织或文件夹关联。
- **Compute 组织安全策略管理员角色：** 此角色对 Compute Engine 组织安全策略具有完全控制权。
- **Compute 组织防火墙策略用户角色：** 此角色可以查看或使用 Compute Engine 安全策略将其与组织或文件夹关联。
- **Compute 组织资源管理员角色：** 此角色对组织或文件夹的 Compute Engine 防火墙策略关联具有完全控制权。
- **Compute OS 登录角色：** 此角色有权以管理员身份登录到 Compute Engine 实例。
- **Compute OS 登录外部用户角色：** 此角色仅在组织级别可用。它为外部用户提供访问权限，以便设置与该组织关联的 OS 登录信息。
- **Compute 数据包镜像管理员角色：** 此角色提供指定要镜像的资源的访问权限。
- **Compute 数据包镜像用户角色：** 此角色提供使用被镜像资源的访问权限。
- **Compute 公共 IP 管理员角色：** 此角色对 Compute Engine 的公共 IP 地址管理具有完全控制权。
- **Compute 安全管理员角色：** 此角色有权创建、修改和删除防火墙规则和 SSL 证书。该角色还有权配置受保护虚拟机设置。
- **Compute 存储管理员角色：** 此角色有权创建、修改和删除磁盘、镜像和快照。
- **Compute 查看者角色：** 此角色对 Compute Engine 资源具有只读访问权限。它无权读取存储在其上的数据。
- **Compute 共享 VPC 管理员角色：** 此角色有权管理共享 VPC 主项目。该角色由组织管理员在组织上设置。

