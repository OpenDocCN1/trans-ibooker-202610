
> Exam Tip
> Remember that, for the uptime check to work, the firewall rules need to be created. To check the IPs of the uptime servers, go to the uptime check console and download the list of rules. For details on how to do it, check the documentation: https://cloud.google.com/monitoring/uptime-checks/.

## Monitoring agents

To get more out of Cloud Monitoring, a Monitoring agent can be installed on the instance to collect additional metrics. By default, the Monitoring agent collects disk, CPU, network, and process metrics; however, additional metrics can also be collected.

The Monitoring agent is a collectd-based agent that can be installed both on GCP and AWS instances. The agent can be also configured to monitor many applications, including the Apache web server, Tomcat, Kafka, Memcached, and Redis.

The installation of the agent on Linux is very straightforward and requires the following two commands to be executed:

```
curl -sSO https://dl.google.com/cloudagents/add-monitoring-agent-repo.sh
```

```
sudo bash add-monitoring-agent-repo.sh --also-install
```

To install the agent on a Windows machine, run the following command in PowerShell:

```
(New-Object Net.WebClient).DownloadFile("https://repo.stackdriver.com/windows/StackdriverMonitoring-GCM-46.exe", "${env:UserProfile}\StackdriverMonitoring-GCM-46.exe") & "${env:UserProfile}\StackdriverMonitoring-GCM-46.exe"
```

More instructions can be found at https://cloud.google.com/monitoring/agent/install-agent#agent-install-windows.

> Exam Tip
> To monitor the instance memory, you need to use the Monitoring agent!

## Cloud Logging

Cloud Logging is the second most important service. It allows you to store and analyze logs, as well as events coming from GCP and AWS. Based on the logs, alerts can be created. It also provides a robust API, allowing logs to be both managed and injected. This means that any third-party application can leverage Google Cloud's operations suite for logging purposes. The gathered logs are visible in the Legacy Logs Viewer, where they can be filtered and exported for further analysis or archival purposes, or integrated with third-party solutions. There are a number of types of logs, and some of them are not enabled by default. Log-based metrics use log entries and can be leveraged to create dashboard charts and custom alerts. Now, let's take a closer look at how to use logging. We will be looking at the following topics:

- Logs Viewer
- Basic log filtering
- Advanced filtering
- Exporting logs
- Logging agent
- Log-based metrics
- Cloud Audit Logs
- Activity
- Retention

## Legacy Logs Viewer

> **Exam Tip**
> The **Legacy Logs Viewer** is a tool that was used in Stackdriver. You can still access it from Google Cloud's operations suite. The new iteration of this tool has a completely new user experience called the **Logs Explorer**. We will have a look at it in the following section. Keep this section for reference in case you get such a question on the exam.

The Legacy Logs Viewer is a console that allows you to view, filter, and export logs. Keep in mind that logs are associated with a single GCP project:

Figure 17.15 – Legacy Logs Viewer

To be able to view logs from other projects, you need to switch the view. An API can be used to get logs from multiple projects. The logs can be filtered, using either basic or advanced filtering, to pinpoint the exact event.

## Logs Explorer

The Logs Explorer is a new tool from the Google Cloud operations suite that allows you to inspect your GCP logs. It pops up as the default pane when you open the Cloud Logging service in the GCP console:

![](img/de2c97ab017f36b3750a5bade111edd7_565_0.png)

Figure 17.16 – Accessing Cloud Logging

With the Logs Explorer, you can choose to view the logs in two different scopes:

- Scope by project – allows you to search and view logs from a single project
- Scope by storage – allows you to view logs from a bucket that was used as a sink

The second option is very convenient as it allows you to get the same experience from browsing the sink logs as you would get for the logs that still reside in the project:

![](img/de2c97ab017f36b3750a5bade111edd7_566_0.png)

Figure 17.17 – Refine scope

To find the logs you are interested in, you can define a query by simply choosing a filter and then a value from the list of the objects that the filter selected:

Logs explorer
OPTIONS
REFINE SCOPE
Project
New features are available in the Logs explorer.
Query
Recent (2)
Saved (0)
Suggested (3)
Resource +
Log name
Severity
Select resource
Search resource filters
Kubernetes Node
Kubernetes Pod
Metric type
OS Patch Job
Service Account
Stackdriver Notification Channel
Subnetwork
VM Instance
Search instance_id
All instance_id
INSTANCE_ID
1080228512014207575
1672071981735813276
2701302113299800956
4721548050940506507
5638987979155252508
8309969565773668901
String preview
resource.type="gce_instance"
Reset
Cancel
Add

Figure 17.18 – Resource filters

The second filter allows you to choose the type of logs to be displayed, as shown in the following screenshot:

![](img/de2c97ab017f36b3750a5bade111edd7_568_0.png)

Figure 17.19 – Log name

The third filter allows the level of severity to be chosen:

![](img/de2c97ab017f36b3750a5bade111edd7_568_1.png)

Figure 17.20 – Severity

From the top-right link, you can go to the menu where you can choose the time range:

![](img/de2c97ab017f36b3750a5bade111edd7_569_0.png)

Figure 17.21 – Time range selection

You can also choose from predefined time spans or a custom one. Finally, when we run the query, we get the logs that show when the machine was started:

![](img/de2c97ab017f36b3750a5bade111edd7_569_1.png)

Figure 17.22 – Filtered logs

The severity in this case is at the INFO level.

The preceding example shows logs with a resource type of the GCE instance and a severity of INFO and higher.

## Exporting logs

Log entries that are received by logging can be exported (copied) to Cloud Storage buckets, BigQuery data sets, and Cloud Pub/Sub topics. You export logs by configuring log sinks, which then continue to export log entries as they arrive in logging. A sink includes a destination and a filter that selects the log entries to be exported. Remember that only the logs that were created after the sink configuration will be exported:

1. To set up an export, go to Logs Router in the Cloud Logging window and click on CREATE SINK:

![](img/de2c97ab017f36b3750a5bade111edd7_570_0.png)

Figure 17.23 – Logs Router

2. In Sink details, fill in the name of the sink and description:

![](img/de2c97ab017f36b3750a5bade111edd7_570_1.png)

Figure 17.24 – Sink details

3. In the **Sink destination** section, you can choose the service that you want to sink to. These include the following:

- Cloud Logging bucket
- BigQuery data set
- Cloud Storage bucket
- Cloud Pub/Sub topic
- Splunk
- Other project:

# 2 Sink destination

Select the service type and destination for logs routing sink

![](img/de2c97ab017f36b3750a5bade111edd7_571_0.png)

Figure 17.25 – Sink destination

4. In choosing the logs to include in the sink section, you can add a filter to define which logs you want to sink. If you leave this blank, all logs will be included. We can use the simple filter we already created. This will give us logs for GCE VM instances:

# 3 Choose logs to include in sink

Create an inclusion filter to determine which logs are included in logs routing sink

![](img/de2c97ab017f36b3750a5bade111edd7_571_1.png)

Figure 17.26 – Choose logs to include in sink

5. If you want to check the filters functions correctly, click on PREVIEW LOGS. This will bring you to the Logs Explorer:

![](img/de2c97ab017f36b3750a5bade111edd7_572_0.png)

Figure 17.27 – Logs preview

6. As we can see, all GCE logs are now visible, as the filter did not specify any additional parameters, such as log types.

7. Finally, you may wish to exclude some logs from the sink. In this section, you choose the exclusion filter name. By setting the exclusion rate, you can decide what percentage of the log should be filtered out. A rate of 100% means that all logs that match the filter will be excluded:

![](img/de2c97ab017f36b3750a5bade111edd7_572_1.png)

Figure 17.28 – Choose logs to filter out of sink
