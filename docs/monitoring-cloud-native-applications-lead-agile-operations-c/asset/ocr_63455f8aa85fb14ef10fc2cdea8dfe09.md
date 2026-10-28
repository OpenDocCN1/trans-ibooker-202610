do fault-finding for a scenario which happened momentarily and automation subsequently restored the systems back to normal behavior. Static alert policies are useless in an environment under constant change; therefore, alerting mechanisms should dynamically adjust to the current environment.

+   - **Managed services:** Due to developers relying on more and more managed services available from cloud infrastructure providers, the common operations responsibilities of operations teams are increasingly getting abstracted. Operation teams now need alerting higher up the stack to monitor and alert on business KPI performance.

Most organizations tend to put alerts on a variety of parameters; however, most alerts do not warrant an immediate human intervention. For example, in case CPU utilization of a container pod increases drastically, you would not want to put an alert on that event, as Kubernetes will bring the utilization levels down by initiating autoscaling. It is therefore very important to identify where to put an alert and at what threshold. In a distributed system, setting up alerts on resources is not advisable as the resources will always be in a flux. You need a different strategy to win the battle of alerts. In the next section, we will take a look at an effective alerting strategy for modern workloads.

# Alerting Strategy

An effective alerting strategy can bring significant benefits to an organization and help in running your operations efficiently. It is worthwhile to set up alerts on your systems and modify them as needed in order to effectively refine your alerting strategy. As a starting point, you can

+   - Identify a set of systems that are mission critical for the business
+   - Set up alerts on these systems and review them time to time for effectiveness
+   - Review your incident life-cycle process as part of any active incident retrospective
+   - Include alert reviews and tuning in an ongoing process for iteration and improvement