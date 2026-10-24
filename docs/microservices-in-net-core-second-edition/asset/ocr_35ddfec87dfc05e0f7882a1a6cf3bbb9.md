We can also see a list of recent invocations of the loyalty program event consumer by looking at the Kubernetes jobs list:

```
> kubectl get jobs
NAME                          COMPLETIONS   DURATION   AGE
loyalty-program-consumer-1590755940   1/1          2s         11s
loyalty-program-consumer-1590757080   1/1          2s         1m11s
loyalty-program-consumer-1590757140   1/1          3s         2m11s
```

This concludes the deployment of the loyalty program microservice. The API part as well as the event consumers part is running in our Kubernetes cluster in AKS. The Kubernetes manifest we have created for the loyalty program is the template for the manifests for all the microservices we will create; only the container images will have different names, and the event consumer schedules will vary.

If we want to convince ourselves that the event consumer is indeed running, we can inspect the logs of one of the instantiations of the CronJob:

```
> kubectl logs job.batch/loyalty-program-consumer-1590755940
[{"sequenceNumber":1,"occurredAt":"2020-06-18T18:07:13.7973414+00:00","name":"SpecialOfferCreated","content":{"description":"Nice deal","id":0}},
{"sequenceNumber":2,"occurredAt":"2020-06-18T18:07:17.7957514+00:00","name":"SpecialOfferCreated","content":{"description":"Nice deal","id":1}},
{"sequenceNumber":3,"occurredAt":"2020-06-18T18:07:47.4246091+00:00","name":"SpecialOfferUpdated","content":{"oldOffer":{"description":"Nice deal","id":1},"newOffer":{"description":"Best deal ever - JUST GOT BETTER","id":0}}},{"sequenceNumber":4,"occurredAt":"2020-06-18T18:08:04.5908816+00:00","name":"SpecialOfferRemoved","content":{"offer":{"description":"Nice deal","id":0}}}]
```

Looking at these logs, we will be able to see the special offers event being consumed shortly after we make calls to the special offers API.

With both the special offers microservice and the loyalty program microservice running in AKS, we have finished implementing examples of all three styles of collaboration: commands, queries, and events.

# Summary

+   - There are three types of microservice collaboration:
+     - Command-based collaboration, where one microservice uses an HTTP POST, PUT, or DELETE to make another microservice perform an action
+     - Query-based collaboration, where one microservice uses an HTTP GET to query the state of another microservice
+     - Event-based collaboration, where one microservice exposes an event feed that other microservices can subscribe to by polling the feed for new events
+   - Event-based collaboration is more loosely coupled than command- and query-based collaboration.
+   - You can use HttpClient to send commands to other microservices and to query other microservices.