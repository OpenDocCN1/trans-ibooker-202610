+   - Health checking
+   - Routing service requests
+   - Load balancing
+   - Authentication and authorization
+   - Providing observability

Hence, the data plane fundamentally touches every network request or packet in the cluster.

The control plane, on the other hand, provides the policy and configuration details for all the sidecar proxies (data planes) in the service mesh. The control plane allows you to control and configure the data planes. All the high-level configuration is sent to the control plane, which is then translated to the data-plane-specific configuration. The control plane assists the data plane by:

+   - Routing between services
+   - Populating service discovery data for data plane usage
+   - Specifying inputs for configurations responsible for load balancing, circuit breaking, and timeouts
+   - Configuring validation, authentication, and authorization settings in the cluster

In the next section we will take a look at Istio, which acts as the control plane to service mesh in a modern cloud native environment.

# Introduction to Istio

As monolithic applications transitioned toward microservices, developers and operations teams experienced challenges with regard to how the services would talk to one another in the service mesh. As a service mesh grows in size, it becomes difficult to manage and maintain. It also becomes difficult to even understand how the different moving parts work together.