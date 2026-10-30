# Additional case studies

We would also like to look at the other three case studies available at the time of writing. We hope you now have a clear idea of how to break these case studies down and understand that the key is to locate keywords that help us map to GCP services. We will not go into so much depth for the other cases, but we hope you will begin to form solutions without guidance as you read through the case studies.

# EHR Healthcare

We will now look at the EHR Healthcare case study. Again, we will review what can be gleaned from the online documentation. At the end of the case study, we will provide an analysis. We would like you to write down the services that jump out to you as you are reading and compare your findings to our analysis:

# Company overview:

> "EHR Healthcare is a leading provider of electronic health record software to the medical industry. EHR Healthcare provides their software as a service to multi-national medical offices, hospitals, and insurance providers."

# Solution concept:

> "Due to rapid changes in the healthcare and insurance industry, EHR Healthcare's business has been growing exponentially year over year. They need to be able to scale their environment, adapt their disaster recovery plan, and roll out new continuous deployment capabilities to update their software at a fast pace. Google Cloud has been chosen to replace their current colocation facilities."

# Existing technical environment:

> "EHR's software is currently hosted in multiple colocation facilities. The lease on one of the data centers is about to expire."

> Customer-facing applications are web-based, and many have recently been containerized to run on a group of Kubernetes clusters. Data is stored in a mixture of relational and NoSQL databases (MySQL, MS SQL Server, Redis, and MongoDB).

> EHR is hosting several legacy file- and API-based integrations with insurance providers on-premises. These systems are scheduled to be replaced over the next several years. There is no plan to upgrade or move these systems at the current time.