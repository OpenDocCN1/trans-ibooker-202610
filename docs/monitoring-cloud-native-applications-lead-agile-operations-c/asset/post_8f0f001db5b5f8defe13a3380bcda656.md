
Prometheus also has an experimental UI based on React, and there is an easy link to switch from the Classic UI which we saw earlier to the React-based UI and vice versa. See Figure 4-34.

![](img/6646c41cea18bb4f7d3c2c6513a4d992_135_0.png)

Figure 4-34. React-based UI

## CHAPTER 4 PROMETHEUS

This UI is very similar to the Classic UI, apart from the difference in the look and feel and some additional features like Use Local Time. Prometheus uses UTC³ while storing and retrieving the time-series data, and therefore all the graphs in Expression Browser also display time in UTC. In the case of React-based UI, you can convert the UTC time into local time, which makes more sense while analyzing the graph. Apart from the line graph, there is another option to stack graphs on top of each other, an example of which is shown in Figure 4-35.

![](img/6646c41cea18bb4f7d3c2c6513a4d992_136_0.png)

Figure 4-35. Stacked graphs

³UTC: Coordinated Universal Time

## Silences

AlertManager can be configured to mute alerts for a specific duration. Silences can be configured using the user interface of the AlertManager; once activated, it will check incoming alerts on the basis of their labels. In case the labels match, notifications will not be sent until the silence is active.

disk is organized in a columnar format with contiguous time blocks which are organized by tagset, measurement, or field. Since each field is arranged sequentially, it is faster to calculate aggregates on a single field. Also, there is no limitation on the number of tags or fields which can be used.

The common challenge for time-series databases is having limited support for multiple fields and tags and supporting only float64 values. The InfluxDB data model doesn’t have such challenges, as it is specifically built to solve these issues. It supports many data formats, and fields and tags also don’t have any limitation around the numbers that can be used. It lets the developer choose the schema by indexing tags and not indexing the fields.

Telegraf, the open source agent of InfluxDB, supports more than 200 plug-ins. It caters to most of the applications, systems, and network devices commonly available. It supports both pull and push mode and therefore can work in scraping or listening methods, making it a universal agent that not just supports InfluxDB but can also work with other monitoring back ends.

## Installation

We can deploy InfluxDB OSS 2.0 on a Kubernetes cluster by running the InfluxDB configuration yaml¹ file. Before we go ahead and deploy InfluxDB, let’s have a closer look at the yaml first in Figure 5-1:

¹YAML (YAML Ain’t Markup Language) is the declarative language used most commonly in cloud native environments. For more details visit www.yaml.org.

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

Figure 5-1. InfluxDB YAML configuration

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

# Figure 5-1. (continued)

This yaml file has the configuration to create a separate namespace called influxdb, a separate service called influxdb, and a stateful set also called influxdb on execution. So, let’s go ahead and deploy this YAML configuration using the kubectl command. Once deployed, you can check to see whether there are running pods in the namespace influxdb. If your deployment is successful, you should see the influxdb-0 pod in a running state, as shown in Figure 5-2.

```
mchakraborti-a01:~ mchakraborty$ kubectl apply -f https://raw.githubusercontent.com/influxdata/docs-v2/master/static/downloads/influxdb-k8-minikube.yaml
namespace/influxdb created
statefulset.apps/influxdb created
service/influxdb created
mchakraborti-a01:~ mchakraborty$ kubectl get pods -n influxdb
NAME           READY   STATUS    RESTARTS   AGE
influxdb-0     1/1     Running   0          31s
```

# Figure 5-2. Running InfluxDB on Kubernetes cluster

We will run the kubectl describe command to get the endpoint IP and the associated port for the influxdb service, as shown in Figure 5-3.

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

# Figure 5-3. Describing the InfluxDB pod

Once we have these details, we can run the kubectl port-forward command to forward port 9999 from inside the cluster to localhost, as seen in Figure 5-4.

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

# Figure 5-4. Port forwarding on localhost

If the installation happened successfully, InfluxDB UI should be accessible at http://localhost:9999/ (see Figure 5-5).

![](img/6646c41cea18bb4f7d3c2c6513a4d992_148_0.png)

# Figure 5-8. Landing page for InfluxDB

At this stage, it is prudent to log out and use the username and password from the Setup screen to log in again, as shown in Figure 5-9.

# Figure 5-10. Create organization through UI

After configuring more than one organization, it is easy to switch from one organization to another using the Switch Organizations option from the drop-down and selecting the option which you want to set as the current organization. In Figure 5-11, the current organization is set as monitoringcloudnativeapps, which can be switched to another organization by selecting monitoringcloudnativeapps2 if so desired.

# Figure 5-11. Switch organizations through UI

It is easy to do the same thing via CLI using Influx CLI and the *influx org create* command. The syntax of the command is

```
$ influx org create --host <http://hostname:port> --token <INFLUX_TOKEN> --name <org name>
```

An implementation of this command is shown in Figure 5-12. Note to pass the **Admin token** in the <INFLUX_TOKEN> section from the tokens available under the Load Data section (explained later in the “Data Collection” section). Once successfully created, the output of the command will have the new org name and the associated ID with it.

![](img/6646c41cea18bb4f7d3c2c6513a4d992_151_0.png)

# Figure 5-12. Create organization using CLI

To confirm whether the new organization is created, check the UI under the Switch Organizations section. Your new org name should show up in the list, as shown in Figure 5-13.

![](img/6646c41cea18bb4f7d3c2c6513a4d992_151_1.png)

# Figure 5-13. New organization available in UI

You can also use the CLI in order to view the list of organizations by using the following command:

```
$ influx org list --host <http://hostname:port> --token <INFLUX_TOKEN>
```
