the journey as an incremental improvement to the original design.

We recommend the following approach when thinking about how to move from monolithic legacy design to modern architecture design.

+   1. Modernize the legacy application development and deployment process.
+   2. At this time, it is good to consider language and/or framework upgrades or replacements that may improve the development and deployment lifecycle. For example, if we have a Java Websphere application we may consider moving this to an Apache Tomcat web server framework if the application is not heavily tied into the features of Websphere.
+   3. Modernize to containers where you can leverage Migrate for Anthos to rapidly move your application’s operating environment from VMs to containers. This will help make incremental improvements to the operations of your application that should give your team time back to focus on modernization efforts.
+   4. Adding a Continuous Integration pipeline for building and unit testing your application will help teams identify and fix problems faster. In turn this will give them a quick feedback loop that will give them the confidence they need to start modernizing the application.
+   5. Adding a Continuous Deployment pipeline that deploys your application to a lower environment and performs automated integration or user acceptance testing will further increase the productivity of the team.
+   6. Adding end to end observability and instrumentation is key at this phase. We can no longer rely on logging into a dedicated Virtual Machine to view logs or debug our application. We need the ability to monitor and investigate incidents that may be occurring across a large set of nodes in our operating environment.
+   7. Leveraging a managed container operating environment like GKE or Anthos here will allow your teams to focus on software and deployment innovations rather than operating the container environment.

Next, you will want to modernize the legacy application itself, not just the development and deployment process. This will create agility, velocity and efficiency for the development teams and infrastructure. Breaking the application down into smaller domains and decoupling its capabilities into individual services with well known contracts will create additional