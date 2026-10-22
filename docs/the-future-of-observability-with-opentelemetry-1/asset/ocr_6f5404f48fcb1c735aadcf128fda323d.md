independently. Providing agency is a fundamental design requirement for an effective observability system.

Let’s start by identifying the responsibilities of every role related to a running software system: the library author, the application owner, the operator, and the responder.

Library authors communicate what their software is doing.

For software libraries that encapsulate critical functionality, such as network and request management, library authors also must manage aspects of the tracing system: injection, extraction, and context propagation.

Application owners compose software and manage dependencies.

Application owners choose the components that comprise their application and ensure that they compile into a coherent, functional system. Application owners also write application-level instrumentation, which must interact cleanly with the instrumentation (and context propagation) provided by the library authors.

Operators manage the production and transmission of telemetry.

Operators manage the transmission of observability data from applications to responders. They must be able to choose what format the data is in and where it is being sent. While the data is in flight, they must operate the transmission system: managing all of the resources required to buffer, process, and route the data.

Responders consume telemetry and generate useful insights.

To do that, responders must understand both the structure and the meaning inherent to the data. (Structure and meaning are described in detail in Chapter 3.) Responders also need to add new and improved analysis tools to their toolbox when they become available.

These roles represent different decision points:

+   - Library authors can only make changes by releasing a new version of their code.
+   - Application owners can only make changes by deploying a new version of their executable.
+   - Operators can only make changes by managing the topology and configuration of executables.