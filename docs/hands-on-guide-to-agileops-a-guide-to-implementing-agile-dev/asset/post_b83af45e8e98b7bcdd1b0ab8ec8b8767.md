
- a. System availability = 90%
- b. SLO = 80%
- c. So, the Error budget = Availability – SLO; which when calculated with values from (a) and (b) i.e. 90% - 80%; results to 10%. Thus, the error budget value is 10%.
- d. This when translated into months results into 72 hours in a month.

This means that even if the product is down for up to three days in a month, it will not breach the SLAs.

- *Reduce the cost of failure*: This aspect corresponds to the two guiding principles in DevOps that are “gradual changes” and “failures are normal.” The SRE team focuses on resolving the problems early in the cycle of development so that the failures do not impact production systems. There are various processes and practices that the SRE team uses to achieve this end objective. Involvement of SREs in the design and architecture stages helps to proactively plan for high availability and in the case of a failure restore quickly and thus reduce the cost of failure.
- *Roll back early, roll back often*: When errors are found or suspected, the first thing the team does is to roll back to the previous version and then continue to explore the problem. Thus, the first step is to recover the system and then focus on exploring the problem and undertaking problem management. This results in a higher availability of systems and services and a lower mean time to resolve. See Figure 6-20.

## DID YOU KNOW?

## MTTD vs MTTF vs MTBF vs MTTR ?

MTTD - Mean Time To Detect, average amount of time it takes to discover an issue.
MTTF - Mean Time To Fail, average amount of time a defective system can continue running before it fails.
MTBF - Mean Time Between Failures, a metric to measure the ability of a system to perform required functions under stated conditions.
MTTR - Mean Time To Resolve, time spent in fixing a system.

![](img/f561fe6de202c361a03741b184bee8ea_160_0.png)

*Figure 6-20. Did you know?*

In recent times SRE and DevOps have become extremely important and sought-after skills in organizations. Everyone has their own definition of what the role or responsibility of this team is, and there are various team structures in which these teams are structured. There is no universal definition of a DevOps engineer or an SRE, and there are overlaps in skill set and responsibilities. Let’s try to capture the two roles, including the differences and overlaps between the two roles.

## SRE and DevOps Structures

In the SRE team, it is expected that every team member is skilled on all areas and becomes multiskilled or E-shaped in terms of skills. In DevOps it is about different team members with different skills coming together to deliver an integrated development and operations experience with the help of culture, processes, best practices, tools, and technologies.

The team structure varies from company to company and in general has the following specialists:

- *Product owner*: This person is the interface between the business and the product and manages the product roadmap and strategy for the product.
- *Infra architect*: This person is responsible for the cloud and noncloud infrastructure architecture including networking, storage, compute, and other elements.
- *Software developer*: This person creates code and test cases, there may not be a separate testing team, and developers may double up as testers writing automated tests.
- *QA engineer*: This person is responsible for the overall quality of the product and ensuring the QA processes of third-party testing, etc., are handled.
- *Release manager*: This person is responsible for the release management function including release plans and scheduling.
- *Administrators (system and application)*: This person is in charge of monitoring, management, and patching activities along with automation for provisioning and deprovisioning of infrastructure and availability of these systems.

As can be seen from the previous roles, the sysadmin role morphs into the SME role in organizations where the strategy is to walk the SRE path. Also, when SREs are part of the DevOps teams, there may be some activities that are done by other teams and not the SRE team, especially releasing a deployment.

## Tools and Technologies in DevOps and SRE Domain

Since there is an overlap between SRE and DevOps and since the way teams and individuals are structured around groups is different from organization to organization, the following skills are essential for both teams. The only difference will be that the resources in the SRE model may be cross-skilled across some of them, while in the DevOps model, the culture, best practices, and guiding principles will allow the DevOps team to seamlessly handle the entire spectrum of technologies required to deliver an end-to-end service. See Figure 6-21.

![](img/f561fe6de202c361a03741b184bee8ea_162_0.png)

Figure 6-21. Evolution of SREs

Containers and microservices: Docker and Kubernetes services are available as SaaS offerings from various Cloud providers like AWS, GCP, and Azure.

*Monitoring tools:* Tools like Prometheus, Zabbix, etc., are important elements of the service that are required to deliver ongoing operations. Various organizations may be using tools from COTS vendors like CA Broadcom, IBM, Microfocus, BMC, Solarwinds, and Zenoss.

*CI/CD/testing tools:* Tools like Jenkins, Git, Gitlab, One Test Suite, Selenium, AppScan, Whitesource, SonarQube, Microsoft TFS, Azure DevOps, and other cloud-native DevOps tool chains along with deployment and configuration management tools like Ansible, Puppet, and Chef fall into this category.

*Infrastructure as code (IaC):* Native cloud technologies like Cloud Formation templates, Deployment Manager templates, and ARM templates along with third-party tools like Terraform, Puppet, Chef, and Ansible are the backbone of automation. Along with these, AI-driven COTS tools like DRYiCE iAutomate and DRYiCE MyCloud are gaining traction with customers.

*Resilience testing:* This is a key tool in the hands of the SRE teams especially. These tools provide end-to-end testing on production systems to test their resilience by bringing down components and testing if the service or application withstands the failure of these components. Chaos Monkey originally created by Netflix is a widely used tool.

## DID YOU KNOW?

![](img/f561fe6de202c361a03741b184bee8ea_163_0.png)

## What is Chaos Monkey?

Chaos engineering is a discipline wherein systems are experimented for their capability to withstand turbulent scenarios in production. Chaos Monkey is a tool wherein production systems are randomly terminated to test resiliency.

*Figure 6-22.* Did you know?

## Balance Between Dev and Ops Work

When you move to a DevOps mode of operations and integrate the teams or make developers responsible for all kinds of operations, another challenge arises. The developers will now get overwhelmed with unplanned work, customer escalations, availability, and downtime of infrastructure and regular maintenance jobs. This may lead to dissatisfaction in the development teams, and the teams may also get distracted from the core job of developing features. Thus, SRE plays an important role here. They focus on the resilience aspects and also work on monitoring and ensuring the entire IT landscape from infrastructure to applications is up and running.

In this model, since the availability SLAs are well defined and there is an error budget defined, the focus of the SRE on availability and resilience ensures that prioritization is done systematically through metrics. Another point to note here is that the error budget or breaches of availability targets would automatically prioritize system stability over feature releases, and the focus of the team would be to ensure that the releases that may hamper availability and impact the metrics are planned and coordinated with the development teams.

An Agile-based Scrum backlog is used by the SRE teams for automation and engineering activities such as problem management and change management. Also, ITIL-based SLAs related to availability and prioritization using priority and severity (along with time to respond and resolve) are metrics used to prioritize the incidents happening in the IT landscape. This creates a balance between the two and ensures agility and speed while keeping the lights on and ensuring the services are available for the end consumers. The following method defines the priority:

- Pull things from the triage queue.
- If there are no triage items, work on the next item from the sprint backlog.

At first the teams may have a conflict between the SLA-driven workflow and the agile-based sprint work. Questions that you may find during the initial stages are “What if there is a lot of support work; how would we get time for sprint deliverables?” Once you are able to overcome the initial challenges, you will soon find that the work will balance out on a longer timeframe, and the teams will be able to complete the sprint tasks as well as take
