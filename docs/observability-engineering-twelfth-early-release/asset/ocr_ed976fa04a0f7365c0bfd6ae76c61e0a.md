+   - There is one stateful data store (“the database”), which you run
+   - Many low-level system metrics are available (e.g., resident memory, CPU load average).
+   - The application runs on containers, virtual machines (VMs), or bare metal, which you control.
+   - System metrics and instrumentation metrics are the primary source of information for debugging code.
+   - You have a fairly static and long-running set of nodes, containers, or hosts to monitor.
+   - Engineers examine systems for problems only after problems occur.
+   - Dashboards and telemetry exist to serve the needs of operations engineers.
+   - Monitoring examines “black-box” applications in much the same way as local applications.
+   - The focus of monitoring is uptime and failure prevention.
+   - Examination of correlation occurs across a limited (or small) number of dimensions.

When compared to the reality of modern systems, it becomes clear that traditional monitoring approaches fall short in several ways. The reality of modern systems is as follows:

+   - Your application has many services.
+   - There is polyglot persistence (i.e., many databases and storage systems).
+   - Infrastructure is extremely dynamic, with capacity flicking in and out of existence elastically.
+   - Many far-flung and loosely coupled services are managed, many of which are not directly under your control.