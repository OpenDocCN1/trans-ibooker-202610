The first allows us to play with some sample data, and the second lets us explore the generated data from our services. Figure 11.7 shows Kibana’s Welcome page.

![](bbox=[0.191, 0.133, 0.872, 0.414])

**Figure 11.7** The Kibana Welcome page with two options: trying out the application with sample data or exploring it on your own.

To explore our data, let’s click the Explore on My Own link. Once clicked, we will see an Add Data page like the one shown in figure 11.8. On this page, we need to click the Discover icon on the left side of the page.

![](bbox=[0.191, 0.563, 0.877, 0.758])

**Figure 11.8** The Kibana setup page. Here we will see a set of options we can use to configure our Kibana application. Notice the icon menu on the left.

In order to continue, we must create an index pattern. Kibana uses a set of index patterns to retrieve the data from an Elasticsearch engine. The index pattern is in charge of telling Kibana which Elasticsearch indexes we want to explore. For example, in our case, we will create an index pattern indicating that we want to retrieve all of