
# Mock test two

1. Company X wants a standardized re-deployable Hadoop cluster, with options that a managed service doesn't offer. Which solution would be best suited?
A. A Cloud API
B. Deployment Manager
C. Dataflow
D. TensorFlow

2. Company X is looking to connect their backend platform to a managed NoSQL database service. There is an expectation that the databases could grow into PB scale. As an architect, they ask you which is the best GCP service to fit these requirements without needing to refactor any applications. What is the best fit?
A. MySQL
B. Bigtable
C. Firebase
D. Redis

3. Select the different types of service accounts (choose three):
A. User-managed
B. Automated
C. Google-managed
D. G Suite
E. Google APIs

4. Company X has two projects, separated by different VPCs that need to be able to communicate with one another. Which network service allows this?
A. VPC peering
B. Cloud Load Balancing
C. Dedicated Interconnect
D. VPN

5. Company X is looking to use containers in the cloud. They want to continue to be developer-focused and have a code-first strategy. What is the best solution?
A. App Engine standard
B. Containers on Compute Engine
C. Cloud Run
D. App Engine flexible

6. Your IT manager is looking at cloud vendor data storage services. His DBA has informed him that the principal requirements are strong consistency and high availability, with the potential to grow to PB scale. What is the best storage solution?
A. Cloud SQL
B. Cloud Storage
C. Cloud Datastore
D. Cloud Spanner

7. Company X needs to be PCI-compliant. Which combination of GCP services would help to meet these requirements?
A. Cloud Monitoring, Cloud Trace, and Cloud Spanner
B. Cloud Monitoring, Cloud Logging, and BigQuery
C. Cloud Error Reporting, Cloud Debugger, and Datastore
D. Cloud Tagging, Cloud Trace, and BigQuery

8. A storage engineer for Company X needs to migrate data from his AWS S3 bucket to his GCP storage bucket. What is the best solution for this?
A. Storage Transfer Service
B. Transfer Appliance
C. Online transfer
D. BigQuery data transfer

9. A company web page is serving users all over the globe. They want to make sure that users will always get content in the most efficient manner, regardless of where they are located. Which load-balancing solution would best fit these requirements?
A. Network Load Balancing
B. Internal Load Balancing
C. HTTP(S) Load Balancing
D. TCP Proxy Load Balancing

10. Company X is looking to the cloud to achieve autoscaling. They wish to deploy over multiple zones in a standardized manner, while also benefiting from load balancing. What GCP service best suits this scenario?
A. Deployment Manager
B. Managed instance groups
C. Google Compute Engine manager
D. Instance fleet

11. You are creating new firewall rules and wish to identify specific targets according to their use, for example, a web server. Which filter should you use?
A. Zones
B. Network tags
C. Instance groups
D. Targets

12. You have deployed an instance into the same VPC as already-existing instances. When you try to use SSH to connect to the external IP address, the connection is refused. Why might this be?
A. The firewall rule to allow SSH is restricted to internal traffic only.
B. There is no external IP allocated to the instance.
C. You do not have the correct custom Identity and Access Management (IAM) role to initiate SSH.
D. You should use the Google API for external SSH.

13. At the moment, your IT department is seeing lots of bugs reported whenever a new software update is released for the company’s internal timesheet application. These bugs were not spotted during QA. You have been asked to design a new strategy that will keep the bugs to a minimum and regain confidence in the IT department. Which option best suits this scenario?
A. Advise that you should only deploy updates once per year.
B. Deploy only part of the update to production.
C. Perform the tests more times during QA.
D. Use canary deployment methods.

14. Your company is looking to connect its onsite networks to a GCP VPC in order to dynamically exchange routes between each site. Which service would you advise?
A. Cloud Router
B. Cloud Interconnect
C. External peering
D. Cloud DNS

15. You plan to connect VPC networks using VPC peering. What network mode is best suited?
A. Auto mode networks
B. VPC VPN networks
C. Custom mode
D. Sub-networking mode

16. You have been tasked with researching different methods to extend your on-premises network to your GCP VPC network. You are reminded by your manager that your network bandwidth is 1 Gbps. What would be the best option?
A. Dedicated Interconnect
B. Partner Interconnect
C. VPC Interconnect
D. VPN Interconnect

17. Company X wants to extend their data center to the cloud. You have been hired as an external consultant to advise on the best hybrid connectivity option. They advise you that they need access to private compute resources on GCP but are not worried about encryption at the application level. What option best corresponds to their needs?
A. Cloud VPN
B. Partner Interconnect
C. Direct Peering
D. Carrier Peering

18. You want to serve all of your content with low latency, worldwide. Which GCP service should you use?
A. Cloud CDN
B. Cloud VPN
C. Google CloudFront
D. Cloud Endpoints

19. You wish to load balance your systems based on incoming ports. What load balancing concept should you use?
A. Network Load Balancing
B. TCP Load Balancing
C. HTTP(S) Load Balancing
D. SSL Proxy Load balancing

20. You are looking to allow access to publish messages to a Cloud Pub/Sub topic. Your security team reminds you that you should be as granular as possible. Which type of IAM role should you use?
A. Primitive role
B. Predefined role
C. Custom role
D. Policy-based role
E. Topic role

# Mock test three

1. You build a container image using the Cloud Build service. You want to access information such as the Google Cloud project where their image is built. What is the recommended way to do it?
A. Use substitutions in your build config file to substitute specific variables at build time.
B. Run a gcloud command in your build.
C. Run an API call to get the information.
D. It is not possible to access this information.

2. You are running a web application on a Linux distribution. You want to completely remove the overhead of patching the operating system. Which option best suits your requirements?
A. Containerize the application and use managed base images.
B. Make the VMs read-only.
C. Use an Alpine (stripped-down) image for your VMs.
D. Use Google's OS patching service.

3. You are tasked with containerizing a classic LAMP application. What would be the best practice you should follow (choose two options)?
A. Package a single app per container.
B. Package all apps into a single container.
C. Remove unnecessary tools.
D. Use public images.

4. You moved your application to GKE and want to see how the application reacts to faults caused by a single microservice not being available. What is the easiest way to do this?
A. Write a script that will randomly kill microservices.
B. Enable Istio on the GKE cluster and use fault injection.
C. Deploy the application from a YAML file that was edited to remove the code related to the microservice you want to test.
D. Move the microservice to a GCE VM and power off the VM during the tests.

5. You are adding a new feature to your application and decided to use Cloud Functions. Your application is on a GCE VM running within a VPC. The Cloud Function needs direct network access to that VM. How can this be achieved?
A. Use private services access.
B. User Serverless VPC Access.
C. Use Private Google Access.
D. Use Private Service Connect for Google APIs.

6. You want to scale your Managed Instance Group (MIG) based on a custom metric you created. Which option suits your requirements?
A. Create a Cloud schedule and Cloud function to query the metric and then scale the MIG.
B. Create a Cloud operations suite alert to trigger the scale out event.
C. Set up your MIG to export the custom metric from all VMs in the group.
D. Custom metrics are not supported by MIGs.

7. You want to measure how well your web application hosted on a GKE cluster is performing. Which option best suits your needs?
A. Periodically perform a survey of customer satisfaction among the users.
B. Create Cloud Monitoring health checks.
C. Use a third-party SaaS solution to perform periodic checks of HTTP 400 responses.
D. Define and set SLIs and SLOs using Cloud Monitoring.

8. You want to make sure only authenticated and authorized users can access your application running on a GKE cluster. What is the Google-recommended design?
A. Create a frontend container with proprietary authentication and an authorization mechanism.
B. Use Identity-Aware Proxy and GKE Ingress.
C. Move your application to Cloud Run and set up authorization.
D. Integrate your application with Active Directory using LDAP.
