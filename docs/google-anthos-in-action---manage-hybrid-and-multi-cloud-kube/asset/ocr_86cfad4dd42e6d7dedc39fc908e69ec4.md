![](bbox=[0.115, 0.09, 0.885, 0.49])

If you would like to try Pub/Sub example yourself we encourage you to follow the step by step tutorial https://github.com/google/knative-gcp/tree/main/docs/examples/cloudpubsubsource

# 11.3.10 Observability

Knative comes with logging and tracing capabilities. The following open source software is supported:

+   - **Prometheus** and **Grafana** for metrics
+   - **ELK** stack (**Elasticsearch**, **Logstash** and **Kibana**) for logs
+   - **Jaeger** or **Zipkin** for distributed tracing

To learn more about metrics and tracing see the Chapter 5 Anthos Service Mesh: security and observability at scale.

You can also integrate with **Google Cloud Logging** (former Stackdriver Logging) for logs using the *Fluent Bit* agent. The installation procedure for