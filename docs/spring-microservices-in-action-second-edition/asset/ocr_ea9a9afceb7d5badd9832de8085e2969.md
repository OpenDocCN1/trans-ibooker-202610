# ADDING THE LOGSTASH ENCODER

To begin, we need to add the logstash-logback-encoder dependency to the pom.xml file of our licensing, organization, and gateway services. Remember, the pom.xml file can be found in the root directory of the source code. Here’s the code to add the dependency:

```
<dependency>
    <groupId>net.logstash.logback</groupId>
    <artifactId>logstash-logback-encoder</artifactId>
    <version>6.3</version>
</dependency>
```

# CREATING THE LOGSTASH TCP APPENDER

Once the dependency is added to each service, we need to tell the licensing service that it needs to communicate with Logstash to send the applications logs, formatted as JSON. (Logback, by default, produces the application logs in plain text, but to use Elasticsearch indexes, we need to make sure we send the log data in a JSON format.) There are three ways to accomplish this:

+   - Using the net.logstash.logback.encoder.LogstashEncoder class
+   - Using the net.logstash.logback.encoder.LoggingEventCompositeJsonEncoder class
+   - Parsing the plain-text log data with Logstash

For this example, we will use LogstashEncoder. We’ve chosen this class because it is the easiest and fastest to implement and because, in this example, we don’t need to add additional fields to the logger. With LoggingEventCompositeJsonEncoder, we can add new patterns or fields, disable default providers, and more. If we choose one of these two classes, the one in charge of parsing the log files into a Logstash format is the Logback. With the third option, we can delegate parsing entirely to the Logstash using a JSON filter. All three options are good, but we suggest using the LoggingEventCompositeJsonEncoder when you have to add or remove default configurations. The other two options will depend entirely on your business needs.

> NOTE You can choose whether to handle the log info in the application or in Logstash.

To configure this encoder, we’ll create a Logback configuration file called logback-spring.xml. This configuration file should be located in the service resources folder. For the licensing service, the Logback configuration is shown in listing 11.1 and can be found in the licensing service’s /licensing-service/src/main/resources/logback-spring.xml file. Figure 11.5 shows the log output that this configuration produces.

# Listing 11.1 Configuring Logback with Logstash for the licensing service

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <include resource="org/springframework/boot/logging/logback/base.xml"/>
```