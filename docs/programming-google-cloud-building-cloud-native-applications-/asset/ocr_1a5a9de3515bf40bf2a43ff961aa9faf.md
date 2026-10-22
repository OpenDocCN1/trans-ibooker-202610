Figure 4-12 visualizes how elements are divided into session windows.

![](bbox=[0.12, 0.17, 0.85, 0.47])

Figure 4-12. Session Windows

# Dataflow Tumbling Window Example

In this example, you will build a Dataflow Tumbling Window pipeline that shows you how to use Dataflow to:

+   - Read messages published to a Pub/Sub topic
+   - Window the messages by timestamp
+   - Write the messages to Cloud Storage

Before you begin, complete the following steps:

+   1. Follow the instructions for installing and initializing the Cloud SDK at https://cloud.google.com/sdk/docs
+   2. From the Google Cloud Console go to APIs & Services > Library