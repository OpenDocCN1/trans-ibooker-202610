Figure 9.2 Each microservice is monitored by polling an endpoint.

![](bbox=[0.18, 0.12, 0.92, 0.58])

Figure 9.3 Every microservice should have two monitoring endpoints: one at /health/startup and one at /health/live. The monitoring system polls both endpoints. As long as both respond with a success response, the monitor considers the microservice to be up.

![](bbox=[0.19, 0.62, 0.88, 0.87])