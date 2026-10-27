```
controlPlaneVIP: 172.16.0.16
ingressVIP: 172.16.0.17
addressPools:
- name: pool1
  addresses:
  - 172.16.0.17-172.16.0.26
nodePoolSpec:
  nodes:
  - address: 172.16.0.7
  - address: 172.16.0.8
```

# Proxy configuration

If nodes need to pass through an http proxy to connect to the internet, populate the proxy: section with the needed information:

+   - url: the url that the proxy server is accessible on in the format http://username:password@fqdn:port
+   - noProxy: A list of IPs, hostnames or domains that should not be proxied

The example below configures an entry for a proxy server accessible at http://172.16.0.101:3128 with no authentication needed and a noProxy entry for the 172.16.0.0/16 range, which tells the system to exclude sending IP’s in that range to the proxy server.

```
proxy:
  noProxy: // specifies a list of IPs, hostnames, and domains th
  - 172.16.0.0/16
  url: http://172.16.0.101:3128 // address of the proxy server.
```

# Cloud Operations for Logging and Monitoring

To configure the options for logging and monitoring, you need to add the projectID and location in the clusterOperations: section:

+   - **projectID**: The project id of the project you want to host metrics and logs in
+   - **location**: A Google Cloud region where you want to store logs and metrics. It's a good idea to choose a region that is near your on-premises data center.