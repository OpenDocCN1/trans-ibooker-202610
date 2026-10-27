+   - *Simplified infrastructure management*—CaaS cloud providers give you the ability to have more control over your services. New services can be started and stopped with simple API calls.
+   - *Massive horizontal scalability*—CaaS cloud providers allow you to quickly and succinctly start one or more instances of a service. This capability means you can quickly scale services and route around misbehaving or failing servers.
+   - *High redundancy through geographic distribution*—By necessity, CaaS providers have multiple data centers. By deploying your microservices using a CaaS cloud provider, you can gain a higher level of redundancy beyond using clusters in a data center.

# Why not PaaS-based microservices?

Earlier in the chapter I discussed five types of cloud platforms—Infrastructure as a Service (IaaS), Container as a Service (CaaS), Platform as a Service (PaaS), Function as a Service (FaaS), and Software as a Service (SaaS). This book focuses specifically on building microservices using a CaaS approach. While certain cloud providers will let you abstract away the deployment infrastructure of your microservice, this book will teach you how to remain vendor independent and deploy all parts of the application (including the servers).

For instance, Cloud Foundry, AWS Elastic Beanstalk, Google App Engine, and Heroku give you the ability to deploy your services without having to know about the underlying application container. These provide a web interface and command-line interface (CLI) to allow you to deploy your application as a WAR or JAR file. Setting up and tuning the application server and the corresponding Java container are abstracted away from you. While this is convenient, each cloud provider’s platform has different idiosyncrasies related to its individual PaaS solution.

The services built in this book are packaged as Docker containers; the main reason is that Docker is deployable to all major cloud providers. In later chapters, we’ll see what Docker is and learn how to integrate Docker to run all the services and infrastructure used in this book.

# 1.6  Microservices are more than writing the code

While the concepts around building individual microservices are easy to understand, running and supporting a robust microservice application (especially when running in the cloud) involves more than just writing the code for the service. Figure 1.10 shows some guidelines to consider while writing or building a microservice.