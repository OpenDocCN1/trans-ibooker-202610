the change will not be logged in the repository, and two situations can occur: changes get lost with newer versions of the service or we are forced to copy the changes to the new version of the service. Figure 2.8 shows a high-level architectural example for this best practice.

![](bbox=[0.191, 0.172, 0.794, 0.34])

**Figure 2.8** It’s in our best practice to strictly separate the build, release, and run stages of the microservice.

# 2.3.6  *Processes*

Your microservices should always be stateless and should only contain the necessary information to carry out the requested transaction. Microservices can be killed and replaced at any time without the fear that a loss of a service instance will result in data loss. If there is a specific requirement to store a state, it must be done through an in-memory cache such as Redis or a backing database. Figure 2.9 shows how stateless microservices work.

![](bbox=[0.192, 0.569, 0.558, 0.734])

**Figure 2.9** Stateless microservices don’t store any session data (state) on the server. These services use SQL or NoSQL databases to store all the information.

**We can store the cart service data in an SQL or NoSQL database.**

# 2.3.7  *Port binding*

Port binding means to publish services through a specific port. In a microservices architecture, a microservice is completely self-contained with the run-time engine for the service packaged in the service executable. You should run the service without the need for a separate web or application server. The service should start by itself on the command line and be accessed immediately through an exposed HTTP port.