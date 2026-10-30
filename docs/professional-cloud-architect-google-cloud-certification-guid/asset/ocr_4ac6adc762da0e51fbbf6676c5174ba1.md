# Exam Tips

Remember that Cloud Run has a 60-minute timeout, while Cloud Function has a 9-minute (540s) timeout. This means that Cloud Run can be a solution for cases where Cloud Function would time out. However, keep the cost aspects of long-running container instances in mind. Also, Cloud Run can serve up to 250 concurrent requests while Cloud Function can only serve a single thread.

# Pricing

As Cloud Run is a fully managed service, you are charged per use and do not have to worry about the underlying infrastructure. It is quite important to understand what this means as we have learned that containers are provisioned and de-provisioned as requests to access the applications are triggered. In the case of Cloud Run, the charge applies when the application is running, and you are billed to the nearest 100 milliseconds. A charge applies to the time when the following occurs:

+   - The container instance is starting.
+   - The container instance is shut down and it is handling the SIGTERM (https://cloud.google.com/blog/products/containers-kubernetes/kubernetes-best-practices-terminating-with-grace) signal.
+   - At least one instance is serving the request.

The active billable time begins with the start of the first request and ends at the end of the last request:

![](bbox=[0.195, 0.575, 0.795, 0.835])

Figure 7.22 – Cloud Run billing time