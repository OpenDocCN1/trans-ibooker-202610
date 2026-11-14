![](assets/cover.png)

![](assets/cover.png)

# Security as Code

by BK  Sarthak  Das and Virginia  Chu

Copyright © 2023 Virginia Chu and BK Sarthak Das. All rights reserved.

Printed in the United States of America.

Published by O’Reilly Media, Inc., 1005 Gravenstein Highway North, Sebastopol, CA 95472.

O’Reilly books may be purchased for educational, business, or sales promotional use. Online editions are also available for most titles ([http://oreilly.com](http://oreilly.com)). For more information, contact our corporate/institutional sales department: 800-998-9938 or *corporate@oreilly.com*.

*   Acquisitions Editor: Jennifer Pollock
*   Development Editor: Sarah Grey
*   Production Editor: Clare Laylock
*   Interior Designer: David Futato
*   Cover Designer: Karen Montgomery
*   Illustrator: Kate Dullea

*   February 2023: First Edition

Add additional revdate spans below as needed. 

# Revision History for the Early Release

*   2022-06-06: First Release
*   2022-09-20: Second Release
*   2022-10-14: Third Release

See [http://oreilly.com/catalog/errata.csp?isbn=9781098127466](http://oreilly.com/catalog/errata.csp?isbn=9781098127466) for release details.

The O’Reilly logo is a registered trademark of O’Reilly Media, Inc. *Security as Code*, the cover image, and related trade dress are trademarks of O’Reilly Media, Inc.

The views expressed in this work are those of the author(s) and do not represent the publisher’s views. While the publisher and the author(s) have used good faith efforts to ensure that the information and instructions contained in this work are accurate, the publisher and the author(s) disclaim all responsibility for errors or omissions, including without limitation responsibility for damages resulting from the use of or reliance on this work. Use of the information and instructions contained in this work is at your own risk. If any code samples or other technology this work contains or describes is subject to open source licenses or the intellectual property rights of others, it is your responsibility to ensure that your use thereof complies with such licenses and/or rights. PROD: Uncomment the following sentence if appropriate and add it to the above para: This book is not intended as \[legal/medical/financial; use the appropriate reference\] advice. Please consult a qualified professional if you require \[legal/medical/financial\] advice.

This work is part of a collaboration between O’Reilly and NGINX. See our [statement of editorial independence](https://oreil.ly/editorial-independence).

978-1-098-12740-4

p class='printer'>\[FILL IN\]</p 

# Chapter 1. Introduction to DevSecOps

We did not start our careers in security, but we both know that delivering software is of utmost importance to developers. “Delivering software” in this context means delivering *something that does what it is supposed to do*. This could refer to the stability of code, or the software meeting the functional requirements (for example, a calculator can add, multiply, subtract, and divide numbers) and performance expectations without any issues (for example, a chat application allows more than 10 people to send messages to each other simultaneously). 

Building *quality* software, however, requires good coding practices, resilient architectures, and security. It’s common for security to be added into the software after it has been built, but the [*shift-left* approach](https://www.dynatrace.com/news/blog/what-is-shift-left-and-what-is-shift-right/#:~:text=and%20reliability%20criteria.-,What%20does%20shift%2Dleft%20mean%20in%20DevOps%3F,side%20of%20the%20DevOps%20lifecycle.) recommends moving security to much earlier in the development life cycle, building it in from the start. We will discuss that approach in this chapter, focusing on *Security as Code*. In this approach, our infrastructure’s security policies–detection, prevention, remediation–are all defined as code.

We will also discuss DevSecOps in this chapter, focusing on the three major players in software development: 

Development
: 

Is your code doing what it is meant to do?

Operations
: 

What is this code running on? Do you have skills/time to maintain this going forward? Can the provisioned infrastructure handle the expected workload?

Testing
: 

Can the code survive unexpected use cases? How does the code respond to something you didn’t account for? 

The primary focus of this book is how to integrate security into your development process through cloud infrastructure. Declaring infrastructure in files is also known as *Infrastructure as Code* (IaC). Kief Morris provides a helpful definition in his book [*Infrastructure as Code*](https://learning.oreilly.com/library/view/infrastructure-as-code/9781098114664/ch01.xhtml#iron-age-cloud-age), 2nd edition (O’Reilly):

“Infrastructure as Code is an approach to infrastructure automation based on practices from software development. It emphasizes consistent, repeatable routines for provisioning and changing systems and their configuration. You make changes to code, then use automation to test and apply those changes to your systems.”

Using IaC and declaring the security aspects of that infrastructure is Security as Code (SaC). SaC is not entirely different from IaC, but rather focuses on enabling security controls using templates.

This chapter introduces the basic concepts of SaC, and Chapter 2 provides setup and instructions to get you started. After that, the chapters are organized by security domains. Some chapters are exclusive to a single domain; others address multiple domains in a single stage of the buildout. Each domain has its own unique set of questions. For instance: 

Data Protection (Chapter 3)
: 

Is everything encrypted? Does the encryption approach follow data classification guidelines? Do I have data classification?

Infrastructure Security (Chapter 3)
: 

As we are running our application in the cloud, is all the infrastructure deployed securely? Is my S3 bucket publicly accessible when it should not be? Can I prevent deployment of my resources when something is missing?

Application Security (Chapter 3)
: 

Is the code I’m running secure? How many active vulnerabilities are there in my code? Did we release code with a critical vulnerability?

Logging and Monitoring (Chapter 4)
: 

Do you know what to monitor? When does an observation become an anomaly? Are the right application security indicators in place to inform us of any mishap?

Identity and Access Management (Chapter 5)
: 

Who has access to what? Does anyone have any elevated privileges? Can someone elevate their own or others’ privileges? 

Incident Response (Chapter 6)
: 

How do we react to incidents? Can I replace part of the offending application when something goes wrong?

Throughout the chapters, we will use Amazon Web Services (AWS)-native security services and best practices to baseline the environment we are deploying. When we assume certain operational and team structures, we explicitly call out those assumptions. 

# Before DevOps: The Software Development Life Cycle

When you are new in your career, writing a piece of code that does exactly what was asked is a joy of its own. With time, however, we’ve realized writing quality code does not stop at making 2 + 2 = 4.  What if the user enters “2 + a”? How should your software behave? 

Well, that’s the responsibility of the Quality and Testing engineers, isn’t it? Wrong.

We’ve seen back-and-forth between developers and testers that was time-consuming and created unhealthy expectations from both teams. Let’s take the example of the calculator input of “2 + a”. If, as a coder, you did not think of this use case because it was not part of the requirement, and your tester or QA team did not record it as a test case, you would be shipping broken code to your customers. Now this broken code is your final product. A codebase that doesn’t work as expected is not a joy for the end user to work with.

Code needs to be hosted on some infrastructure to be built and deployed. Is it going to run on a server in your datacenter, on a virtual machine in the cloud, on a container, or on someone’s laptop? Depending on your answer, you have another set of responsibilities related to infrastructure. Once you set up your infrastructure, you will need to answer a new set of questions, including: 

*   Are the coders developing this on the exact same platform on which it will be deployed?
    
*   Who is setting up all the infrastructure?
    
*   Does the infrastructure fail-open or fail-safe, in the case of an error?
    

That set of questions needs another set of tests to make sure the application code is being run correctly on the right platform, and that the platform is not misbehaving. 

In traditional software development, only *developers* take care of development, which means their prime directive is writing code up to a specification. The *operations* team handles the environment and method of deployment, and the change management procedures. *Testers* take the output of the developers and the operations team and make sure the near-final product does not break. These three roles are not mutually exclusive, but each team needs to wait for input from the previous team to start their work. A very common representation of this flow is the Software Development Life Cycle (SDLC) model (see [Figure 1-1](#ch01.xhtml#fig_1_software_development_life_cycle_waterfall_model)). 

In practice, the waterfall model of the SDLC might operate something like this: Developers create code quickly, based on functional requirements. The code is sent for testing, errors are found, and code is sent back to developers. The developers fix the code and send it for another round of testing. Once the testing is complete, the code is handed off to the operations team for maintenance. 

![](assets/introduction_to_devsecops_106521_01.png)

Siloed teams operating in a hands-off style, similar to the SDLC in [Figure 1-1](#ch01.xhtml#fig_1_software_development_life_cycle_waterfall_model), have their disadvantages. Each team has its own toolsets and handles a very specific piece of the SDLC, and is typically unaware of the toolsets of the other teams. This makes it difficult to get quality software out the door on time. 

The waterfall model leads to a lot of back-and-forth between teams, as we’ve noted. The back-and-forth is made worse when you have not delivered anything because your code has not passed your testing teams. So, a whole lot of time and effort is lost without producing any tangible outcomes.

Enter DevOps. In order to reduce time-to-market and improve the quality of software, the concept of DevOps was introduced. In their book [*Effective DevOps*](https://learning.oreilly.com/library/view/effective-devops/9781491926291/ch02.xhtml#idm140536841630480) (O’Reilly), authors Jennifer Davis and Ryn Daniels define DevOps as: 

“A cultural movement that changes how individuals think about their work, values the diversity of work done, supports intentional processes that accelerate the rate by which businesses realize value, and measures the effect of social and technical change. It is a way of thinking and a way of working that enables individuals and organizations to develop and maintain sustainable work practices. It is a cultural framework for sharing stories and developing empathy, enabling people and teams to practice their crafts in effective and lasting ways.”

In a DevOps model, the development, testing, and operations teams don’t work in silos, but are the same group. If we were to visualize the DevOps model, it would look like a homogenous blob of roles. Kief Morris writes in *Infrastructure as Code* that the goal of DevOps is:

“to reduce barriers and friction between organizational silos—development, operations, and other stakeholders involved in planning, building, and running software. Although technology is the most visible, and in some ways simplest face of DevOps, it’s culture, people, and processes that have the most impact on flow and effectiveness. Technology and engineering practices like Infrastructure as Code should be used to support efforts to bridge gaps and improve collaboration.”

We want to emphasize this point: *DevOps is not solely enabled by technology. It is effective only when people, process, and technology are working together.* There is a common misconception that if you use tools that are used in CI/CD systems, you’re automatically practicing DevOps. This is flawed thinking. What enables DevOps is collaboration. 

# What Is DevSecOps?

DevSecOps is not a “new version” of DevOps, but rather a conscious effort to add security into your DevOps framework. Like DevOps, there are numerous definitions and approaches to DevSecOps. For the purposes of this book, we define *DevSecOps* as the ability to implement and deploy SaC in software. 

We will be leaning heavily on APIs, cloud services, and other open source projects to implement SaC, much like IaC. When a part of your SDLC becomes “as-code”, your team should have the openness to build things. 

As your organization begins to implement DevSecOps, there are two important things your team will need to outline: tools and security guidelines. 

First, how will you write your IaC? In other words, what tools will you use? This could be a tool like AWS CloudFormation or Terraform. There are numerous services and products available from vendors and the open source community that you can use to build and integrate SaC into your pipeline. As we mentioned, this book will use AWS and open source projects to demonstrate the *why* and *how* of doing DevSecOps, instead of fixating on a particular tool’s licensing or procurement. We chose to focus on AWS in this book since it is currently the most popular cloud infrastructure vendor, with [33% of the market](https://www.statista.com/statistics/967365/worldwide-cloud-infrastructure-services-market-share-vendor/#:~:text=In%20the%20first%20quarter%20of,with%20eight%20percent%20market%20share.). However, the book’s underlying principles will be useful to all readers. 

Second, what are your company’s security “rules of the road?” What has your security team designated as “definitely don’t do this” rules?  Understanding why the security team provides certain guidance helps you understand the concern underlying the rule. 

In DevSecOps, you are building security directly into your software development pipeline (see [Figure 1-2](#ch01.xhtml#fig_2_building_security_into_a_devops_pipeline)). In step 1, the developer lints their code locally and makes sure its formatting follows the team’s conventions and standards. They then commit the code to the repository. In step 2, the build system of the pipeline scans for errors and any other vulnerabilities and misconfigurations. The security of the pipeline is built into this stage. Finally, in step 3, the code is (possibly) ready for deployment. The last hurdle is a *decision gate*–a mechanism that checks for errors. If any errors are found, deployment is canceled and developers are informed. If the code has no errors, the deployment goes through.

![](assets/introduction_to_devsecops_106521_02.png)

Steps 1 and 2 happen every time. The top version of step 3 only occurs when the linters and scanners discover no errors. The lower version of step 3, and step 4, only occur when the linters and scanners discover an issue.

The code that is deployed is not the only thing that needs to be secured. You also need to protect the security checks you are implementing within the DevOps pipeline. Imagine for a moment that anyone who commits code can turn security checks on and off. Would it be secure?  

We have seen teams bypass their security checks because an “urgent” code change was needed and it was faster to deploy without security. That is a recipe for disaster. Situations where a real emergency justifies a non-secured code change should be recorded and remediated. There should not be a bypass function for deployments.

# Introducing Automatoonz

Security is a broad topic. Its subfields include physical security, application security, cryptography, training, and much more. We will limit our scope in this book to a subset of security domains, which we’ll explore by following the journey of an imaginary company we’ll call Automatoonz. 

Automatoonz is an animation company that’s recently been getting into web toons and series creation. The company operates on AWS and is currently trying to build security into their development and deployment processes. They want to be Lean and secure, but the company is relatively new in the space and doesn’t have the funding to hire more security engineers. Thus, they want to automate as much security as possible.  

Because Automatoonz operates on AWS, there are some security responsibilities they don’t have to worry about, such as physical security. AWS refers to this as the [Shared Responsibility Model. ](https://aws.amazon.com/compliance/shared-responsibility-model/)

# Cloud Infrastructure: Secure by Default

In cloud infrastructure, every resource is created through an API call. Each API call has parameters that configure your resource exactly as you want it. The Software Development Kit (SDK) you use will also automatically generate a default configuration, to provide maximum customization options.

For example, if you are using the AWS SDK for Python (Boto3) to create an Amazon S3 bucket, the only required parameter is a name for the bucket. The SDK does not ask you for encryption, nor does it require you to ensure the bucket is not publicly accessible. If you want to enforce encryption on the contents of the bucket, you should do so upon deployment. This is what is referred to as Secure by Default.  

Secure by Default does not mean you should lock down everything you possibly can. It’s often said that the safest computer system is the one that’s unplugged. But, if you make your system so secure that it’s practically unusable–well, people won’t use it. They’ll look for workarounds, potentially compromising security in the process. In this book, we aim to demonstrate *usable security*–a balance between usability and security.

In an IaC paradigm, though, you can *templatize* your resources–make declarative code for patterns you come across and create templates that developers can work from, in order to keep the codebase consistent. For example, if you are building a web server within AWS, you should be able to standardize the architecture for future deployments. If the architecture is defined only in a diagram, you’ll need to codify that diagram as IaC. To do so, you’ll need to create declarative templates so that the same web server pattern can be deployed repeatedly with zero deviation.

What does templatization provide in terms of security? Let’s revisit the S3 bucket example. At Automatoonz, the legal team is tasked with ensuring that all of the company’s data is *encrypted-at-rest*, meaning that data stored in any persistent storage has to be encrypted through a cryptographic key, which is only accessible to authorized users. In cloud computing, you can define encryption-at-rest through API flags in your IaC templates. This also helps standardize a Secure by Default posture for your infrastructure. 

Of course, the legal team informs the developers of this requirement, and the developers need to apply the right checks and balances to adhere to the policy. If there is an IaC template for the bucket, the developers must check it before it is sent to AWS endpoints for resource creation. This will prevent the unencrypted bucket from ever being created. 

At a very high level, [Figure 1-3](#ch01.xhtml#fig_3_this_decision_gate_rejects_unencrypted_templates) shows what we are trying to achieve. The developer on the left creates an AWS CloudFormation template, which in turn creates an S3 bucket. This goes to a decision gate. If encryption is not enabled, the decision gate shuts down deployment. Only after the template is fixed can the deployment go through.

![](assets/introduction_to_devsecops_106521_03.png)

# Move Fast, Secure Fast: The Importance of Automation

When you drive a car, there are certain rules of the road that you must obey. Not adhering to these rules can lead to mishaps, serious accidents, and legal trouble. In security, we have similar invariable rules. Sometimes the rules are driven by regulatory or compliance requirements; other times they’re intended to create engineering excellence. Regardless of the reason, every team is expected to follow these rules.

How can developers ensure they consistently follow rules related to security? Recall our earlier discussion of IaC, and its ability to define cloud resources in a declarative manner. Herein lies our answer. If we can set predictable flags and/or parameters for cloud resources, we can allow operations such as creating new resources or deploying new code to go through *only* if certain things are present.

When you want to move at the speed of the cloud, you want to secure things at the speed of the cloud as well. This means automating as much of your security as possible. There is a manual way of doing the check shown in [Figure 1-3](#ch01.xhtml#fig_3_this_decision_gate_rejects_unencrypted_templates), in which the developer looks at their code and prevents a misconfiguration. But, remember the old adage “Humans are the weakest link in security?” You ideally want to automate these checks. To automate a security check–a central idea in DevSecOps–there are certain attributes to consider:

Idempotence
: 

The security check should yield the same results no matter how many times you pass the template in question.

Baseline
: 

The security check should apply to all or most resources. If a security check only applies to one variant of a resource, which appears in less than 30% of your deployments, you will get a lot of noise and failures while doing the checks.

Recommendation
: 

You found a security misconfiguration. That’s cool, but did you tell the build system what to do? If you leave your end users without a fix, you are adding to their work. Your scans should recommend fixes when a misconfiguration is found.

We will be using these attributes as we build our pipeline throughout the book. One thing we want to reiterate: Automation does NOT solve everything. Without the right owners and processes, all the automation in the world will not amount to anything. These security checks need to be maintained, and the developer should give feedback to improve efficiency.

# DevSecOps Culture 

Technology is not a silver bullet. Culture plays a role, too. DevOps at its core is built on a culture, and its tools are only useful when people use them in the right ways. 

We have seen organizations acquire every possible tool that claims to solve security, yet not even configure half of them. Why aren’t the teams using these security tools? Do they lack training? Are the tools overkill, or too expensive to roll out to the entire organization? 

We highly recommend answering one simple question before you buy any security tools:  *What security problem are you trying to solve?* Answers like “make things secure” and “improve my security” are as vague as it gets. Try to boil it down to a single sentence. If that doesn’t give you a clear answer, consider writing a risk statement. A risk statement goes something like this:

“Lack of X leads to loss of Y because of Z.”

X = a security function, like access control or logging 

Y = the impact if your security control is not implemented (i.e., what is at stake?)

Z = how your security control prevents a security event

We won’t go into detail on procurement, but it is crucial to understand what security problem your tool is meant to solve before buying it. A lot of enterprise-grade tools offer free trials or demos. If your team has a builder mindset, you may want to explore the vast expanse of open source cloud security tools, many of which have made their mark in the industry and continue to get better.

Once you’ve selected a tool, who will maintain it? Will it need to be updated regularly? Your tool should have a clear owner on the operational side. Let’s say you get a Cloud Security Posture Management tool, and the security team has integrated every cloud account to be ingested. The security team receives alerts from the accounts, but takes no action because they expect the cloud account owner to act on those alerts. But, the cloud account owner thinks the *security* team is taking care of it. Without a clear process for addressing the alerts, the teams have no plan of action. Simple tools like responsibility assignment matrices (or, RACI charts) and runbooks can take care of this. We discuss these tools in more detail in Chapter 7. 

# Summary

Now that we have introduced DevSecOps and clarified the scope of the book, let’s start implementing DevSecOps into our pipeline!

The subsequent chapters will begin by setting up the infrastructure for Automatoonz and iteratively building security capabilities into the application development process. We will primarily focus on the technology and tooling, but we will call out any people and process must-haves, derived from our experience. 

A quick recap of key takeaways from this chapter:

*   DevSecOps is a variant of DevOps that includes security in its iterative DevOps model.
    
*   If you can prevent misconfigured resources from being created, you can create security hygiene and educate developers.
    
*   Automating security checks requires idempotence, baseline, and recommendation. Chapters 3 and 4 address this topic in more detail.
    
*   Automating security checks alone will not solve security problems. Establish people and processes to own and operate the automation.
    
*   Establish *why* you’re implementing a security tool before rolling it out.
    

# Chapter 2. Setting Up Your Environment

Now that you’re excited and engaged, you’re ready to experiment and create something in your AWS account. We’ll use free, publicly accessible sample code to demonstrate how you can create or destroy objects or resources with the “magic” of automation.

In this chapter, we’ll set up your local environment with some simple commands. As we discussed in Chapter 1, AWS will be the cloud provider of choice for our Automatoonz adventure in this book. 

When you are finished running the below commands, you will have a fully automated Amazon Elastic Kubernetes Service (EKS) cluster and an AWS Lambda container image pipeline on which to deploy a simple Python application.

###### Tip

Remember to set up and test your account credentials, and test your access. Also, don’t forget to destroy the sample resources you created in your account.

# What You’ll Need

Below is a list of tools you’ll need to follow along with the exercises and activities in this book:

AWS CLI 
: 

An open source tool that enables you to interact with AWS services in the assigned account by using commands in your terminal. For more information, visit the [official site](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.xhtml). Please use the latest version.

AWS CloudFormation
: 

An AWS domain-specific language (DSL) that supports two file types: YAML and JSON. The files are used to deploy and provision infrastructure resources tracked as code. For more information, visit the [official site](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.xhtml).  

Docker (community edition)
: 

A software platform, or platform as a service (PaaS), that bundles software into units called containers. These containers allow you to build, test, and deploy applications. For more information, visit the [official site](https://www.docker.com/). Please use the latest version.

Open Container Initiative (OCI) images
: 

A Linux Foundation project where open industry standards around container formats and runtime created an official certification program. In 2015, Docker, CoreOS, and other leaders in the container industry launched OCI. For more information, visit the [official site](https://opencontainers.org/about/overview/).

Python
: 

An open source, interpreted, object-oriented, high-level programming language with dynamic language with dynamic semantics. For more information, visit the [official site](https://www.python.org/). Please use version 3.X or higher. 

Git repository
: 

An open source, distributed version control system for different file types. For more information, visit the [official site](https://git-scm.com/). Please use the latest version.

Kubectl
: 

An open source Kubernetes command-line tool that allows users to interact with Kubernetes clusters. For more information, visit the [official site](https://kubernetes.io/docs/tasks/tools/). Please use the latest version. 

Kubernetes
: 

An open source system for automating deployment, scaling, and management of containerized applications. For more information, visit the [official site](https://kubernetes.io/). Please use version 1.21 or higher.

# Installing and Verifying Your Setup

You can use package managers to download the latest versions of tools, or curl to directly download the installation package. Then, extract the installer file and run the install script. This section provides commands for the Darwin, Linux, and Windows operating systems.

## Installing the AWS CLI

Run the appropriate commands to install the AWS CLI, which allows you to interact with your AWS account and manage IaC. (See the official AWS [documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.xhtml) for more details.)

Darwin:

```
 > brew install awscli
```

Linux:

```
 > curl "https://awscli.amazonaws.com/awscli-exe-linux-<CPU_architecture>.zip" -o "awscliv2.zip"
> unzip awscliv2.zip
> sudo ./aws/install
```

Windows:

```
 > choco install awscli
```

After the installation, complete the configuration to associate your AWS account (see [documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.xhtml)). This lets you run the sample code provided in the [GitHub repository](https://github.com/bksarthak/devsecops-book) that accompanies this book. 

## Installing the Docker Engine

Run the appropriate commands to install the Docker engine ([documentation](https://docs.docker.com/engine/install/centos/)). The Docker CLI allows you to interact with your Dockerfile.

Darwin:

```
 > brew install docker
```

Linux:

```
 > curl -fsSL https://get.docker.com -o get-docker.sh
> sudo sh get-docker.sh
```

Windows:

```
 > choco install docker-engine
```

After the installation is completed, test for a response from the Docker CLI.

## Checking your Python Version

Run the appropriate commands to check if Python version 3.X or higher has been installed. Python will be used in the AWS Lambda container in the sample code.

Darwin:

```
 > python3 --version
```

Linux:

```
 > python3 --version
```

Windows:

```
 > python --version
```

If Python 3.X is not installed, visit the [official site](https://www.python.org/downloads/) for more information.

## Installing Git

Run the appropriate commands to install Git, the distributed version control system. The Git CLI allows you to interact with Git repositories.

Darwin:

```
 > brew install git
```

Linux:

```
 > sudo yum install git -y
```

Windows:

```
 > choco install git
```

After installation, complete the configuration to associate your personal settings. Then, run a command to test for a response from the Git CLI. For more information or troubleshooting, visit the [official site](https://git-scm.com/doc).

## Installing Kubernetes

Run the appropriate commands to install the Kubernetes CLI, which will allow you to interact with Git repositories.

Darwin:

```
 >> brew install kubectl
```

Linux:

```
 > curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
> sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

Windows:

```
 > choco install kubernetes-cli
```

After the installation is complete, test for a response from the Kubernetes CLI. For more information or troubleshooting, visit the [official site](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-kubectl-binary-with-curl-on-linux).

Now that you’ve installed and verified your tools, it’s time to create a pipeline.

# Creating Your First Bare-Bones Pipeline

Begin creating your pipeline by downloading the latest version of the sample code:

```
 > git clone <BOOK_SAMPLE_CODE_REPO_URL>
```

This command will create an AWS Service Catalog portfolio to hold all the AWS Service Catalog infrastructure and cluster products:

```
 > aws cloudformation create-stack \
        --template-body  file://ServiceCatalogPortfolio.yml  \
        --stack-name EKSPortfolio \
        --parameters file://parameter-files/ServiceCatalog.json \
        --capabilities CAPABILITY_NAMED_IAM \
        --region <INPUT_YOUR_REGION>
        
>Add s3 bucket creation with KMS key
```

Next, upload all the AWS Service Catalog files and the AWS Lambda function code:

```
> aws s3 cp service_catalog_products/ \
s3://<s3_bucket_for_sc_products_lambda_function>/ \
--recursive \
--exclude "*" \
--include "*.yml" \
--include "*.zip" \
-–region <INPUT_YOUR_REGION>
```

Then, provision the AWS Service Catalog networking resources:

```
> aws cloudformation create-stack \
--template-body file://ServiceCatalogProduct.yml \
--stack-name EKSNetwork \
--parameters file://parameter-files/NetworkProduct.json \
--region <INPUT_YOUR_REGION>
```

The following command will provision the AWS Identity Access Management (IAM) product role resource, which integrates with the AWS Service Catalog:

```
 > aws cloudformation create-stack \
       --template-body file://ServiceCatalogProduct.yml \
       --stack-name EKSIAM \
       --parameters file://parameter-files/IAMProduct.json \
       --region <INPUT_YOUR_REGION>
```

Provision the AWS Service Catalog EKS Nodegroup product resource:

```
 > aws cloudformation create-stack \
--template-body file://ServiceCatalogProduct.yml \
--stack-name EKSIAM \
--parameters file://parameter-files/IAMProduct.json \
--region <INPUT_YOUR_REGION>
```

Provision the AWS Service Catalog EKS Lambda product resource:

```
> aws cloudformation create-stack \
--template-body file://ServiceCatalogProduct.yml \
--stack-name EKSLambda \
--parameters file://parameter-files/EKSLambdaProduct.json \
--region <INPUT_YOUR_REGION>
```

Provision the AWS Service Catalog EKS Virtual Private Cloud (VPC) endpoint product resource:

```
> aws cloudformation create-stack \
--template-body file://ServiceCatalogProduct.yml \
--stack-name EKSVPCENDP \
--parameters file://parameter-files/VPCEndpointProduct.json \
--region <INPUT_YOUR_REGION>
```

Provision the AWS Service Catalog EKS control plane logging product resource:

```
 > aws cloudformation create-stack \
--template-body file://ServiceCatalogProduct.yml \
--stack-name EKSLOG \
--parameters file://parameter-files/EKSLoggingProduct.json \
--region <INPUT_YOUR_REGION>
```

# Summary

[Figure 2-1](#ch02.xhtml#fig_1_infrastructure_as_code_pipeline) diagrams the environment you are deploying in your AWS account. 

![](assets/image-to-come.png)

Let’s look at the pieces of this pipeline, corresponding to the numbers in [Figure 2-1](#ch02.xhtml#fig_1_infrastructure_as_code_pipeline):

1.  Your laptop or terminal to upload and download code
    
2.  The AWS Service Catalog manages logical products and modularized code
    
3.  AWS IAM role to allow access the the EKS cluster
    
4.  Amazon CloudWatch for logging and metrics
    
5.  AWS Lambda container images and Amazon ECR
    
6.  The EKS managed cluster
    
7.  The Python application deployed in the the EKS cluster
    

Take a few minutes to look around and get familiar with your environment. Now that you’ve set up your infrastructure, you’re ready to begin securing it. Chapter 3 will get you started.

  

# Chapter 3. Securing Your Infrastructure

You likely already know that cloud infrastructure can be spun up and down considerably faster than traditional datacenter infrastructure. Cloud providers use declarative languages to help end users configure the infrastructure they spin up. For example, AWS uses CloudFormation. 

In this book, for the sake of simplicity, we’ll be using CloudFormation code. However, you should know that there are other IaC tools–such as Cloud Development Kit (CDK) and Hashicorp’s Terraform–that use different writing and deployment formats, but achieve the same goal of creating infrastructure through code. 

# What Makes Infrastructure Secure?

What does it mean to “secure” your infrastructure? IaC is code, and just like you secure your application’s code, you need to plan to secure your IaC. The top three priorities involved in securing IaC are, in order of importance:

1.  Preventing unwanted access to your code 
    
2.  Detecting misconfigurations in the infrastructure to be deployed 
    
3.  Preventing misconfigured code from being deployed
    

In this chapter, we’ll explore all three priorities in detail.

###### Note

We are assuming that you have an intermediate understanding of how to use Git-based version control and will not be covering Git troubleshooting or branching strategy. For more on those topics, we recommend reading [*Head First Git*](https://learning.oreilly.com/library/view/head-first-git/9781492092506/) by Raju Gandhi (O’Reilly). 

We also delve into topics, like identity and access management (IAM). We recommend reading the [AWS documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.xhtml) to understand IAM, if you are not familiar with it.

# Hands Off! Preventing Unwanted Access With IAM Permissions

Always store your IaC files in a remote, versioned repository as you integrate changes. AWS has its own native Git-based version control system, known as CodeCommit, which we’ll be using throughout this book. Now, how do you secure this repository from unwanted access? 

*Identity and access management* (IAM) is the core method of assigning permissions to resources in AWS accounts. Anyone who needs to create, update, or delete files within your CodeCommit repository should–at the bare minimum–have the permissions laid out in the IAM policy. This policy allows CodeCommit actions to be taken in your specific repository only if it is in the correct region: in this case, as shown below, only end users in the us-east-2 region can perform actions. 

The second statement below allows you to list all CodeCommit repositories. We have separated the two statements because you cannot use `ListRepositories` for a specific resource.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "codecommit:*",
            "Resource": "arn:aws:codecommit:us-east-2:111111111111:*",
            "Condition": {
                "StringEquals": {
                    "aws:RequestedRegion": "us-east-2"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": "codecommit:ListRepositories",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "aws:RequestedRegion": "us-east-2"
                }
            }
        }
    ]
}
```

This IAM policy is a starting point for working with CodeCommit repositories. You can optionally pare these permissions down if you want to limit a user’s actions to a specific repository. You can see an example in the book’s [supplemental materials](https://github.com/bksarthak/devsecops-book/tree/main/chapter-3) on GitHub.

# Detecting Misconfigurations

The next step in securing your IaC is to detect any misconfigurations written into it. To explain more about misconfigurations, let’s shift our focus back to our fictional company, Automatoonz. 

Automatoonz is working to become a fast-moving, developer-focused organization in which security is baked into the development lifecycle. Since security has often been treated as an afterthought within their organization, prioritizing it will help front-load initial security patterns and work, allowing the development team to scale with less friction in the future.

The developers are planning to use Amazon [EC2 instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.xhtml) to deploy and host an application to be used by customers. Lupe, from the Automatoonz security team, asks the developer team to make sure the volumes attached to the EC2 instances are encrypted. However, Jake, a newly hired DevSecOps engineer, accidentally forgets to encrypt one of the volumes. This mistake is identified during an internal audit, and the developer team rushes to fix it. 

Afterward, Jake, Lupe, and the developer and security teams want to make sure this mistake never happens again. Rather than depending on human consistency, the teams agree to use automation to find errors in the IaC. They need to decide on some “rules of the road” to ensure that their automations meet compliance standards and pass audits. 

## Identifying a Standard

Both teams want to find a baseline standard to secure the IaC code, ideally one they can implement quickly. After some research, they find the [Center for Internet Security (CIS)](https://www.cisecurity.org/), a cloud-provider-agnostic consortium that provides baseline configurations for different scenarios through its [CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks/) resource. CIS’s configurations are considered the industry standard and are updated regularly. The Automatoonz teams decide to use CIS Benchmarks and note that they’ll need to track updates to keep up with any changes. Now they need to decide which level of hardening they want as the baseline for their environment. 

## Threat Modeling

The CIS Benchmarks are a good starting point, but they don’t account for all security considerations. In order to create a custom security action plan, Lupe suggests developing a threat model of the application. 

*Threat models* are a design tool that help teams enumerate the possible vulnerabilities in any given application. There are many ways to build a threat model. One of the more popular frameworks is [STRIDE](https://docs.microsoft.com/en-us/azure/security/develop/threat-modeling-tool-threats), a threat modeling tool that helps identify security risks early in the development cycle. STRIDE includes six categories of security risks, shown in Table 3-1.

Table 3-1. STRIDE’s security risk categories

 
| STRIDE category || Definition || What does it mean for Automatoonz? |
| --- | --- | --- |
| Spoofing || Pretending to be someone you are not by using else’s credentials || Ability to assume the role of CodePipeline to deploy the CloudFormation stack without checks |
| --- | --- | --- |
| Tampering || Unauthorized changes made to the system (either infrastructure or application) || Direct changes to the application and infrastructure is done without going through the CodePipeline |
| Repudiation || Users denying that they have made changes without other users being able to disprove them || Any changes to the infrastructure cannot be traced back to a log entry |
| Information disclosure || Exposure of information to users who should not have access to the information || Permissions granted to the end users provide visibility into the underlying infrastructure and logs |
| Denial of service || Valid users are not allowed to carry out tasks  || Developers are not able to commit their code to CodeCommit  |
| Elevation of privilege || Unprivileged user gains access to privileged operations || Users can deploy and spin up resources without going through the CodePipeline checks |

While Table 3-1 is not an exhaustive list of possible threats, they are the ones we’ll worry about for our purposes in this chapter. In the next section, we’ll focus on various security controls that can protect Automatoonz from these security risks.

## Security Controls

There are three types of security controls in AWS, as shown in [Figure 3-1](#ch03.xhtml#fig_1__different_types_of_security_controls): preventive, corrective, and detective. AWS offers misconfiguration detection services like SecurityHub, GuardDuty and Config. While these are valuable, they are all *detective security controls*: they only detect misconfigurations *after* a resource is created. 

It’s also important to have *preventive controls* to *prevent* the developers from launching misconfigured resources. Time is the bad actor’s best friend: The longer a misconfigured resource is up, the more time they have to poke around the account to find weak points, run different attacks, and gain access to sensitive data. 

The third kind of security control is *corrective controls*, which fix a configuration to bring the system back to its known good state. Developers should aim to have all three controls in place to secure their IaC. In the context of Automatoonz, we will focus on implementing preventive controls and automation first, then detective.

![](assets/securing_your_infrastructure_046357_01.png)

Our design goal is to automatically trigger deployments when IaC is pushed to the CodeCommit repository. The workflow will begin with Automatoonz developers committing their code. A commit invokes a build stage, which only deploys into the VPC if the code is up to security standards and considered safe. In the build stage, the security standards and expectations are codified in the form of checks. If the code passes these checks, it is deployed into the environment.

The Automatoonz developer team (Aisha and Dave) already has a VPC and associated subnets, and have continued to build security groups and other resources to meet their needs. However, what they build is not always secure, which has led to some uncomfortable conversations with the security team. 

The developers are using an existing CodeCommit repository to store their code. This will be the starting point for their DevSecOps journey. Having versioned IaC helps the team collaborate on code, roll back deployments, and integrate with testing functions. The developers can integrate their unit tests in the pipeline (which we’ll demonstrate later in this chapter and throughout the book) to enhance automation and debugging. 

However, it emerges that *someone* has created a security group that has port 22 access, open to the internet. This is a problem: Bad actors could find this port and use it as an attack vector to brute-force their way into Automatoonz’s AWS VPC network. What should the teams do?

Ideally, they should monitor the main branch, since the code there has been merged after human approval and/or reviews. When someone commits code to the CodeCommit repository, a pipeline is executed. As part of this pipeline, the team can select the exact branch they want to monitor for changes. Let’s create the pipeline to do that. 

# Better Than a Cure: Implementing Preventive Controls

A *pipeline* (as [defined by RedHat](https://www.redhat.com/en/topics/devops/what-cicd-pipeline)) is a “set of steps which must be performed to create a new version of the application”—which, in this case, is Automatoonz’s IaC. Follow along with the steps below to set up your pipeline using AWS developer tools. 

To begin, you’ll first need to set up your CodeCommit repository. You can find the code in our [GitHub repository](https://github.com/bksarthak/devsecops-book) (as ch3-codecommitrepo.yml), but we’ll reproduce it here as well:

```
AWSTemplateFormatVersion: "2010-09-09"
Description: Template to create Codecommit repository, CodePipeline
Parameters:
 CodeCommitName:
   Type: String
   Default: AutomatoonzRepo
Resources:
 CodecommitRepo:
   Type: AWS::CodeCommit::Repository
   Properties:
     RepositoryName: !Ref CodeCommitName
     RepositoryDescription: Repo created for automatoonz team infra Code
     Tags:
       - Key: Team
         Value: Automatoonz
```

Running this code sets up a CodeCommit repository with the default name `AutomatoonzRepo`. (If you want to change your repository name, you can do so while launching the CloudFormation stack or pass parameters in the command below.) This is a new, empty repository. As we progress through the steps, you’ll add more functionalities into this configuration file to deploy different resources.

To deploy the repository, run this command:

```
        aws cloudformation create-stack \
--stack-name devsecops-repo \
--template-body file://ch3-codecommitrepo.yml
```

Use the following command to check its deployment status through the AWS console or the CLI:

```
aws codecommit list-repositories
Output:
{
  "repositories": [
      {
         "repositoryName": "devsecops-repo"
         "repositoryId": "a0000e13-x83x-4027-aaef-650c0XXXXXX",
      }
 ]
}
        
```

Wait for the repository to launch successfully before moving on. 

Next, you’ll want to add your first file into the repository. Start by cloning your repository into your local system by executing the steps [provided in the AWS public documentation](https://docs.aws.amazon.com/codecommit/latest/userguide/how-to-create-file.xhtml).

In the root of your cloned repository, create a folder named cfn and, in it, create a new file named secgroup.yml.  The file in the book’s repository, chapter-3/ch3-secgroup.yml, will have the following contents:

```
AWSTemplateFormatVersion: "2010-09-09"
Description: Template to create misconfigured Security Group in a VPC
Parameters:
 VPCId:
   Type: String
Resources:
 MisconfiguredSecGroup:
   Type: AWS::EC2::SecurityGroup
   Properties:
     GroupDescription: Misconfigured Security group which should NEVER be launched
     GroupName: MisconfiguredSecGroup
     SecurityGroupIngress:
       - IpProtocol: tcp
         FromPort: 22
         ToPort: 22
         CidrIp: 0.0.0.0/0
     VpcId: !Ref VPCId
     
```

This code creates a misconfigured security group with an ingress rule that allows anyone on the internet to use SSH over port 22–just like the one our friends at Automatoonz are dealing with. Let’s see how you can stop this group from launching.

## Implementation

Commit the ch3-secgroup.yml file to the newly created repository, then push to origin. (As we mentioned in the beginning of this chapter, if you are not familiar with using Git branches, please see [*Head First Git*](https://learning.oreilly.com/library/view/head-first-git/9781492092506/) or the Git documentation [site](https://git-scm.com/book/en/v2/Git-Branching-Branching-Workflows).) You should now be able to see your file in the repository.

You want your commits to trigger the deployment check and only allow deployment if the code is secure, as shown in [Figure 3-2](#ch03.xhtml#fig_2__flow_of_deployment_check). 

![](assets/securing_your_infrastructure_046357_02.png)

To achieve this automated deployment check, you will use AWS CodePipeline and AWS CodeBuild. CodePipeline will act as a “harness” to link the phases of the pipeline together, while CodeBuild will be your build, configuration, and testing system. [Figure 3-3](#ch03.xhtml#fig_3__full_setup_using_aws_codepipeline_and_aws_codebu) represents the full setup.

![](assets/securing_your_infrastructure_046357_03.png)

You will create another version of the CloudFormation file with CodePipeline and CodeBuild. Let’s walk through some snippets of that code. The full version is in the [Chapter 3 folder](https://github.com/bksarthak/devsecops-book/tree/main/chapter-3) of the book’s repository, named ch3-codepipeline.yml.

To deploy the file, run this command:

```
aws cloudformation update-stack \
--stack-name devsecops-repo \
--template-body file://ch3-codepipeline.yml \
--capabilities CAPABILITY_NAMED_IAM
```

In this file, you will see a couple of IAM roles being created. The [EventBridge](https://aws.amazon.com/eventbridge/) service will use these roles to tell you when someone has committed code to the repository; CodePipeline will use the second role (named CodePipelineRole in the codepipeline.yml) to perform tasks on your behalf. The EventBridge rule looks for changes to the CodeCommit repository and triggers CodePipeline whenever someone commits to the main branch. The excerpt below shows the pattern the EventBridge rule is looking for:

```
EventPattern:
       source:
         - aws.codecommit
       detail-type:
         - 'CodeCommit Repository State Change'
       resources:
         - !Join [ '', [ 'arn:aws:codecommit:', !Ref 'AWS::Region', ':', !Ref 'AWS::AccountId', ':', !Ref CodecommitRepo ] ]
       detail:
         event:
           - referenceCreated
           - referenceUpdated
         referenceType:
           - branch
         referenceName:
           - main
     Targets:
       -
         Arn:
           !Join [ '', [ 'arn:aws:codepipeline:', !Ref 'AWS::Region', ':', !Ref 'AWS::AccountId', ':', !Ref CodePipeline ] ]
         RoleArn: !GetAtt AmazonCloudWatchEventRole.Arn
```

In addition to the two IAM roles within ch3-codepipeline.yml, you have also created a CodeBuild project that will install an open source IaC checker. The checker will verify whether the security group file is misconfigured or not.

```
CodeBuildProject:
   Type: AWS::CodeBuild::Project
   Properties:
     Name: Automatoonz_Project
     Source:
       Type: CODEPIPELINE
     Description: This project will test Code
     ServiceRole: !GetAtt CodePipelineRole.Arn
     Artifacts:
       Type: CODEPIPELINE
     Environment:
       Type: LINUX_CONTAINER
       ComputeType: BUILD_GENERAL1_SMALL
       Image: aws/codebuild/amazonlinux2-x86_64-standard:2.0
     TimeoutInMinutes: 10
```

The CodeBuild project reads the buildspec file (buildspec.yml)from the root directory of the CodeCommit repository during execution. A buildspec file is a collection of instructions and steps performed in the CodeBuild project. 

(See the [AWS documentation](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.xhtml) for more.) You’ll use this build project as a test stage to check the files you commit into your CodeCommit repo. 

The buildspec file performs three major steps. First, it installs an open source checker called [`cfn-nag`](https://github.com/stelligent/cfn_nag), which scans CloudFormation files. Second, it runs that checker on the code we committed to the repository. Third, it provides a pass or fail result. The next snippet, from the buildspec file, shows the first and second stages. 

```
version: 0.2
phases:
 install:
   commands:
     - echo Entering the install phase...
     - gem install cfn-nag
 build:
   commands:
     - echo Entering the build phase...
     - cfn_nag_scan --input-path $CODEBUILD_SRC_DIR/cfn
```

Now you need to commit buildspec.yml to the CodeCommit repository so that the CodeBuild repository can use it. 

Once you perform this commit, visit the CodePipeline service on the AWS Management Console. You should see that the pipeline has started its execution (shown in [Figure 3-4](#ch03.xhtml#fig_4_pipeline_running_after_a_new_file_is_committed_to)).

![](assets/securing_your_infrastructure_046357_04.png)

The first stage of the pipeline is connected to the CodeCommit repository. The pipeline will pull the changes from the CodeCommit repository and pass them onto the next stage, which is the CodeBuild project. 

After the build process completes, the pipeline will fail. This is totally normal and expected. This pipeline *should* fail when there’s a misconfigured resource, like the security group you just committed. But, how can you know that the pipeline failed for *this* reason and not some other reason? 

This is a great time to dive into the pipeline’s logs to see what error occurred, and when and where it happened. Within the AWS console, go to CodePipeline and click on the pipeline you just created. “TestStage” should show as “failed.” Click the “Details” link inside the TestStage box (see [Figure 3-5](#ch03.xhtml#fig_5_opening_the_error_in_codepipeline)). 

![](assets/securing_your_infrastructure_046357_05.png)

This link will take you to the execution logs in your last run, so you can see what was executed as your stages ran through the pipeline. It should look similar to the snippet below:

```
[Container] 2022/03/23 23:36:50 Running command cfn_nag_scan --input-path $CODEBUILD_SRC_DIR
------------------------------------------------------------
/codebuild/output/src699107553/src/secgroup.yml
------------------------------------------------------------------------------------------------------------------------
| WARN W28
|
| Resource: ["MisconfiguredSecGroup"]
| Line Numbers: [9]
|
| Resource found with an explicit name, this disallows updates that require replacement of this resource
------------------------------------------------------------
| WARN W9
|
| Resource: ["MisconfiguredSecGroup"]
| Line Numbers: [9]
|
| Security Groups found with ingress cidr that is not /32
------------------------------------------------------------
| WARN W2
|
| Resource: ["MisconfiguredSecGroup"]
| Line Numbers: [9]
|
| Security Groups found with cidr open to world on ingress.  This should never be true on instance.  Permissible on ELB
------------------------------------------------------------
| FAIL F1000
|
| Resource: ["MisconfiguredSecGroup"]
| Line Numbers: [9]
|
| Missing egress rule means all traffic is allowed outbound.  Make this explicit if it is desired configuration
------------------------------------------------------------
| WARN W36
|
| Resource: ["MisconfiguredSecGroup"]
| Line Numbers: [9]
|
| Security group rules without a description obscure their purpose and may lead to bad practices in ensuring they only allow traffic from the ports and sources/destinations required.
Failures count: 1
Warnings count: 4
[Container] 2022/03/23 23:36:50 Command did not exit successfully cfn_nag_scan --input-path $CODEBUILD_SRC_DIR exit status 2
```

As you can see in the logs, the sections that start with `WARN` and `FAIL` are the misconfigurations within your CloudFormation file. The failure occurred because the secgroup.yml file had multiple security violations. The security group has the following two warnings and one error:

*   `WARN W2`: IP range is open to the world
    
*   `FAIL F1000`: Missing egress rule
    
*   `WARN W36`: Missing security group description
    

# Summary

In this chapter, you created a CodeCommit repository, a CodePipeline, and a CodeBuild project. When you make a commit to the CodeCommit repository, the CodePipeline gets triggered. The CodePipeline has a CodeBuild project as a stage containing `cfn-nag`, which is a CloudFormation checker. 

You learned about three types of security controls that work in tandem to secure your environment, and that adding preventive checks into the software development lifecycle as early as possible helps to prevent security events. You also learned that if you control the ingress point where code gets deployed, you can control whether to deploy a given IaC file into your environment. 

Understanding logging and errors can help you cut down on bugs and have a successful build.  Scanners like `cfn-nag` let you isolate misconfigurations as specific lines of your code, which you can fix before deployment.

The Automatoonz team is off to a great start. They (and you) have created an initial pipeline for conducting IaC checks and preventing security misconfigurations early in the life cycle. In the next chapter, we will show you how to further refine this pipeline by adding logging and monitoring features to detect and prevent tampering.

A quick recap of key takeaways from this chapter:

*   There are three types of security controls that developers should have in place to secure IaC: Preventive, detective, and corrective.
    
*   When you set up a CodePipeline, preventive checks are triggered when code is committed.
    
*   The open source checker `cfn-nag` will look for common security misconfigurations within code.
    

# Chapter 4. Controlling Access Through Automation

In this chapter, we will focus on identity. Specifically, we will discuss enabling authentication and authorization through automation. You will learn about tools you can use to prevent and detect misconfigurations related to identity and access management (IAM) within your environment. 

As a reminder, *authentication* is how you prove who you are, or validate your identity; *authorization* means granting a user permission to do something. 

Let’s start by thinking about the environment in which you currently work, if applicable.  Do you know how many accounts are overly permissive? How many of your permissions are actually used?

There are two types of identities: human and machine. *Human identities* are accounts used by humans for daily, noncritical or one-off operations. Accounts with *machine identities* are used by machines for automating or carrying out certain privileged operations. In an ideal world, human identities would only be used for reading data that is maintained by machine identities. All operations should be automated as much as possible through machine identities.  

In our case study, the members of the Automatoonz DevSecOps team need to provide identities to the users of their infrastructure (humans), as well as to the infrastructure itself (machines). Machine identities will allow services such as EC2 or S3 to communicate with each other. If the infrastructure within AWS does not have the right permissions, you will always get a “Permission Denied” error.

We’ll begin the chapter by examining how the principle of least privilege is critical to your information security program. We’ll then move on to fine-tuning permissions, where we cover how to scale IAM permissions with resource tagging and separation of duties. We’ll also discuss why prevention is just as important as detection when it comes to IAM security controls. We’ll wrap up this chapter by creating a pipeline for detecting IAM policies that are not aligned with security best practices.

###### Note

This book assumes that you have some basic knowledge of AWS, so we won’t be diving into the basics of AWS IAM here. If you need a refresher on IAM, please visit the [AWS documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/intro-structure.xhtml). 

# The Principle of Least Privilege

“Can you give me the same permissions Roland has, so I can do my work faster?” We are 100% certain that you’ve either asked or been asked this question. Without context, this request sounds harmless, but Roland’s account *could* include permissions to delete infrastructure, data, or both. Assigning users the appropriate permissions is not simple, and getting it right usually involves some trial and error. 

The *principle of least privilege* states that any account–human or machine–should be given only the permissions its user needs to complete their job. Let’s look at how this plays out in our case study. Currently, the Automatoonz environment is set up to grant wide-ranging permissions to all members of the development team. How did this happen? Well, the security team had initially locked down their permissions, but the development team asked the security team for broader permissions so as not to obstruct development work. The development team leads, Aisha and Dave, found themselves doing a great deal of back-and-forth with the security team. They eventually pressured the security team to grant wider permissions. 

As Automatoonz grows, the compliance team lead, Lorena, has made it clear that this security arrangement is a problem. Unless they change it, the company will not be certified to work in different geographies, nor will it comply with security frameworks such as NIST 800-53, an industry-agnostic security framework used globally by compliance teams to standardize their security capabilities. The NIST 800-53 standard [mandates](https://csf.tools/reference/nist-sp-800-53/r5/ac/ac-6/) that organizations use (and provide verification that they use) the principle of least privilege. Not following this standard will deter partners and keep the company’s customer base from growing. 

To prove this point, Lorena cites numerous recent examples of incidents that put the Automatoonz business objectives at risk–incidents that resulted from these wider permissions. For instance, last quarter, the audit team and the development team were both granted superuser permissions on their AWS accounts. This meant they could remediate misconfigurations and develop products at their own speeds. The security team noticed that some of their AWS resources, like S3 and EBS volumes, were not encrypted, so they created a KMS encryption key and began encrypting these volumes. Since the security team did not know who was accessing data, or from where, they could not customize the KMS key policy to the appropriate roles. They simply locked access to the entire development team’s storage. Lorena points out that if the security team had the ability to *read* resource misconfigurations, they would not have been able to temporarily lock access to the development team’s storage. 

In another example from the current quarter, the finance team repeatedly warned the development team that their EC2 billing was over budget. To get the finance team off their backs, Dave and Aisha decided to delete some instances that they did not recognize, assuming they were old resources. In a couple of hours, it became clear that they had in fact deleted active resources being used by the art design team. Lorena points out that Dave and Aisha should not have had permissions to delete other teams’ resources.

According to the compliance team, Automatoonz has only a loose definition of who has which permissions, and this situation is affecting the productivity of multiple teams. 

First, let’s be clear that these problems are not just IAM problems. IAM is one of many technical controls used to prevent mishaps like the ones Lorena noted above. We need to evaluate the situation holistically, at 10,000 feet. We can do this using the People-Process-Technology triad for IAM security, as shown in Table 5-1. 

Table 4-1. Overall themes within the People-Process-Technology triad for IAM security

 
| People || Process || Technology |
| --- | --- | --- |
| Who is responsible for fixing problems? || How are tasks and responsibilities divided? || What AWS tools are used for prevention and detection?  
(e.g., GuardDuty, IAM Access Analyzer, Service Control Policies, Permission Boundaries) |
| --- | --- | --- |
| Who is responsible for setting standards? || What is the escalation path? || What metrics are used to measure the health of these tools? |
| Which team is responsible for testing new preventive/corrective/detective controls? || How are exceptions handled? || Notify the responsible team through Simple Notification Service |

This is where the principle of least privilege comes into play. If users are being given permissions beyond the scope of what they need to do their jobs, you have a problem waiting to happen. Automatoonz has had two big incidents in just one year where people with excessive permissions inadvertently hindered the productivity of another team. Even though their intentions are not in question here, their actions had negative repercussions for the entire company. Also, from an audit standpoint, the principle of least privilege is a must-have in most compliance frameworks, so not following it also creates negative repercussions for the company.

In our careers, we have seen entire production databases get deleted simply because someone had permissions to do so, and there was no second layer of defense or verification. In startups, when teams are still small, this usually happens because a few people are granted broad administrative permissions. The company then grows, but the permissions are never revoked or reassessed.

# Fine-tuning Access Controls 

We have seen people push back on having fine-grained access controls through IAM because the fine-tuning of IAM policies never ends. If you start a user with 10 permissions on day 1, they need 2 more on day 10 and 5 more on day 20. Fast-forward to day 365, and the user has been granted nearly all permissions because “they’re needed.”  Where does it stop? And, why put in all the effort to grant fine-grained user permissions if it’s just going to end with broad permissions?

First, you need to know that it *is* impossible to get access controls right in one shot. You will need to iterate to some extent. To reasonably include IAM controls in your DevSecOps plan, there are a few overarching strategies that we recommend building into your IAM automation pipeline. Let’s go through these strategies and break them down one at a time.

## Use a Tagging System

The first component to build is a tagging system. AWS provides resource tagging as a governance feature: You can tag AWS resources, such as IAM policies and roles, or non-IAM resources like EC2 instances, using a key-value pair. For example, if you want to identify an IAM role being used by the game development team, you might use a tag value of “`team:gamedev`.” In this example, “`team`” is the key, and “`gamedev`” is the value.

IAM and AWS resource tagging work well together. The benefits of using tagging include:

*   standardized resource naming for ease of understanding
    
*   data classification to identify sensitive resources
    
*   owner or team tags that show which teams own which resources
    
*   automation and/or security tags to prevent automated actions on certain resources
    

Within AWS, resources allow access to certain permissions through tags and their respective values. 

Let’s look at an example permission policy that implements tagging: 

```
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": "iam:DeleteUser",
        "Resource": "*",
        "Condition": {"StringEquals": {"iam:ResourceTag/type": "temp"}}
    }]
}
```

The policy above allows deletion of IAM users who have the tagged key `type` and the value `temp`. The intent of this policy is to prevent accidental deletion of any other type of user by only allowing deletion of temporary users, such as external auditors, contractors, or interns.

## Clarify Team Responsibilities

It is difficult to build and/or validate IAM permissions for any user–human or machine–without understanding what that user needs to do. Each team has a set of responsibilities and must access certain resources as part of their operations. The earlier you understand what the teams will use for their jobs, the easier it will be for you to build appropriate IAM policies. Within Automatoonz, for example, the developer team is only responsible for spinning EC2 instances up or down if they own the instances. They should not be allowed to terminate or stop instances that do not belong to them. 

The level of rigor and granularity in your IAM controls should increase as you get closer to your production environment. Similarly, the ratio of human identities to machine identities should substantially decrease as you get closer to production. Your automation (via machine identities) should be doing predictable, known deployments. Keep humans as far away from the production environment as possible to avoid human-induced errors.

Teams will often explore services as they learn about or build something new. For example, your developer team may have permissions to spin up Sagemaker instances for learning or exploring, but not in production environments. Such checks can be implemented at the AWS Organizations level using *service control policies* (SCPs). SCPs are IAM-like JSON documents that allow or deny execution of AWS API calls over an AWS account or a group of AWS accounts (if you apply the SCPs onto an Organizational unit).

## Prevent AND Detect

In Chapter 3, you learned about the importance of both prevention and detection in security. Staying true to course, we recommend implementing IAM controls at both the preventive and detective levels. 

As a quick recap, *preventive controls* will block any misconfiguration before it happens in the AWS infrastructure. For example, if you tried to create a public S3 bucket, a preventive control might prohibit that action to prevent you from exposing your data. 

*Detective* *controls* inform you of any misconfigurations in your AWS environment (after resources have been initiated/built). The AWS service GuardDuty is a good example of a detective control. 

Together, these two types of controls complement each other. Detective controls can help you find out what your preventive controls do *not* cover, and how effective they are. Sticking to our previous example, if you add a preventive control to stop anyone from creating a public S3 bucket, you would use a detective control to check for public S3 buckets. If you find one, this indicates that your preventive control is not working correctly. 

Detective controls that are *not* mapped to preventive controls are also a great feedback loop for preventive controls. If your detective controls find a surge of some particular violation in your environment, that could be a sign that you should invest in a preventive control to avoid that type of violation.  

AWS provides many preventive and detective controls as services. Let’s look at a few of them. 

Preventive controls in AWS include:

Service control policies (SCPs)
: 

SCPs allow or deny specific APIs, which can be applied on an AWS account or group of AWS accounts.

Permissions boundary
: 

An additional layer of permission that prevents any IAM role from expanding its permissions without adding them to the permissions boundaries.

Detective controls in AWS include:

IAM Access Analyzer
: 

This will tell you if your resources are accessing material, or being accessed, outside your zone of trust (i.e., outside your account or organization). 

GuardDuty
: 

A machine-learning-based detective control that alerts you when IAM access patterns deviate from a baseline.

AWS Config rules
: 

AWS Config is a service that tracks the states of AWS resources and their individual configurations. AWS Config rules allow you to take actions or set alerts for certain configuration changes to resources. These rules can be custom or managed by AWS. For example, if you want to check whether backup is enabled for a database you created, you could use a Config rule to detect that. AWS provides an ever-growing list of Config rules, which are listed in its [documentation](https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.xhtml). 

# The IAM Pipeline

To implement the concepts you’ve just learned, let’s return to Automatoonz, where the security team is getting requests to approve the developer team’s IAM policies. In this section, you’ll help them create a pipeline that can allow the developers to self-serve and verify the sanity of their IAM policies, or assist the security team in evaluating multiple IAM policies automatically, so they can get approvals out to the developer team as early as possible. The functionalities of the pipeline are dependent on who will use the pipeline. 

###### Note

The architecture we are suggesting here is an extension of the pipeline we created in the earlier chapters. For simplicity, it uses just one open source tool. Feel free to modify and use it according to your needs.

The architecture diagram in [Figure 4-1](#ch04.xhtml#fig_1__high_level_architecture_for_iam_linting) shows the IAM pipeline we’ll build. Let’s say that Dave (developer team lead at Automatoonz) will first commit his IAM policies into the CodeCommit repository that houses all the IaC components. The event of committing these IAM policies will trigger the pipeline we are building here, which contains an open source IAM scanner. This scanner will provide feedback via build logs to tell Dave whether the IAM policies he just committed are secure. Typically, these kinds of tools are called linters. They will scan for issues in your code without actually executing them.

The dotted line in [Figure 4-1](#ch04.xhtml#fig_1__high_level_architecture_for_iam_linting) that runs from the security team to the CodeBuild project is meant to show that the IAM scanner in the pipeline is configured by a security team member, but is not part of the pipeline code.

![](assets/controlling_access_through_automation_064188_01.png)

To build the IAM pipeline, you’ll first create a new directory in the Automatoonz repository you created in Chapter 2. To do that, go to the cloned repository in your system, and run the following command:

`mkdir iam-policies/`

Create a new file inside this directory called requested-iam.json. This is the file Dave is asking the security team to evaluate. Copy and paste the content below into the requested-iam.json file you created.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "s3:DeleteObjectVersion",
                "s3:DeleteObject",
                "s3:DeleteBucket"
            ],
            "Resource": "*"
        }
    ]
}
```

Upload this new file to the Automatoonz repository using the` git commit` command.

Now, let’s switch gears to the Automatoonz security team and figure out how they can scan insecure IAM files at scale. For this task, we’ll be working with another popular open source tool known as [Parliament](https://github.com/duo-labs/parliament). Parliament helps identify possible security issues with IAM JSON files. In order to automate this scan, install Parliament in the CodeBuild project. 

The modified build file will look like this:

```
version: 0.2
phases:
  install:
    commands:
      - echo Entering the install phase...
      - gem install cfn-nag
      - pip install parliament
  build:
    commands:
      - echo Entering the build phase...
      - cfn_nag_scan --input-path $CODEBUILD_SRC_DIR
      - parliament -–directory $CODEBUILD_SRC_DIR/iam-policies
```

Once you’ve edited the build file on your local machine, commit it to the repository so that the new build file is updated for your CodePipeline. Once you push the commit, you’ll see the CodePipeline start to run. If everything runs correctly, your CodeBuild logs should look like [Figure 4-2](#ch04.xhtml#fig_2_output_of_codebuild_failure). 

![](assets/controlling_access_through_automation_064188_02.png)

Let’s look more closely at the code in [Figure 4-2](#ch04.xhtml#fig_2_output_of_codebuild_failure), specifically the line that starts with the word `LOW`:

```
LOW - Unnecessary use of Resource * - ['s3:DeleteBucket','s3:DeleteObject','s3:DeleteObjectVersion']-
          {'line':4,'column':9,'filepath':'/codebuild/output/src/iam-policies/requestied-iam.json'}
```

This is a finding from the Parliament scanner, which says that the IAM policy you uploaded has an asterisk: \*. This means it is applicable to all resources. As you know, the principle of least privilege holds that you should be providing permissions for only what each user needs–including resources.

The output states that the build failed because using \* as the policy resource is too permissive for general usage. The preventive controls in place specify that this policy should therefore be rejected. To automate this process even further, you could create a Simple Notification Service (SNS) topic that sends this failure message back to the person who made the commit, quickening the turnaround time for the IAM policy validation.

In addition to open source tools like iam-lint and Parliament, AWS has also launched an API within IAM Access Analyzer that validates IAM policies for syntactical and security issues. You can make this API part of your CI/CD pipeline. 

# Summary

Let’s recap what you’ve learned in this chapter. First, you learned that the principle of least privilege is important not just from a security perspective, but from operational and compliance standpoints as well. You saw that implementing IAM controls in AWS is not a fixed, one-time task, but an evolving process that involves planning, execution, and iteration. You also now know why it’s so important to understand a user’s need for an IAM permission before provisioning the IAM policy for it.

We also revisited the topic of preventive and detective controls. You learned that they are equally important in understanding how well your IAM is implemented, and that tagging resources is an important first step in implementing attribute-based access control within AWS. 

A quick recap of key takeaways from this chapter:

*   IAM is difficult to get right in one shot. Refine your IAM through prevention and detection mechanisms.
    
*   Utilize AWS’s tag-based access-control mechanisms to better scale your IAM controls.
    
*   Clearly delineate team responsibilities as you build and test IAM policies within your AWS environment.
    
*   Your team should do some deep dives into the IAM roles you’re provisioning, enumerating the permissions and identifying unused roles. In short, it’s wise to clean up the cobwebs in your IAM infrastructure.
    

# Chapter 5. People and Processes

Up until this chapter, we have focused on technology. We’ve discussed how different types of tools have helped us in achieving different security functions. But, just like every sound security program, we have to address the entirety of the People, Process, and Technology framework, which we briefly introduced in Chapter 5. This framework was [popularized in the 1990s](https://www.mirrorreview.com/ppt-framework-an-overview/) by cryptography expert Bruce Schneier, who argues that these three things form the foundation of security. Security cannot be implemented with technology alone, nor can it be implemented with just people or just a process. Balancing the three lets us security-minded folks focus on building secure things instead of spending 99% of our time responding to events.

Now that you know what security tools and setups you need to get your technology to a minimum viable state, this chapter will cover the people and processes related to securing your IaC. Before we dive in, we want to make it very clear that there is no one right way to manage people or processes related to securing your infrastructure. Each organization has its own approach, based on its size, culture, and team structure. The pointers we give in this chapter are derived from our personal experiences, and from common themes we have seen among organizations that have implemented DevSecOps successfully.

# People: Team Structures and Roles

In this chapter, we will refer to a DevSecOps “team,” whose members are focused on this journey. There are certain high-level roles that an ideal DevSecOps team should have. In this context, *role* does not necessarily mean one or two people; depending on the budget and the employees available, you can assign a single person or multiple people to carry out a role’s responsibilities, or assign multiple roles to one person. In this section, we discuss roles related to security, development, compliance, and product manager.

## Security Engineers

There are multiple subdisciplines in security: application security, cryptography, and network security, to name a few. In a DevSecOps context, you might want a security generalist. The fundamental concepts of security should be clear to your candidates, of course; if they don’t know the basics of security, they won’t be able to guide others. If the person is a specialist who has worked in container security extensively, that’s a bonus! But, such expertise isn’t a necessity. AWS provides guidance on security through its public [documentation](https://docs.aws.amazon.com/security/). AWS also updates and launches features and services at breakneck speed. It is difficult to keep up with their pace, much less be proficient in all of their offerings. So, it is not mandatory that your security team be composed of experts in each and every AWS service from day one (and, if you required it, you would have a very difficult time finding candidates). You want your security team to be aware of AWS services and features, generally, and familiar with their relevant security services. 

One way to provide measurable milestones for your security team’s proficiency is to help them earn the [AWS Security certification](https://aws.amazon.com/certification/certified-security-specialty/). AWS Security is no different than other security concepts, but being comfortable with AWS security tools, specifically, may help alleviate any concerns and allow your security team to focus on what is most important in their jobs. 

## Developers 

In order to automate, maintain, and debug your DevOps toolchain, you need to have developers on your team. Remember, DevSecOps is not a single destination; you’ll always be building, improving, and refining your approach. There is no single codebase you can clone to implement DevSecOps perfectly. For some organizations, a developer-heavy DevSecOps team is sufficient, while others do well with a team of security experts who know how to code. The important thing is that the lead developer role for your DevSecOps initiative should *not* be assigned to a part-timer. 

We have seen organizations assign developers to DevSecOps initiatives on a one-off basis (which you should NOT do). In this framework, the developers build the DevSecOps toolchain and then go back to their primary team functions. This structure has two major issues. First, the organization spends time and resources building something it won’t use long term, wasting resources. Second, the organization never actually ends up using DevSecOps consistently, which gives them a false sense of security.  Over time, if no one is maintaining and updating the toolchain and security checks the developers built, then they will be abandoned. Your tooling must be constantly calibrated to align with changes in how your environment uses IaC, or it will fail or be discontinued. 

Saying “I need a developer” is a very broad statement. Based on our experience, most DevSecOps teams include people who are familiar with (in no particular order): 

Coding languages
: 

If you work in the tech industry, you know there is no dearth of coding languages out there. As a rule of thumb, those on your development team should have knowledge of at least one of the languages supported by AWS Software Development Kit (SDK). We recommend Python and Go for AWS DevSecOps roles You can find more details about supported languages for AWS SDK in the [documentation](https://aws.amazon.com/developer/tools/\)). 

IaC languages 
: 

As you build infrastructure and automations within AWS, there is a very high possibility that you will need to use CloudFormation, Terraform, or Cloud Development Kit (CDK). We have also seen some organizations lend their own flavor to these tools–for example, creating wrappers around Terraform to customize the way their developers use it. A DevSecOps engineer should know at least one IaC language to be able to build infrastructure within AWS.

AWS 
: 

For members of your development team, we recommend one year or more of hands-on experience and/or one associate-level AWS certification. We are deep believers in Anton Chekhov’s adage that “knowledge is of no value unless you put it into practice.” We highly recommend prioritizing practical knowledge over theoretical knowledge. A developer who understands the sharp edges of implementing something on AWS will provide an invaluable point of view while building your DevSecOps practice.

Version control systems (VCS)
: 

This is as simple as it gets: If you’re writing code, you need some way to manage versioning and storage of that code. The two most common versioning systems are Git and Mercurial. We don’t prefer one over the other, but having hands-on experience with a VCS helps developers to ramp up quickly.

Shell scripting
: 

You’ll need to use some level of shell scripting to patch together the open source tooling used within your infrastructure (much like the infrastructure we’ve presented in this book). Being able to read and write some level of Bash scripting is also highly helpful in the realm of DevSecOps.

By no means is this a definitive list of every DevSecOps engineering skill. Depending on your organization, your team will need to ramp up additional skills and tools. 

## Compliance Team

As we build secure infrastructures, it is imperative to keep track of the compliance status and operational health of your system. Some might ask, “Why should compliance be part of a DevSecOps initiative? Compliance is all about audit and reporting.” 

Hear us out: In our experience, too often, DevSecOps team only engages the compliance team when there is an escalation or an audit is about to happen. This last-minute inclusion sometimes leads to a scramble to provide data in time. However, when a few compliance team members help the DevSecOps engineers decide up front what data should be collected, things tend to operate much more smoothly. Engineers/developers can also help build reporting capabilities that feed data directly to the compliance team. This significantly reduces confusion when audits occur. 

In fact, one of the most successful teams we’ve worked with built their reporting dashboards according to their compliance team’s specifications. A tactical dashboard was used mainly by engineers, while the compliance team had a “risk posture” dashboard that showed a 10,000-foot view of all the checks and balances. Your compliance team should be provided with data gathered from all the checks and balances in a normalized (or, cleaned) manner, so they have the latest information about the security posture of the organization. 

It’s crucial to include your compliance team in the DevSecOps journey early on. This helps ensure that your hard work scales to multiple teams, and that the business gets a solid return on investment. 

## Product Manager 

Not every security team has an engineering manager or a technical product manager. The title is not as important as the function here. We suggest defining this role as a person who looks at the DevSecOps initiative as a whole and represents the DevSecOps team at a strategic level. 

A team of engineers that works in isolation without strategic alignment with the wider DevSecOps initiative will eventually be overlooked. They might not even be seen as contributing to the company’s overall objectives. This is exactly where the “product manager” role fits in. Some of the responsibilities of this role include managing workflows, ensuring the engineers on the team are not overloaded with multiple requests, and acting as a “load balancer” for requests and work priorities. This role should also help the DevSecOps team align with the strategic goals of the organization, focusing on the team’s objectives and improving morale.

## Team Structure

Throughout this book, we have followed Automatoonz as they secure IaC at scale with their existing team and technology. You’ve seen that Automatoonz is structured as siloed teams with individual team goals. These team goals are not communicated with other teams, which has led to business disruptions. 

If we were to draw the Automatoonz DevSecOps operating model, it would look something like [Figure 5-1](#ch05.xhtml#fig_1__siloed_operations_at_automatoonz).

![](assets/people_and_processes_967487_01.png)

Everyone has direct access to the AWS cloud infrastructure, and everyone has their own methods of using the cloud. The lack of consistency and standardization is a result of the company’s “do-what-works-best-for-you” approach.

In a mature organization that has transitioned operations to the cloud and has been built around the skills and responsibilities mentioned in this section, these siloes are united. This unification is due to the common goals, practices, and processes engendered by DevSecOps principles. These principles include standardizing the procedures for making changes in the infrastructure. In short, if anyone needs to access (or change) the cloud infrastructure, there are certain rules they must follow. This more focused structure, based on DevSecOps principles, is shown in [Figure 5-2](#ch05.xhtml#fig_2__focused_operations_using_devsecops).

![](assets/people_and_processes_967487_02.png)

Now that we’ve explored the people involved in securing IaC, let’s move on to exploring the processes–how teams operate within this focused structure.

# Processes: Practices and Communication

As with the previous section, we want to remind you that the processes we discuss in this section are not a definitive list. Instead, we provide some guiding principles that you can implement as appropriate, depending on your organization’s size and existing practices. 

###### Tip

Regardless of which practices you are following, we highly recommend having measurable goals and using metrics to define the effectiveness of your processes. Defining and setting metrics is a science of its own; we will not go into detail in this chapter. If you’re interested in learning more, we recommend [*How to Measure Anything*](https://www.oreilly.com/library/view/how-to-measure/9781118836446/), 3rd edition, by Douglas W. Hubbard (Wiley) and [*How to Measure Anything in Cybersecurity Risk*](https://www.oreilly.com/library/view/how-to-measure/9781119085294/) by Douglas W. Hubbard et al. (Wiley).

## Communicate to the Right People, Consistently

As soon as you begin to implement any kind of change in an organization, you need to notify stakeholders at all levels, from leadership to the tactical workforce. Explain what the change brings to the organization and what resources it is going to require, and keep the stakeholders informed on your progress. 

During this process, we’ve often seen people focus so hard on the good news that they end up not communicating the bad news. That is a big mistake. If you keep the blockers and hindrances hidden, your stakeholders will never know how to help you out. Be transparent and honest about what is working and what isn’t. Table 7-1 outlines the basic building blocks of a typical status report.

Table 5-1. Sample status report structure

 
| Section || Includes || Example |
| --- | --- | --- |
| Highlights || Things your team has successfully accomplished || We implemented IAM checks for 60% of projects in our environment.  |
| --- | --- | --- |
| Lowlights || Things your team could not implement successfully || Our team is being asked to remediate security misconfigurations, which is taking 40% of our time. |
| Trends || Metrics that show how your highlights and lowlights are trending || After implementing an IAM scanner upon submission, we saw a 15% uptick in detected security misconfigurations.  |
| Upcoming objectives || Focus areas for the next cycle || We plan to ensure that all containers running in our environment have logging enabled and cannot be tampered with. |
| Blockers || Items currently at a standstill, and potentially a root cause for the issue || We have not been able to identify a SME for containers. |

Providing these high-level details helps you clearly and succinctly communicate to stakeholders what you have been working on and how it has been helping the organization. It also opens doors for people outside the team to contribute or guide the team through its blockers.

## Make Product Owners Accountable for Their Security Findings

If we could correct just one wrongheaded notion in organizations everywhere, it would be this one: “It’s a security problem, so security should fix it.” 

We disagree with this school of thought. Security teams augment and assist the business objectives by making sure things are run securely, but the institutional knowledge of *why* something is built the way it is belongs to the relevant teams. For instance, when a development team starts building a particular feature, they go through the design process, weigh the risks and rewards, make decisions, and then build it. No one can explain that feature or provide background knowledge better than the development team that built it. So, when the security team detects a vulnerability with that feature, the team best equipped to address it is actually *development*. The security team can provide a second pair of eyes for verification or assistance in implementing a solution, but if the security team is pulled into remediating every misconfiguration found, the list quickly becomes so long that just keeping up would require a second team. We’ve seen many security teams get overwhelmed this way, especially in large enterprises. 

Every product team should be open to working with the security team to fix their misconfigurations, rather than just offloading the remediation. On the same note, if the security team notices a pattern of similar problems being repeated elsewhere in the organization, they should invest time in finding and fixing the root cause. If a new preventive or corrective control would solve the problem at a larger scale, security should take the lead in implementing it.

## Build Threat Modeling Into Your Processes

If you aren’t accustomed to including threat modeling in your processes, it can definitely feel like additional overhead. Yes, it is an additional step. But, building threat models tells you whether your controls are covering those attack paths with the most potential to impact your workload. 

Each time you build a threat model, some of the security vulnerabilities will have a constant security control associated with them. That is totally OK; in fact, this is how you will build your control matrix, a sort of library of your controls mapped to threats. Larger organizations usually track this control matrix using governance tools, such as RSA Archer. Smaller organizations often store it in accessible places such as wikis. Regardless of where your control matrix is stored, it should be updated regularly. 

A great way to start creating your control matrix is to follow the [Cloud Security Alliance’s Cloud Control Matrix (CCM)](https://cloudsecurityalliance.org/research/cloud-controls-matrix/) model. At the time of writing, the CCM has close to 200 controls, spread across 17 security domains, which provide guidance on what security controls should be implemented in a cloud infrastructure. It also includes a security questionnaire designed to function as a starting point for the compliance team. 

###### Warning

Following the CCM does *not* mean you can forget about threat modeling. The CCM is meant to be industry-agnostic, so it does not cover everything. Its controls are objective in nature, whereas you will need to make subjective calls about how to secure certain parts of your application. Something that poses a threat to *your* system might not be a threat for someone else. If you decide to use the CCM as a checklist, you *will* be bound to it. We recommend using CCM as a baseline, then employing threat modeling to find gaps in your security posture. 

To summarize: Start with CCM as your list of must-have controls in any application you build, then use a threat model to find any controls you should add or remove. Let’s look at an example to illustrate this idea.

Let’s say that you, much like our friends at Automatoonz, have an application that is externally exposed. Your users need to log in to use this application. Per the STRIDE model, which we discussed in Chapter 3, if multi-factor authentication (MFA) is enabled for your users, no malicious actor can spoof an authentic user’s account. If you started off with the CCM, you already have a control matrix that includes a control statement for MFA. So, next time, if you see another application that has external users logging in, you’ll know that you can address spoofing and repudiation concerns with your existing MFA pattern. 

Now let’s say that, in the same application, you need to authenticate your backend services using mutual Transport Layer Security (mTLS). This particular control is not present in the CCM, but mTLS is a security control, so you can add it to your control matrix for future use. Your final control matrix should contain references to all security controls that are present, missing, or need to be worked on for the application to be deployed.

Try to standardize the CCM controls where possible, whether they be detective, corrective, or preventive controls. We’ve seen threat models provide value by means of a secure pattern-generation tool. Let’s look at an example from Automatoonz.

A few years ago, Lupe, who handles threat modeling for the Automatoonz security team, was asked to threat model four separate applications, all of which used SSH for their functionality. Every single one of these applications had the same need, but Lupe and the security team had to guide different teams on the same thing four separate times. Rather than keep this up, the security team decided to build a standardized secure SSH pattern, since that was the common denominator among all of these SSH use cases. 

The next time a team came to Lupe for help with implementation of SSH, she simply provided the approved pattern and its associated requirements. Now, if the application meets the requirements, everyone can rest assured that it’s using a secure, approved pattern, which accelerates the security team’s review. The team can still consult with application owners if they have trouble meeting the requirements or if there’s some special need not addressed by the pattern, but they are no longer wasting time and energy doing the same work multiple times.

## Build Roadmaps to Reach Your DevSecOps Goals

When building new initiatives, it is imperative to set SMART (specific, measurable, attainable, relevant, time-bound) goals for your team, on a quarterly basis at minimum. These team goals should, of course, align with your business’s overall goals as well. 

Here are some examples of next quarter’s SMART goals for our friends at Automatoonz: 

*   As a security engineer, I should be able to identify AWS resources within our AWS accounts. (Scott, Inventory and Asset Management)
    
*   As a compliance officer, I should be able to see the compliance coverage for our AWS workloads. (Lorena, Audit and Compliance)
    
*   As a product manager, I should be able to identify the top security use cases and prioritize workloads for the DevSecOps team. (Bex, Product Manager, Security)
    

# What Next?

You are now at a pivotal stage, ready to take what you’ve learned in this book and implement it in your own workplace. If you are working in a corporate setting and the problems that Automatoonz is facing sound familiar, your workplace is likely a good candidate to adopt DevSecOps.

We know that adopting DevSecOps is easier said than done. As a first step, we urge you to list the use cases where security-focused automation could help you. Estimate the work it would take to roll out such automations and have an open dialog with your team and managers to explore the possibilities. Once you collectively agree on an automation to deploy, start the deployment and record the metrics to show progress. If you can show with metrics that your solution has reduced manual labor and saved multiple teams time, then you have data you can leverage to convince your company to take a more comprehensive program-level approach to DevSecOps.

We’ll close with a few final tips gained from our experience in running DevSecOps programs:

Identify the pain points that matter.
: 

Everyone has something they want to fix, but focus on the things that matter most. Identify which issues affect productivity across the board and rank them by the magnitude of their impact, frequency of occurrence, or both.

Start small, iterate often.
: 

In your search for the perfect solution, don’t get caught in “analysis paralysis,” where you’re using people’s time (and thus money), but not generating anything useful. Don’t shy away from building proofs of concept to demonstrate your ideas. Even if the proof of concept disproves your hypothesis, it still yields valuable data. Once you build your v1, ask the users which big-ticket items they want to see fixed. Prioritize them as you build your v2. 

Measure as much as you can.
: 

When identifying a problem to solve, you need to ensure that the problem is worth solving. Data is important here too. If you can show that the problem occurs nearly every week and takes X number of hours for Y engineers to solve, you show a clear need to solve this problem. And once you have implemented a solution to address the problem, how do you show that it worked? Measurement. Using the same metrics you used to show the impact of the problem, you can demonstrate how the problem has declined after the solution was implemented, proving your solution’s efficacy.

Building a successful DevSecOps program takes iteration and measurement. Everything else follows from that. If you have a team with the right mindset and skills, you can evolve to solve bigger problems at your workplace.

# Summary

As you finish this book, we hope you have learned to recognize the right people, the right tools, and the right approach to begin your DevSecOps journey. It is important to find the right partner for this journey, so make sure your team is also excited and determined to move in the right direction.

A quick recap of key takeaways from this chapter:

*   The size and composition of the roles on a team depend on your budget, and the company’s needs. One size does not fit all.
    
*   Your DevSecOps program/initiative should include a team of developers, security engineers, and a product manager. This team’s primary function is to build and maintain applications that support the principles of DevSecOps.
    
*   At its inception, your DevSecOps program/initiative should include a compliance team; this should not be a last-minute addition.
    
*   Ensure consistent communication within your team and with your stakeholders, ideally by way of a concise status report. Transparent communication breeds trust and encourages data-driven reasoning.
    
*   Developers who build misconfigurations into their applications should be accountable for remediating them. The security team can assist them, but the originators of the misconfigurations should be the driving force of remediation. 
    
*   Use threat models and document architectural patterns as you see them. Having a library of modular patterns promotes the creation of reusable, secure architectures.
    
*   Give your security team time to prevent recurring misconfigurations. Security teams should not be fighting fires every day. If there is a recurring problem, allow your security team to spend time investigating the root cause and fixing it.
    

# About the Authors

**BK Sarthak Das** is a Senior Security Architect at AWS, who has helped multiple Fortune500 customers in securing their cloud environment. BK started his career as a full-stack web developer, and grew into the security domain, which led him to get his Masters from University of Washington (Seattle) with a focus on Cybersecurity. BK has published multiple AWS tech blogs and is regularly building solutions, which can be adopted by AWS users.

**Virginia Chu** is a Senior Cloud Infrastructure Architect at AWS. She works with enterprise-scale customers around the globe to design and implement a variety of solutions in the cloud. Virginia started as Linux system administrator and developer wearing many hats. She’s self-taught, so in her spare time she’s digging deep and trying to learn everything she doesn’t already know. Virginia has published AWS tech blogs and provides modern solutions to the cloud community.