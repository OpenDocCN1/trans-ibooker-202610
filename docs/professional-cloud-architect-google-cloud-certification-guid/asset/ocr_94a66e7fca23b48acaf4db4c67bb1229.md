# Cloud Logging

Cloud Logging is the second most important service. It allows you to store and analyze logs, as well as events coming from GCP and AWS. Based on the logs, alerts can be created. It also provides a robust API, allowing logs to be both managed and injected. This means that any third-party application can leverage Google Cloud's operations suite for logging purposes. The gathered logs are visible in the Legacy Logs Viewer, where they can be filtered and exported for further analysis or archival purposes, or integrated with third-party solutions. There are a number of types of logs, and some of them are not enabled by default. Log-based metrics use log entries and can be leveraged to create dashboard charts and custom alerts. Now, let's take a closer look at how to use logging. We will be looking at the following topics:

+   - Logs Viewer
+   - Basic log filtering
+   - Advanced filtering
+   - Exporting logs
+   - Logging agent
+   - Log-based metrics
+   - Cloud Audit Logs
+   - Activity
+   - Retention

# Legacy Logs Viewer

> **Exam Tip**
> The **Legacy Logs Viewer** is a tool that was used in Stackdriver. You can still access it from Google Cloud's operations suite. The new iteration of this tool has a completely new user experience called the **Logs Explorer**. We will have a look at it in the following section. Keep this section for reference in case you get such a question on the exam.

The Legacy Logs Viewer is a console that allows you to view, filter, and export logs. Keep in mind that logs are associated with a single GCP project: