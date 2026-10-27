This can be done using Kubernetes Operators, Container sources or using an existing Services. To see how you can develop your own source refer to the knative eventing documentation (https://knative.dev/docs/eventing/custom-event-source/custom-event-source/)

# 11.3.9 Knative Use Cases

With Knative you can cover a number of use cases from a simple single service to very complex multi microservices applications. Using Knative Serving you can create both HTTP and gRPC [13]services, webhooks and APIs. You are able to manage rollouts and rollbacks. You can easily control the traffic to your application. With cloud eventing you can create simple or very complex eventing pipelines. Combining those two services you can deliver a fully cloud-native event driven applications.

Let's have a look at a simple example of binding running services to an Cloud [14]IoT Core. The messages from the IoT devices are sent to the Google Cloud IoT Core and sinked to PubSub. The Cloud Eventing service uses the PubSub Source to get the messages from the topic. The messages are sent to the Broker and converted to Cloud Events. Trigger makes sure the events are sent to the proper service that can further process, log or display them to the user.

Figure 11.8.