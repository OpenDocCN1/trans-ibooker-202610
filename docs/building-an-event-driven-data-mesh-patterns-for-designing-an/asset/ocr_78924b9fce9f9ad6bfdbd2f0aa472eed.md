![](bbox=[0.17, 0.07, 0.25, 0.16])

The ability to self-register as a new consumer is a good acid test for self-service capabilities because it requires integration between the user's identity, application service accounts, and event-stream read/write access controls.

Data product management remains a second major requirement of a self-service platform. Users must be able to identify streams that they could publish, register them as data products, and deprecate any data products that are no longer needed:

# Identify candidate data products

The system must provide the user with a list of event streams that they could register. Semantically, these are streams that fall within the domains that the user belongs to or otherwise have some form of jurisdiction over. One simple option is to allow only the user who created the event stream to register it as a data product. A more advanced option consists of integration with the event broker's user and organization accounts (covered next in “Service and User Identities” on page 110), such that only users belonging to the team or group that own the stream can manage it.

# Register a data product

Once the data product owner identifies an event stream they want to publish as a data product, they can ideally click a big Register button to start the registration process. They should then be prompted for the mandatory metadata as specified by federated governance, such as Name, Owner, SLA, Quality, and Description. Additional properties should automatically be pulled in, such as the underlying stream name, event broker address, schema, and metadata pertaining to permissions and access controls.

# Deprecate and deregister a data product

Eventually a data product may need to be removed. Deprecation would lock the data product down so that only existing consumers could continue to use it, while simultaneously marking it as deprecated and concealing it from search results. The data product owner is then responsible for providing a deprecation plan for its current users, including a migration plan and a timeline for when the work must be completed. You should ensure that only deprecated data products with zero existing registered consumers can be deleted, to prevent accidental deletion of system-critical data products.

Figure 5-5 is an example of what a management UI may look like. Starting from the top, you can see both the domain and user logged into the console. Under the “Unregistered streams” header are possible candidates for publishing as a data product—there is currently only one, the stream named Ecom.Orders.USA. These streams are shown to only the active user because they’re associated with the ownership of this stream. Finally, the two currently registered data products are shown