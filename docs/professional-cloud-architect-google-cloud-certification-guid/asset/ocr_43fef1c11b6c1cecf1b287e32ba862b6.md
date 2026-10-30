> Exam Tip
> Remember that, for the uptime check to work, the firewall rules need to be created. To check the IPs of the uptime servers, go to the uptime check console and download the list of rules. For details on how to do it, check the documentation: https://cloud.google.com/monitoring/uptime-checks/.

# Monitoring agents

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