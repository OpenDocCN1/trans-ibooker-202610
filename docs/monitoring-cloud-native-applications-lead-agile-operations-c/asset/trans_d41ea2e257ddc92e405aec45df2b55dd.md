
Prometheus还有一个基于React的实验性用户界面，并且有一个简单的链接，可以轻松地从我们之前看到的经典用户界面切换到基于React的用户界面，反之亦然。参见图4-34。

![](img/6646c41cea18bb4f7d3c2c6513a4d992_135_0.png)

图4-34. 基于React的用户界面

## 第4章 Prometheus

这个用户界面与经典用户界面非常相似，除了外观和感觉上的差异以及一些附加功能，比如“使用本地时间”。Prometheus在存储和检索时序数据时使用UTC³，因此表达式浏览器中的所有图表也以UTC显示时间。对于基于React的用户界面，您可以将UTC时间转换为本地时间，这在分析图表时更有意义。除了折线图之外，还有一个选项可以将图表堆叠在一起，示例见图4-35。

![](img/6646c41cea18bb4f7d3c2c6513a4d992_136_0.png)

图4-35. 堆叠图表

³UTC：协调世界时

## 静默

AlertManager可以配置为在特定时间段内静默警报。可以使用AlertManager的用户界面配置静默；一旦激活，它将根据警报的标签检查传入的警报。如果标签匹配，则在静默生效期间不会发送通知。

磁盘以列式格式组织，包含连续的时间块，这些时间块按标签集、测量或字段进行组织。由于每个字段按顺序排列，因此在单个字段上计算聚合值更快。此外，可使用的标签或字段数量没有限制。

时序数据库常见的挑战是，对多个字段和标签的支持有限，并且只支持float64值。InfluxDB数据模型没有这样的挑战，因为它正是为解决这些问题而构建的。它支持多种数据格式，同时字段和标签的数量也没有任何限制。它允许开发者通过索引标签而不索引字段来选择模式。

Telegraf是InfluxDB的开源代理，支持200多个插件。它能够满足大多数常见应用程序、系统和网络设备的需求。它同时支持拉取和推送模式，因此可以通过抓取或监听方式工作，使其成为一个通用代理，不仅支持InfluxDB，还可以与其他监控后端配合使用。

## 安装

我们可以通过运行 InfluxDB 配置的 yaml¹ 文件，将 InfluxDB OSS 2.0 部署到 Kubernetes 集群上。在继续部署 InfluxDB 之前，我们先仔细看一下图5-1中的 yaml：

¹YAML（YAML Ain’t Markup Language，YAML不是标记语言）是云原生环境中最常用的声明式语言。更多详情请访问 www.yaml.org。

```
1 ---
2 apiVersion: v1
3 kind: Namespace
4 metadata:
5     name: influxdb
6 ---
7 apiVersion: apps/v1
8 kind: StatefulSet
9 metadata:
10    labels:
11        app: influxdb
12    name: influxdb
13    namespace: influxdb
14 spec:
15    replicas: 1
16    selector:
17        matchLabels:
18            app: influxdb
19    serviceName: influxdb
20    template:
21        metadata:
22            labels:
23                app: influxdb
24        spec:
25            containers:
26                - image: quay.io/influxdb/influxdb:v2.0.1
27                name: influxdb
28                ports:
29                    - containerPort: 8086
30                    name: influxdb
31                volumeMounts:
32                    - mountPath: /root/.influxdbv2
33                    name: data
34    volumeClaimTemplates:
35    - metadata:
36            name: data
37            namespace: influxdb
38        spec:
39            accessModes:
40            - ReadWriteOnce
41            resources:
```

图5-1. InfluxDB YAML配置

```
42              requests:
43                  storage: 10G
44 ---
45 apiVersion: v1
46 kind: Service
47 metadata:
48     name: influxdb
49     namespace: influxdb
50 spec:
51     ports:
52       - name: influxdb
53         port: 8086
54         targetPort: 8086
55     selector:
56         app: influxdb
57     type: ClusterIP
```

# 图5-1.（续）

此 yaml 文件包含用于创建独立的命名空间 influxdb、独立服务 influxdb 以及有状态副本集 influxdb 的配置。因此，让我们继续使用 kubectl 命令部署此 YAML 配置。部署完成后，您可以检查 influxdb 命名空间中是否有正在运行的 Pod。如果部署成功，您应该会看到 influxdb-0 Pod 处于运行状态，如图5-2所示。

```
mchakraborti-a01:~ mchakraborty$ kubectl apply -f https://raw.githubusercontent.com/influxdata/docs-v2/master/static/downloads/influxdb-k8-minikube.yaml
namespace/influxdb created
statefulset.apps/influxdb created
service/influxdb created
mchakraborti-a01:~ mchakraborty$ kubectl get pods -n influxdb
NAME           READY   STATUS    RESTARTS   AGE
influxdb-0     1/1     Running   0          31s
```

# 图5-2. 在Kubernetes集群上运行InfluxDB

我们将运行 kubectl describe 命令来获取 influxdb 服务的端点 IP 和关联端口，如图5-3所示。

```
mchakrabort-a01:~ mchakraborty$ kubectl describe service -n influxdb influxdb
Name:                     influxdb
Namespace:                influxdb
Labels:                   <none>
Annotations:              Selector: app=influxdb
Type:                     ClusterIP
IP:                       10.98.194.86
Port:                     influxdb 9999/TCP
TargetPort:               9999/TCP
Endpoints:                172.17.0.4:9999
Session Affinity:         None
Events:                   <none>
```

# 图5-3. 描述InfluxDB Pod

获得这些详细信息后，我们可以运行 kubectl port-forward 命令，将集群内部的9999端口转发到本地主机，如图5-4所示。

```
mchakrabort-a01:~ mchakraborty$ kubectl port-forward -n influxdb service/influxdb 9999:9999
Forwarding from 127.0.0.1:9999 -> 9999
Forwarding from [::1]:9999 -> 9999
Handling connection for 9999
Handling connection for 9999
Handling connection for 9999
Handling connection for 9999
Handling connection for 9999
```

# 图5-4. 在本地主机上进行端口转发

如果安装成功，应该可以通过 http://localhost:9999/ 访问 InfluxDB 用户界面（见图5-5）。

![](img/6646c41cea18bb4f7d3c2c6513a4d992_148_0.png)

# 图5-8. InfluxDB 登录页面

在此阶段，最好注销并使用设置屏幕中的用户名和密码重新登录，如图5-9所示。

# 图5-10. 通过用户界面创建组织

在配置了多个组织之后，可以使用下拉菜单中的“切换组织”选项，并选择要设置为当前组织的选项，轻松地在组织之间切换。在图5-11中，当前组织设置为 monitoringcloudnativeapps，如果需要，可以通过选择 monitoringcloudnativeapps2 切换到另一个组织。

# 图5-11. 通过用户界面切换组织

使用 Influx CLI 和 *influx org create* 命令，通过 CLI 可以轻松地完成同样的操作。该命令的语法如下：

```
$ influx org create --host <http://hostname:port> --token <INFLUX_TOKEN> --name <org name>
```

此命令的一个实现示例如图5-12所示。注意，在 <INFLUX_TOKEN> 部分传递 **管理令牌**，该令牌可从“加载数据”部分下的可用令牌中获取（稍后在“数据收集”部分说明）。成功创建后，命令的输出将包含新的组织名称及其关联的 ID。

![](img/6646c41cea18bb4f7d3c2c6513a4d992_151_0.png)

# 图5-12. 使用CLI创建组织

要确认新组织是否已创建，请检查“切换组织”部分下的 UI。您的新组织名称应显示在列表中，如图5-13所示。

![](img/6646c41cea18bb4f7d3c2c6513a4d992_151_1.png)

# 图5-13. 新组织在UI中可用

您还可以使用 CLI 通过以下命令查看组织列表：

```
$ influx org list --host <http://hostname:port> --token <INFLUX_TOKEN>
```

