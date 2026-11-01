
## DevOps in the Product Lifecycle

Figure 3-3 shows the steps in the DevOps framework.

![](img/f561fe6de202c361a03741b184bee8ea_50_0.png)

Figure 3-3. DevOps framework

1. The Agile practices in business planning step establishes business goals and adjusts them based on customer feedback, which helps improve agility and business outcomes. This helps in gaining the trust and confidence of the customers on the value being delivered through the deliverables. This first phase in the product lifecycle is a stepping-stone in building the product journey correctly. With the right adoption of tools and techniques, this phase becomes essential and integrated with other phases. Conducting the right ceremonies, estimating and planning for delivering faster, and embracing the culture of change within the team all play a vital role. This phase is generally managed through tools such as Atlassian Jira, Azure Boards, VersionOne, LeanKit, etc. These tools are extended with other lifecycle tools to get end-to-end traceability.

2. *Continuous integration* is a practice in which software developers frequently integrate their code in the codebase with the code of the application where other team members also add their code. This helps in the early detection of integration defects while building the code, which if caught at a later stage will prove to be expensive to remediate. Building a CI pipeline is a common practice that is running across many organizations today and is a de facto standard too. While this pipeline was implemented in the application space, today it is applicable for building infra pipelines too. Tools like Jenkins, TravisCI, CircleCI, TeamCity, etc., are all well-known tools that orchestrate the key steps, from building the code to delivering the executables/binaries. A complete CI pipeline comprises source code management, build execution, unit testing, code coverage, and artifact/binary deployment.

3. The *environment build* focuses on instant infrastructure provisioning by adopting runbook automation, configuration management tools, and self-service catalogs. Automation in this space helps to reduce IT sprawl. Building an environment comprises various activities such as provisioning and configuring infrastructure, preparing runbook automation, performing and automating security scans, and integrating infra pipelines with ITSM tools. In successive sections, we will be covering examples on how CI is implemented in application and infrastructure space and how they are integrated.

4. The *continuous delivery* practice focuses on releasing the product across different environments. A well-defined release and deployment process ensures timely delivery of a quality product. Every stage in deployment passes through a series of quality checks. After its success, it is then deployed in the target environment. For example, if the CI build is successful, the CD pipeline picks up binaries from the development environment and deploys them into the test/QA environment. If the CI pipeline is not successful, the deployment will not proceed. Similarly, if the artifacts have to be transferred from the test/QA environment to preproduction, artifacts will require more quality gates such as percent coverage that is configured and accepted, security score, etc. Tools such as Jenkins, Azure DevOps, GitHub Actions & Runners, Atlassian Bamboo, etc., are good examples that perform continuous deployment operations.

5. *Continuous testing* is a practice that means testing earlier and continuously to detect defects early in the lifecycle, which will result in reduced costs. This helps in establishing continuous feedback on the quality of the product. Continuous testing can be achieved by having your test cases automated and executed with each code integration and build process. Testing scope has expanded today, and it is not restricted just to functional, performance, and security testing. As applications are moving toward microservices-based architecture, addressing the growing market needs of the customer base, the need for extended testing has become essential including API testing, accessibility testing, resiliency testing, etc. A few tools in this space are Selenium, Appium, JMeter, HCL One Test Suite, etc.

6. The *continuous monitoring and feedback* practice involves monitoring applications and infrastructure across all phases and also acknowledging feedback from customers. This will help to lay out actions to optimize and improve the application and thus enhance customer experience and value. Every incident or problem with the deployed application is closely monitored and addressed with agility. ITSM tools like ServiceNow, Remedy, etc., come to the rescue in this space.

So, we have agile principles and a DevOps-connected model that enables organizations to work closely and move faster.

## Summary

Agile and DevOps complement each other, but they should not be considered as replacements for each other. On one hand, if agile focuses on iterative development with continuous feedback principles, DevOps focuses on bringing teams together that collaborate with each other and plan for a continuous journey of improvements.

Organizations that practice agile easily transition and extend into a DevOps working model. IT teams that practice agile and DevOps reap long-term benefits such as the following:

- Collaborative and self-organized teams
- Embracing change through trust and transparency
- Faster time to market with automation
- Continuously improving with feedback loops
- Simplified processes and integrated workflows
- Lower costs
- Higher customer satisfaction scores
- Better business alignment

Let’s next look at the key factors that are accelerating the transformation of infrastructure operations to agile.

## CHAPTER 4

### Factors Leading to Agile Operations

In this chapter, we will be discussing the shift toward agile and microservices, deployments, and how continuous testing happens at various levels. The topics you’ll learn about in this chapter are as follows:

- The shift toward agile
- The benefits of agility
- Cloud computing
- Microservices architecture
- Deployment patterns and automation
- Shift-left testing
- Changes in architecture impacting operations

Digital organizations are adopting agile operations these days since it has become a necessity. In a May 2021 survey by McKinsey (https://www.mckinsey.com/business-functions/organization/our-insights/the-impact-of-agility-how-to-shape-your-organization-to-compete), it was observed that highly successful agile transformations resulted in 30 percent of operational performance that was driven by continuous improvements and removing hand-overs. The survey also measured the agile operating maturity model (for the 2,190 respondents) and its business impact. About 10 percent of the entire sample was driving successful agile transformation wherein agility was driven at scale to create value. The survey also revealed that agile transformation is now becoming a mainstream topic of interest.

There have been similar surveys conducted across different geographies and industries, and they indicate that operating models are getting transformed. But this did not happen overnight. Emerging platforms like the cloud and new software development models like microservices contributed to the need for an agile approach to business. The demand for agility, resiliency, cohesive teams, secured environments, quick turnaround times, digitalization, intelligent automation, and end-to-end integration served as motivators for infrastructure operations teams to adopt agile. See Figure 4-1.

![](img/f561fe6de202c361a03741b184bee8ea_55_0.png)

Figure 4-1. Common challenges faced with traditional operations

It is true that the vision for an operations teams is to ensure that systems are stable. Traditional ways of working were good until now, but these norms are now set to change in a way that meets and sustains the needs of digital businesses. So, the need of the hour is to shift from traditional ways of operations to agile operations. While the vision remains the same, the underlying ways of working must be transformed. This creates a demand for upskilling our teams as well as introducing and adopting agility
