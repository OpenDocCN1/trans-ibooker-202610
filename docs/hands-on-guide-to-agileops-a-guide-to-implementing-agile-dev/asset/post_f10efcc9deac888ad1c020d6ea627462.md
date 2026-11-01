
![](img/f561fe6de202c361a03741b184bee8ea_72_0.png)

Figure 4-15. Static code analysis with SonarQube, Dashboard view

## Infrastructure Testing

Infrastructure testing verifies whether the application works with the intended hardware and network. The aim is to test the infrastructure between different environments whenever the new software is ready for deployment. This helps to mitigate risks during migration and production movement of workloads. This type of testing is also performed when infrastructure-related activities are to be performed such as patching, addition of new devices, etc. Various methodologies are available that enable product teams to perform infrastructure testing such as client-server infrastructure testing, network-level testing, cloud testing, etc. See Figure 4-16.

![](img/f561fe6de202c361a03741b184bee8ea_73_0.png)

Figure 4-16. Examples of infrastructure testing

## Smoke Testing

Smoke tests are generally used in integration, system, and acceptance testing where major product functions are tested randomly but are not covered in depth. The intention is to test if the software deployed is running or not. Hence, it is also known as *confidence testing* or *build verification testing*. This testing is performed by the QA testers when the application is released to the QA/test environment, and the process can be manual or can be automated. If these smoke tests pass, then the QA testers may proceed to functional and resiliency testing and, in the case of failures, notify the development team that the build has failed. See Figure 4-17.

![](img/f561fe6de202c361a03741b184bee8ea_74_0.png)

Figure 4-17. Smoke testing workflow

## Functional and Performance Testing

This form of testing has been in practice for quite a long time where QA testers test the application against its functionalities and performance. This is known as *black-box testing*. This is a type of testing where the QA department has no clue on how the application has been developed. The testers have a list of test scenarios and test cases that are executed either manually or using automated tools, and the results are specified as pass or fail. Commercial tools such as HCL’s OneTest (an integrated suite for functional, performance, service virtualization, and API testing), HP UFT (unified functional testing), HP LoadRunner, or open source tools like Selenium, JMeter, etc., are commonly used. See Figure 4-18.

![](img/f561fe6de202c361a03741b184bee8ea_75_0.png)

Figure 4-18. Developing performance testing suites

## Security Testing

This testing category uncovers vulnerabilities in the system and determines that the data and resources of the system are protected from possible breaches. It focuses on finding all the possible loopholes and weaknesses of the system that could lead to loss of information. It also ensures that the system is free from threats or risks, which can cause data loss. With the increase in digitalization, organizations have seen a rise in security threats and hence are investing in various software solutions that proactively scan for such threats and provide these vulnerabilities to the teams on time. There are different forms of security testing that are performed at various levels by different specialists to ensure that the product getting released is tested well, and when it goes live, it continues to monitor the system as well. Tools such as Nessus, HCL AppScan, Qualys, Veracode, etc., are some of the commonly used tools in this space. In fact, this type of testing is introduced in infrastructure as code pipelines wherein security scans are automated to avoid manual checks. For example, the image hardening process requires a compliance check, a process wherein organizations harden a VM image comprising the organization’s compliance ruleset. This is done since organizations do not allow the use of VM images directly from the Internet in order to avoid any security issues; this can easily be automated using the existing security tools. If all goes well and the pipeline gets an approval from the security team, the image is moved to a shared gallery, which is accessible to everyone within the organization. See Figure 4-19.

![](img/f561fe6de202c361a03741b184bee8ea_76_0.png)

Figure 4-19. Security testing types

## User Acceptance Testing

User acceptance testing (UAT) is another form of testing that is performed by end users. This is done just before the actual launch in production. The actual users are given access to the new setup to verify the application functionality based on their real-world scenarios. A UAT environment is made ready for the end users to perform this testing with the new application version. The UAT testers have their own set of test cases that are executed in this environment, and the results are recorded. If the user acceptance tests pass, then the end users or the customers provide sign-off for the actual deployment in production; otherwise, the product team is notified for taking steps to improvise. See Figure 4-20.

![](img/f561fe6de202c361a03741b184bee8ea_76_1.png)

Figure 4-20. UAT testing phases

## Chaos Testing

A new form of testing was conceptualized by Netflix in 2011 through its testing tool called Chaos Monkey. The aim of this testing was to test infrastructure resiliency. This helps teams to check whether their existing infrastructure can scale and recover in case of any failures. The concept relies on proactive identification of issues so that outages can be avoided. This testing type is also termed *chaos engineering*, and there are a number of tools available that enable teams to introduce chaos in the system and monitor the behavior. Such an analysis helps teams to build better systems. See Figure 4-21.

![](img/f561fe6de202c361a03741b184bee8ea_77_0.png)

Figure 4-21. Chaos engineering/testing

## A/B Testing

Also known as *bucket testing* or *split-run testing*, A/B testing is a random experiment done on two variants of the application. The term *A* refers to the control or original variant, and the term *B* refers to the new variation. The version that indicates a positive business impact will be the actual winner. This enables product teams to optimize their web application or websites and increase ROI. Thus, using statistical data, teams analyze the performance of their website and define ways to optimize it based on visitor behavior. As a simplistic example, if 50 percent of visitors visit the variant and this variant derives more sales than the other variant, then variant A is the winner. See Figure 4-22.

![](img/f561fe6de202c361a03741b184bee8ea_78_0.png)

Figure 4-22. A/B testing

Ideally, product teams should practice testing at all levels right from the product inception. There are tools available that automate these testing categories, which speeds up agile development. Even in infrastructure as code, teams develop scripts, leverage coding guidelines, check for technical debt, and automate whatever comes their way to enable faster time to delivery.

## Changes in Architecture Impacting Operations

Another aspect that has led to the need for agile operations is the change in common architectures. To handle configuration changes, the infrastructure and platform world has moved to a model of idempotency. Simply put, it is an operation that produces the same result no matter how many times it is performed. In this model, tools like Puppet and Chef produced configurations created in scripts that are applied on the servers and endpoints. If any change was made to the configuration at an individual server level or endpoint that is not authorized or initiated through the configuration management tool, the tools agent would simply revert the configuration to the desired state that was assigned to that server based on the role that the server may be performing in the architecture. Thus, a web server role has a desired state configuration, and it won’t change unless the desired state configuration is modified for that role through the configuration management tool. This greatly reduces the incidents arising out of misconfigurations or configuration conflicts between development and production systems. This also brings in standardization so the operations team will know what to expect for the configuration of a particular server. Changes to configuration become simpler and automated, and incidents due to changes are drastically reduced in this model. However, one still needs to maintain the desired state configurations and keep the servers updated based on the patching, security, and compliance needs.

With the advent of containerization and microservices, the concept of idempotency has given way to the concept of immutable infrastructure. Immutable infrastructure simply means infrastructure that does not change, so you do not update or upgrade the containers but rather shut them down when there is an update and bring the other containers up. With orchestration and automation coupled with DevOps practices, you can release using a canary deployment or blue-green deployment, and this model becomes easy to use without the overhead of managing the configurations. Containers can be spun up and down in seconds, whereas virtual machines would take a few minutes to boot up. The containers are lightweight and do not carry the overhead of the entire operating system and thus enable the realization of the idea of immutable infrastructure.

Virtual machines are more like pets. Containers are more like cattle. We love our pets and name them and maintain them, and we have a long-term relationship with our pets. This is synonymous with the virtual machines and idempotent infrastructure, which needs to be maintained, kept up-to-date, and the instances kept live for a long time. While containers are more like cattle, they are ephemeral. With containerization and the concepts of agility, high availability, and scalability, the application development and deployment models are rapidly moving toward a “cattle” mindset from a “pet” mindset. A container image is just replaced with a new updated image rather than updating the older images.

Another important aspect that has implications for agile operations is that the container-based applications and architectures are easily portable across cloud environments since they are delinked from the underlying infrastructure components. An application currently deployed on AWS can be easily ported to Google Cloud or Azure without requiring modifications. Thus, configuration dependencies that existed between applications and infrastructure no longer exist in these models. Kubernetes’ excellent abstraction over the infrastructure means that infrastructure and development teams can focus on their own areas of expertise.
