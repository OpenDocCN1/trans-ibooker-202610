![](bbox=[0.16, 0.12, 0.87, 0.4])

Figure 2.X Monitoring telemetry data with Prometheus & Grafana

Using Prometheus and Grafana allows us to not only see the service telemetry, but also build domain specific dashboards to highlight certain application level metrics, for example the amount of Approved vs Rejected proposals over time as shown in figure 2.x.

From the performance point of view, you need to make sure that services are respecting their Service Level Agreements (SLAs) which basically means that they are not taking too long to answer requests. If one of your services is misbehaving and taking more than usual, you want to be aware of that.

For tracing, you will need to modify your services if you are interested in understanding the internal operations and their performance. OpenTelemetry provides drop-in instrumentation libraries in most languages to externalize service metrics and traces.