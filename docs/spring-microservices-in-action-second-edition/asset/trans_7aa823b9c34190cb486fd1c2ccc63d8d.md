

headers that are not part of the HTTP specification and **B3**, which stands for “Big-BrotherBird,” Zipkin’s previous name.

> **NOTE**  If you want to know more about the MDC fields, we highly recommend that you read the SL4J logger documentation at http://www.slf4j.org/manual.html#mdc and the Spring Cloud Sleuth documentation at https://cloud.spring.io/spring-cloud-static/spring-cloud-sleuth/2.1.0.RELEASE/single/spring-cloud-sleuth.html.

You can also configure the log data shown in figure 11.5 by using the Logging-EventCompositeJsonEncoder. Using this composite encoder, we can disable all the providers that were added by default to our configuration, add new patterns to display custom or existent MDC fields, and more. The following listing shows a brief example of the logback-spring.xml configuration that deletes some output fields and creates a new pattern with a custom field and other existent fields.

```
```xml
<encoder class="net.logstash.logback.encoder
                 .LoggingEventCompositeJsonEncoder">
  <providers>
    <mdc>
      <excludeMdcKeyName>X-B3-TraceId</excludeMdcKeyName>
      <excludeMdcKeyName>X-B3-SpanId</excludeMdcKeyName>
      <excludeMdcKeyName>X-B3-ParentSpanId</excludeMdcKeyName>
    </mdc>
    <context/>
    <version/>
    <logLevel/>
    <loggerName/>
    <pattern>
      <pattern>
        <omitEmptyFields>true</omitEmptyFields>
        {
          "application": {
            "version": "1.0"
          },
          "trace": {
            "trace_id": "%mdc{traceId}",
            "span_id": "%mdc{spanId}",
            "parent_span_id": "%mdc{X-B3-ParentSpanId}",
            "exportable": "%mdc{spanExportable}"
          }
        }
      </pattern>
    </pattern>
    <threadName/>
    <message/>
    <logstashMarkers/>
    <arguments/>
    <stackTrace/>
  </providers>
</encoder>
```
```

Although we chose the LogstashEncoder option for this example, you should select the option that best fits your needs. Now that we have our Logback configuration in the licensing service, let’s add the same configuration to our other services: configuration and gateway. We’ll define and run the ELK Stack applications in Docker containers.

## 11.2.3 Defining and running ELK Stack applications in Docker

To set up our ELK Stack containers, we need to follow two simple steps. The first is to create the Logstash configuration file, and the second is to define the ELK Stack applications in our Docker configuration. Before we start creating our configuration, though, it’s important to note that the Logstash pipeline has two required and one optional element. The required elements are the inputs and outputs:

- *The input enables a specific source of events to be read by Logstash.* Logstash supports a variety of input plugins such as GitHub, Http, TCP, Kafka, and others.
- *The output is in charge of sending the event data to a particular destination.* Elastic supports a variety of plugins such as CSV, Elasticsearch, email, file, MongoDB, Redis, stdout, and others.

The optional element in the Logstash configuration is the filter plugins. These filters are in charge of performing intermediary processing on an event such as translations, adding new information, parsing dates, truncating fields, and so forth. Remember, Logstash ingests and transforms the log data received. Figure 11.6 describes the Logstash process.

![](img/3c6dc613426280ca3eafe43dea24391e_347_0.png)

**Figure 11.6** The Logstash configuration process contains two required (input and output) and one optional element (filter).

For this example, we’ll use as the input plugin the Logback TCP appender that we previously configured and as the output plugin the Elasticsearch engine. The following listing shows the `/docker/config/logstash.conf` file.

### Listing 11.3  Adding the Logstash configuration file

```
input {
  tcp {
    port => 5000
    codec => json_lines
  }
}

filter {
  mutate {
    add_tag => [ "manningPublications" ]
  }
}

output {
  elasticsearch {
    hosts => "elasticsearch:9200"
  }
}
```

In listing 11.3, we can see five essential elements. The first is the input section. In this section, we specify the tcp plugin for consuming the log data. Next is the port number 5000; this is the port that we’ll specify for Logstash later in the docker-compose.yml file. (If you take another look at figure 11.4, you’ll notice that we are going to send the applications logs directly to Logstash.)

The third element is optional and corresponds to the filters; for this particular scenario, we added a mutate filter. This filter adds a manningPublications tag to the events. A real-world scenario of a possible tag for your services might be the environment where the application runs. Finally, the fourth and fifth elements specify the output plugin for our Logstash service and send the processed data to the Elasticsearch service running on port 9200. If you are interested in knowing more about all the input, output, and filter plugins Elastic offers, we highly recommend you visit the following pages:

- https://www.elastic.co/guide/en/logstash/current/input-plugins.html
- https://www.elastic.co/guide/en/logstash/current/output-plugins.html
- https://www.elastic.co/guide/en/logstash/current/filter-plugins.html

Now that we have the Logstash configuration, let’s add the three ELK Docker entries to the docker-compose.yml file. Remember, we are using this file to fire up all of the Docker containers used for the code examples in this and the previous chapters. The following listing shows the docker/docker-compose.yml file with the new entries.

### Listing 11.4  Configuring ELK Stack/Docker Compose

```
#Part of the docker-compose.yml removed for conciseness
#Some additional configuration also removed for conciseness

elasticsearch:
```

The first allows us to play with some sample data, and the second lets us explore the generated data from our services. Figure 11.7 shows Kibana’s Welcome page.

![](img/3c6dc613426280ca3eafe43dea24391e_350_0.png)

**Figure 11.7** The Kibana Welcome page with two options: trying out the application with sample data or exploring it on your own.

To explore our data, let’s click the Explore on My Own link. Once clicked, we will see an Add Data page like the one shown in figure 11.8. On this page, we need to click the Discover icon on the left side of the page.

![](img/3c6dc613426280ca3eafe43dea24391e_350_1.png)

**Figure 11.8** The Kibana setup page. Here we will see a set of options we can use to configure our Kibana application. Notice the icon menu on the left.

In order to continue, we must create an index pattern. Kibana uses a set of index patterns to retrieve the data from an Elasticsearch engine. The index pattern is in charge of telling Kibana which Elasticsearch indexes we want to explore. For example, in our case, we will create an index pattern indicating that we want to retrieve all of

![](img/3c6dc613426280ca3eafe43dea24391e_351_0.png)

# Figure 11.9 Configuring the index pattern for the Elasticsearch engine

the Logstash information from Elasticsearch. To create our index pattern, click the Index Patterns link under the Kibana section at the left of the page. Figure 11.9 shows step 1 of this process.

On the Create Index Pattern page in figure 11.9, we can see that Logstash has already created an index as a first step. However, this index is not ready to use yet. To finish setting up the index, we must specify an index pattern for that index. To create it, we need to write the index pattern, logstash-*, and click the Next Step button.

For step 2, we’ll specify a time filter. To do this, we need to select the @timestamp option under the Time Filter Field Name drop-down list and then click the Create Index Pattern button. Figure 11.10 shows this process.

![](img/3c6dc613426280ca3eafe43dea24391e_351_1.png)

# Figure 11.10 Configuring the timestamp filter for our index pattern. This pattern allows us to filter events within a time range.

