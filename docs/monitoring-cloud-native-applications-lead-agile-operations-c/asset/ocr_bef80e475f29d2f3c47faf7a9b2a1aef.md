From the list of data sources, let’s try and integrate with InfluxDB. On the settings page, depending on the InfluxDB edition you are integrating with, you will have an option of choosing either InfluxQL or Flux as your query language of choice. We will go ahead with Flux, as we have InfluxDB 2.0 running. See Figure 6-14.

![](bbox=[0.15, 0.214, 0.885, 0.395])

Figure 6-14. Selecting a query language

Then we will pass on the connection information so that InfluxDB can be reached from Grafana. We will also need to mention the organization, default bucket, connection token, and minimum time interval for Grafana to query InfluxDB. The default slider will mark InfluxDB as the default data source. See Figure 6-15.