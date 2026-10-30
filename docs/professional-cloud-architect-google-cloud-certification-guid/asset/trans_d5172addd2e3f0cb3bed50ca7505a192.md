

![](img/de2c97ab017f36b3750a5bade111edd7_531_0.png)

# 图 16.16 – 新的 VM 实例

我们也可以在 gcloud 命令中指定变量属性。在下面的示例中，我们有一个名为 vm-template1.jinja 的新模板，它期望主机名和区域的变量：

```
resources:
- name: {{ properties["name"] }}
  type: compute.v1.instance
  properties:
    zone: {{ properties["zone"] }}
    machineType: zones/{{ properties ["zone"] }}/machineTypes/g1-small
    disks:
    - deviceName: boot
      type: PERSISTENT
      boot: true
      autoDelete: true
      initializeParams:
        sourceImage: projects/centos-cloud/global/images/centos-stream-8-v20210721
    networkInterfaces:
    - network: global/networks/default
    accessConfigs:
    - name: External NAT
      type: ONE_TO_ONE_NAT
```

再次，让我们使用我们的 gcloud 命令来创建它。但是，我们需要将名称指定为 vm002，区域指定为 europe-west4-a：

```
gcloud deployment-manager deployments create
deployment002 --template vm-template1.jinja --properties
name:vm002,zone:europe-west4-a
```

让我们检查我们的 GCP 控制台并确认我们现在有一个名为 vm002 的 VM 实例，位于 europe-west4-a 区域：

![](img/de2c97ab017f36b3750a5bade111edd7_532_0.png)

图 16.17 – 实例

到目前为止，我们只查看了单个 VM 部署，但我们也可以通过 Deployment Manager 部署多个 VM。

让我们看一个示例，我们想要部署一个由不同区域中的 Web 应用程序和 DB 服务器组成的三层应用程序。我们可以扩展之前的模板以添加所需的信息。请注意，我们现在指定了三个单独的资源。让我们创建一个名为 vm-template2.jinja 的新模板文件：

```
resources:
- name: {{ properties["webname"] }}
  type: compute.v1.instance
```

```
properties:
  zone: {{ properties["webzone"] }}
  machineType: zones/{{ properties ["webzone"] }}/
machineTypes/{{ properties["type"] }}
  disks:
  - deviceName: boot
    type: PERSISTENT
    boot: true
    autoDelete: true
    initializeParams:
      sourceImage: projects/centos-cloud/global/images/
centos-stream-8-v20210701
  networkInterfaces:
  - network: global/networks/default
  accessConfigs:
  - name: External NAT
    type: ONE_TO_ONE_NAT

- name: {{ properties["appname"] }}
  type: compute.v1.instance
  properties:
    zone: {{ properties["appzone"] }}
    machineType: zones/{{ properties ["appzone"] }}/
machineTypes/{{ properties["type"] }}
    disks:
    - deviceName: boot
      type: PERSISTENT
      boot: true
      autoDelete: true
      initializeParams:
        sourceImage: projects/centos-cloud/global/images/
centos-stream-8-v20210701
    networkInterfaces:
    - network: global/networks/default
    accessConfigs:
    - name: External NAT
```

```
type: ONE_TO_ONE_NAT

- name: {{ properties["dbname"] }}
  type: compute.v1.instance
  properties:
    zone: {{ properties["dbzone"] }}
    machineType: zones/{{ properties ["dbzone"] }}/machineTypes/{{ properties["type"] }}
    disks:
      - deviceName: boot
        type: PERSISTENT
        boot: true
        autoDelete: true
        initializeParams:
          sourceImage: projects/centos-cloud/global/images/centos-stream-8-v20210701
    networkInterfaces:
      - network: global/networks/default
    accessConfigs:
      - name: External NAT
        type: ONE_TO_ONE_NAT
```

现在，我们需要更新我们的配置文件。让我们创建一个新文件并将其命名为 multi.yaml。让我们添加以下文本：

```
imports:
  - path: vm-template2.jinja

resources:
  - name: newinstance
    type: vm-template2.jinja
    properties:
      webname: web001
      webzone: europe-west2-a
      appname: app001
      appzone: europe-west4-b
```

```
dbname: db001
dbzone: europe-west6-c
type: g1-small
```

现在，我们可以运行以下命令来触发名为 multideployment 的部署：

```
gcloud deployment-manager deployments create multideployment --config multi.yaml
```

现在，让我们从 Cloud Shell 检查我们的结果：

```
The fingerprint of the deployment is NdvSfAvJTKQnTfuYocnBQ==
Waiting for create [operation-1556477906253-5879bc0b3f6a3-e71eb3bb-b27e918e]...done.
Create operation operation-1556477906253-5879bc0b3f6a3-e71eb3bb-b27e918e completed successfully.
NAME TYPE STATE ERRORS INTENT
app001 compute.v1.instance COMPLETED []
db001 compute.v1.instance COMPLETED []
web001 compute.v1.instance COMPLETED []
```

图 16.18 – 多部署

我们可以通过 GCP 控制台确认我们的部署。让我们查看 Deployment Manager 菜单，在那里我们可以看到我们的部署已成功：

![](img/de2c97ab017f36b3750a5bade111edd7_535_0.png)

图 16.19 – 结果

我们还可以看到每个 VM 实例都已部署到正确的区域：

![](img/de2c97ab017f36b3750a5bade111edd7_536_0.png)

图 16.20 – 实例

这就是我们如何为仅 VM 实例类型配置模板的方式。我们还可以将模板扩展到其他类型的服务，例如网络。作为最后一个示例，让我们看看如何创建模板和配置文件来创建网络和防火墙规则集。在此示例中，我们需要有三个单独的模板：一个用于我们的网络，一个用于我们的子网，一个用于我们的防火墙规则。

让我们先看我们的网络。这很简单。请注意，我们现在使用的是 compute.v1.network 类型，并且我们有单个属性。我们将其设置为 false 以避免自动创建相应的子网。名称是从我们的配置文件传递的变量。让我们将此文件保存为 network.jinja：

```
resources:
- name: {{ env["name"] }}
  type: compute.v1.network
  properties:
    autoCreateSubnetworks: false
```

接下来，我们可以创建一个子网模板。同样，我们将名称作为变量传递，该变量将从我们的配置文件传递。我们还将传递有关 IP 范围和要附加到的网络的信息，我们将使用 network.jinja 模板创建该网络，以及创建子网所在的区域。让我们将其保存为 subnet.jinja：

```
resources:
- name: {{ env["name"] }}
  type: compute.v1.subnetwork
  properties:
    ipCidrRange: {{ properties["iprange"] }}
    network: {{ properties["network"] }}
    region: {{ properties["region"] }}
```

最后，让我们创建一个模板，以便我们可以创建一个防火墙规则。同样，我们将从配置文件传递名称，但这次我们将名称附加一个硬编码值。我们还将设置要附加此规则的网络，并传入防火墙规则的协议。让我们将此模板保存为 firewall.jinja：

```
resources:
- name: {{ env["name"] }}-firewall-rule
  type: compute.v1.firewall
  properties:
    network: {{ properties["network"] }}
    sourceRanges: ["0.0.0.0/0"]
    allowed:
      - IPProtocol: {{ properties["Protocol"] }}
        ports: ["80"]
```

现在，我们需要我们的配置文件。我们将导入所有三个模板，而不是像之前的示例中那样导入单个文件。这并没有什么真正的复杂性——我们只需为每个要导入的模板添加一行额外内容。这里需要注意的关键点之一是确保子网和防火墙规则连接到正确的网络。我们可以按如下方式传递变量：

```
$(ref.custom-net.selfLink)
```

这里，`custom-net` 是我们正在创建的网络的名称。此名称作为变量传递。让我们将此配置文件保存为 `config.yaml`：

```
imports:
- path: network.jinja
- path: subnet.jinja
- path: firewall.jinja

resources:
- name: custom-net
  type: network.jinja

- name: custom-subnet
  type: subnet.jinja
  properties:
    iprange: 10.10.0.0/16
    network: $(ref.custom-net.selfLink)
    region: us-central1

- name: custom
  type: firewall.jinja
  properties:
    network: $(ref.custom-net.selfLink)
    Protocol: TCP
```

最后，让我们将这些文件上传到我们的 Cloud Shell 环境并触发我们已经熟悉的 `gcloud` 命令：

```
gcloud deployment-manager deployments create networking --config config.yaml
```

让我们看看此命令在 Cloud Shell 中的输出是什么样的：

```
The fingerprint of the deployment is 2A6YxCYsxQ_e04Se7N6RpW==
Waiting for create [operation-1556741126674-587d909dcd9bd-cld90539-9a25356a]...done.
Create operation operation-1556741126674-587d909dcd9bd-cld90539-9a25356a completed successfully.
NAME                TYPE                STATE       ERRORS  INTENT
custom-firewall-rule compute.v1.firewall COMPLETED   []
custom-net          compute.v1.network  COMPLETED   []
custom-subnet       compute.v1.subnetwork COMPLETED   []
```

图 16.21 – Google Cloud Shell 输出

