
The problem has only gotten more complex in the last three years. Cloud providers have added hundreds of thousands of additional product stock-keeping units (SKUs) driven by thousands of new features. In the months preceding publication of this edition, Google Cloud and Azure offered completely new ways of making discount commitments in the form of Azure Savings Plans and Google Flexible Committed Use Discounts (Flexible CUDs), changing the game yet again.

During the last decade of our respective careers, we heard a consistent theme from practitioners and executives alike: there’s a lack of cloud value education and knowledge available. Mike heard it while running cloud cost optimization for Atlassian’s massive cloud deployments. J.R. heard it while coaching the world’s largest cloud spenders as cofounder of Cloudability’s cloud spend management platform (acquired by Apptio in 2019) from 2011 to 2020 and later as the executive director of the FinOps Foundation, as an employee of the Linux Foundation.

From this demand for a resource from which we can all learn, along with a need for someone to formally define FinOps, we joined forces in early 2019 with 26 expert practitioners from the industry to create the FinOps Foundation.

The community of now nearly 10,000 practitioners from the FinOps Foundation have provided most of the best practices we’ll cover in this book. The examples herein come from their hard-earned experience and war stories shared in the various community forums. We’ve also woven in quotes from them to help connect the content with real-life thoughts and opinions on FinOps.

## Who Should Read This Book

Anyone working in engineering, finance, procurement, product ownership, or leadership in a company running—or aspiring to run—in the public cloud will benefit from this book. As an organization understands the personas in FinOps, it can map them to relevant teams across the business.

Engineers are most likely not used to thinking about costs as a day-to-day concern. In the precloud days, they were mainly worried about performance in relation to hardware. Constrained by procurement and unable to get more servers whenever they needed them, they had to plan far in advance. They have to think about the cost of their infrastructure choices and its impact on the business. At first, this can feel foreign and at odds with their primary focus of shipping product and feature enhancements. Over time they add cost as another efficiency metric that they can tune to positively impact the business.

> *A lot of engineers will just throw hardware at a problem. FinOps requires engineers to consider the cost and margins.*
> 
> —John Stuart, VP, DevOps, Security & IT at Jobvite

*Finance* traditionally focused on retroactive reporting on a monthly or quarterly granularity, based on set budgets that quickly became out of date. The job has evolved now to help enable the business to continually move forward, and to proactively partner with tech and engineering teams to forecast spend, based on the actions of engineers (who aren't used to thinking about that cost). In other words, they're shifting from opaque and fixed capital expenditure (CapEx) reporting to transparent and fluid operational expenditure (OpEx) forecasting. As part of that role, finance teams become engineering team partners who understand what the drivers of cloud spend are amid thousands of SKUs. They help to fundamentally reinvent how to perform the finance function, while also rethinking how to report technology spend to executives and investors.

*Procurement teams* are used to tightly controlled spending, careful rate negotiations, and wielding the power of the purchase order before vendors get paid. Now procurement teams become strategic sourcing. They pull all rogue spending together into an enterprise agreement with their cloud service provider to get the best rates for what engineers are already using to deliver value.

> *We don't win by shaving cents from the cloud provider. We win by delivering features to our customers.*
> 
> —Alex Landis, Autodesk

*Product owners*, responsible for the pricing and margins of their products, struggled in the precloud world to understand all the costs that went into servicing a customer or operating a commercial offering. Cloud lets them understand more, or all, of the cost of delivering digital value, understand the cost impact of new features, and see how customer use cases create variability in the cost of an application. This allows product teams to collaborate much more effectively with engineering teams developing the products, to forecast revenue and margin more effectively, and to target customer demand much more efficiently.

*Leadership*, C-level executives, VPs, directors, or technology leaders managing budgets for a team have often lost direct control of cloud spending decisions and now rely on their teams to operate within reasonable budgets. Tech executives no longer plan large purchase decisions far in advance. Instead, they think more about how to forecast and manage spending that's already happening. The conversation has shifted from ensuring services have the capacity to ensuring that they're spending the right amount of money for the job. They want more control over how much is spent and more ability to strategically influence where it is spent.

This book seeks to break down barriers between these personas by laying out a common lexicon and set of best practices to follow.

## About This Book

In the coming chapters, we’ll formally define FinOps. The definition we’ve created was formulated by the most experienced cloud financial management teams on earth, who manage hundreds of millions of dollars (or billions in some cases) per year in cloud spend. We’ve collected their common practices for achieving cloud success along with some pitfalls they’ve identified and solved. We’ll show what effective FinOps looks like and how to start the FinOps transformation in your organization.

Previously, the only way to gain access to this knowledge would be to attend public events where these experts would present their ideas. This book and the FinOps Foundation changes that with a vibrant community, in-depth training resources, and standardized best practices.

After you’ve read this book, we encourage you to get involved with the FinOps Foundation’s various programs as a pathway to continue honing your skills and career. The mission of the foundation is to advance every individual who manages the value of the cloud. The community is there for you. We hope that the real-world strategies, processes, and stories in this book will inspire everyone to better take control of cloud spend. And in the process, we can make our organizations, and our own careers, more competitive.

## What You Need to Know Before Reading On

At the time of writing, we assume readers will have a base level of knowledge of at least one of the three main public cloud providers (Amazon Web Services [AWS], Azure, and Google Cloud Platform [GCP]). Readers should understand how the cloud works and charges for resources. They should also be familiar with the major resource types like compute and storage, and higher-level service offerings like managed databases, queues, and object storage.

A good starting place for the level of AWS knowledge needed is the AWS Business Professional training, or better, the AWS Cloud Practitioner certification. Both cover the basics of operating in AWS. For Google, check out the GCP Cloud Digital Leader course. For Azure, try the Azure Fundamentals learning path. These can usually be completed in a single-day workshop or through online training.

Readers should also understand the fundamentals of how cloud computing works; know the key services on their cloud provider, including their common use cases; and have a basic understanding of how billing and pricing work in the pay-as-you-go consumption model.

For example: as an AWS user, you should already know the difference between EC2 (Elastic Compute Cloud) and RDS (Relational Database Service). You should understand that there are different ways to pay for those resources, such as On-Demand,
