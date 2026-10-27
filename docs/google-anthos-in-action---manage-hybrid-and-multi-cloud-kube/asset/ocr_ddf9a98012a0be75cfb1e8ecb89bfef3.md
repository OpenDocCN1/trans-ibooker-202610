# 8 Hybrid applications in Anthos

by Jason Quek

# This chapter covers

+   - Highly available applications
+   - Geographically distributed applications
+   - Applications regulated by law
+   - Applications which have to run on the edge

In the real world, applications are bound by rules such as data locality requirements, resource constraints, situations where a stable connection to the cloud cannot be guaranteed—such as at a baseball stadium, a construction site or on a fighter jet—or to do low latency computation locally on the edge to avoid large amounts of data transfer. That an application must be available and survive a regional disaster or cloud outage.

However, there still exists a need to run applications with the same consistency and stability that the Kubernetes platform provides. Thus solutions such as Anthos are designed to create this conformant distributed cloud platform for such applications.

In this chapter we will go over the different situations and show various architectures involving the use of Anthos and it’s suite of products to support these types of applications.

# 8.1 Highly available applications

These are a class of applications which have a need to be running 24 hours a day, 7 days a week. Financial institutions managing transactions, healthcare applications, traffic management are just some of the examples of applications which have an impact on the real world if unavailable for a short period of time.