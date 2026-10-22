core subdomains. Core subdomains that deal with monetary transactions, are obligated by law to provide an audit log, or require deep analytics of the system’s behavior are better addressed by the event-sourced domain model.

With all of this in mind, an effective heuristic for choosing the appropriate business logic implementation pattern is to ask the following questions:

+   - Does the subdomain track money or other monetary transactions or have to provide a consistent audit log, or is deep analysis of its behavior required by the business? If so, use the event-sourced domain model. Otherwise...
+   - Is the subdomain’s business logic complex? If so, implement a domain model. Otherwise...
+   - Does the subdomain include complex data structures? If so, use the active record pattern. Otherwise...
+   - Implement a transaction script.

Since there is a strong relationship between a subdomain’s complexity and its type, we can visualize the heuristics using a domain-driven decision tree, as shown in Figure 10-3.

![](bbox=[0.13, 0.65, 0.88, 0.9])

Figure 10-3. Decision tree for business logic implementation pattern