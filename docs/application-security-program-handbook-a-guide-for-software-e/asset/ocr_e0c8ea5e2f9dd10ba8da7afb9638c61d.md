# 3.5.1 Integrating with defect tracking

In order to be effective at managing vulnerabilities the first thing that most organizations will do is integrate the security tools with their defect tracking tool. That might be Jira, or Bugzilla, or something else, but in any case, this will allow the developers to see the vulnerability information as it becomes known from the various scanning tools in the organization. For instance, the DAST tool that is integrated with the testing environment may detect a vulnerability when the developer's code was deployed to the testing environment. This DAST tool can then call an API with the defect tracking tool and open an issue to either the application security team to triage, or directly to the development team to resolve. Which one depends on the confidence of the security tool that detected the vulnerability and the maturity of the organization.

Opening defects on teams will help get the organization to resolution on the open vulnerabilities but being able to have visibility in to the all the open vulnerabilities across an organization can be challenging. There are usually different defect tracking tools being used unless the organization has tried to standardize. This means that the application security team cannot necessarily rely on building a dashboard in a single defect tracking tool. Often the application security team will build their own dashboards that pull information from the different defect tracking tools, or the security analysis tools that are being used. The application security team is then on the hook for managing this dashboard and enhancing it along the way as technology changes.

Assuming that the organization has access to a centralized dashboard that shows the various open vulnerabilities, the hard part becomes apparent:

+   - How do you get these vulnerabilities closed?
+   - How do you prioritize the vulnerabilities?
+   - Who owns the vulnerabilities?
+   - What if the vulnerability cannot be closed?
+   - Is there enough information for the development team to provide mitigation?

These are all questions that each application security team faces when the begin to review the open vulnerabilities. One of the most critical questions above is regarding ownership. Software is complex and is pieced together by several different components as I discussed earlier. When a vulnerability is found, for instance, in a specific part of an application, it is often difficult to locate the team that has developed that code. This is especially true where an organization has something like a team that develops common components that are leveraged by most of the applications in the organization. The application security team needs to have an inventory of the applications in the organization as well as resources to contact that are responsible for resolving found vulnerabilities. This may be not just the engineering leader for the application like the technical manager, but also the project manager and even the product owner. Both of the later resources typically maintain the control over what gets worked on in a given timeframe, not the engineering leader.

# 3.5.2 Prioritizing vulnerabilities

Before even being able to request that the engineering team tackle a vulnerability, it’s important to ensure that they organization has clear direction on timeframes for resolution.