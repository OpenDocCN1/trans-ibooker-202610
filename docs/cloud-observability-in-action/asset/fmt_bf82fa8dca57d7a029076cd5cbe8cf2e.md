
Note that I’m not suggesting that you should give up on the idea of standardizing on one main frontend, such as Grafana (it is very helpful to have all the data in one place, including the support of correlation). However, be prepared that there will always be a case where certain signals or sources are not covered by that frontend, requiring you to use a dedicated tool, in addition.

## 6.1 Frontends

We will first discuss the two most popular open source frontends: Grafana and Kibana. Next, we will have a look at other open source offerings and commercial offerings used in the observability space.

### 6.1.1 Grafana

Grafana ([https://github.com/grafana/grafana](https://github.com/grafana/grafana)) is nowadays part of Grafana Labs’ LGTM stack ([https://github.com/grafana](https://github.com/grafana))—Loki, Grafana, Tempo, and Mimir—its first and most important asset. Grafana started out as a fork of Kibana ([http://mng.bz/x47W](http://mng.bz/x47W)), when Grafana Labs’ founder Torkel Ödegaard was looking for a UI that focuses on time series data and offers graph-focused dashboards.

Grafana offers several resources, but the three most important are
* `Data sources`—Representing backends you want to use in your dashboards or alerts
* `Dashboards`—A way to organize related visualizations
* `Alerts`—Sending you a notification when a certain thing happens (see chapter 7 for greater detail)

Let us now have a look at Grafana and its top-level resources in action. First, we need to stand up a Grafana instance; we do this with the following Docker Compose file that you launch with the, by now, well-known command `docker-compose` `-f` `docker-compose.yaml` `up` (make sure you first go to `ch06/grafana/`).

Listing 6.1 The Docker Compose configuration for the Grafana instance
```
version: "3.5"
services:
  grafana:
    image: grafana/grafana-oss:9.1.4
    ports:
    - "3000:3000"
    volumes:
    - ./grafana.ini:/etc/grafana/grafana.ini                           ❶
    - ./ds.yaml:/etc/grafana/provisioning/datasources/datasource.yaml  ❷
    depends_on:
    - prometheus
 
  prometheus:
    image: prom/prometheus:latest
    ports:
    - "9090:9090"
    volumes:
    - ./prom-config.yaml:/etc/prometheus/prometheus.yml
```
❶ Provides a Grafana configuration file that simplifies login
❷ Configures Prometheus as the default data source

The `grafana.ini` file we reference in the Docker Compose file allows us to avoid logging in. Note that at time of writing, Grafana was in version 9; hence, the concepts and screenshots are referencing this version. There were some fundamental changes from Grafana 8 to 9 (unified alerting—that is, alerts being global, rather than on a per-dashboard basis), so keep this in mind when comparing your onscreen output with the screenshots you see here.

Let’s begin with Grafana data sources ([https://grafana.com/docs/grafana/latest/datasources/](https://grafana.com/docs/grafana/latest/datasources/), including Amazon CloudWatch, MySQL, Prometheus, and Jaeger, representing backends you can use for visualizations or alerting. If the core data sources are not enough, there are hundreds of plug-ins ([https://grafana.com/grafana/plugins/](https://grafana.com/grafana/plugins/)) available, or you can create your own.

Now go to `http://localhost:3000/`, and then click the gear icon with the Configuration label in the lower part of the left-hand menu. Select the Data Sources menu item. You should see the preconfigured Prometheus data source and, when you click the Add Data Source button, a list of all the available data sources (including potential data source plug-ins you installed).

You can preinstall and preconfigure plug-ins (data sources and visualizations) on Grafana startup. The details are beyond the scope of the book, but as a starting point, here’s an example. Let’s say you want to preinstall the ClickHouse data source plug-in (assuming you have the `grafana-clickhouse-ds` binary available locally) with ClickHouse running self-hosted or from one of the SaaS offerings. Using Docker Compose, you’d add the following lines to the `docker-compose.yaml` YAML manifest in the Grafana section:
```
...
    volumes:
    - ./ds.yaml:/etc/grafana/provisioning/datasources/datasource.yaml
    environment:
      GF_INSTALL_PLUGINS: grafana-clickhouse-ds    ❶
```
❶ The binary of the plug-in, in your local filesystem

In addition, you would define the ClickHouse data source in a file called `ds.yaml` (which is used in the Docker Compose file previous) with the following content:
```
apiVersion: 1
datasources:
  - name: ClickHouse
    type: grafana-clickhouse-datasource   ❶
    jsonData:                             ❷
      defaultDatabase: default
      port: 9000
      server: clickhouse
      username:
      tlsSkipVerify: true
      secureJsonData:
        password:
```
❶ Defines the type of data source plug-in
❷ Below, the data source plug-in-specific configuration

If you now do a `docker-compose` `up`, the ClickHouse data source is automatically added and configured, readily available for you to use in Grafana.

After this short detour on how to provision plug-ins, let’s have a look at a concrete data source. After clicking the Add Data Source button (I picked CloudWatch; see figure 6.1, noting that I trimmed out certain lines, so you will see more here), you should see data source–specific configuration options, including authentication, authorization, and various defaults or limits you want to set.

![](OEBPS/Images/06-01.png)

Figure 6.1 An example Grafana data source, showing configuration options

In this view, you can also define whether the data source should be used as the default data source for new dashboards or alerts (the Default toggle is in the upper-right corner), and if you click the Dashboards tab, you will usually (but not always) find predefined dashboards for the data source. You can use those predefined dashboards as is or as a starting point for your own dashboards. Finally, in the bottom row, you will find several buttons, and with the Save & Test button, Grafana performs a simple (data source–dependent) check to see if it can access the data source with the credentials you provided.

Note The test that Grafana carries out when you press the Save & Test button is typically very basic. Think of it as a necessary but not sufficient condition: if it passes, you can move on (e.g., using Explore to issue a test query against the data source). If it doesn’t pass, you want to check connectivity and whether you have access to the data source, such as by checking IAM roles or API keys.

Grafana dashboards ([https://grafana.com/docs/grafana/latest/dashboards/](https://grafana.com/docs/grafana/latest/dashboards/)) group panels ([https://grafana.com/docs/grafana/latest/panels/](https://grafana.com/docs/grafana/latest/panels/)), supporting a variety of visualizations ([https://grafana.com/docs/grafana/latest/visualizations/](https://grafana.com/docs/grafana/latest/visualizations/)), from time series plots to heatmaps to bar charts that are organized into (collapsible) rows. You can make your dashboards much more reusable and flexible by using variables ([https://grafana.com/docs/grafana/latest/dashboards/variables/](https://grafana.com/docs/grafana/latest/dashboards/variables/)) in a dashboard.
