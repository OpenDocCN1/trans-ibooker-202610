![](bbox=[0.15, 0.1, 0.85, 0.36])

Figure 2-5. AWS Recommending the “Rehost” Strategy

# Rehosting On AWS

The approach for rehosting your legacy .NET application onto AWS will vary depending on how your application is currently hosted. For web apps deployed to a single server running IIS you can easily replicate this environment on an Amazon Elastic Compute Cloud (EC2) instance on AWS. If your .NET application is currently deployed to a managed environment (an “App Service” as other cloud providers might call it) then the equivalent on AWS is *Elastic Beanstalk* and you should find the experience of working with Elastic Beanstalk familiar. We will cover migrating managed hosting to Elastic Beanstalk later on in the section “Elastic Beanstalk” but first, let’s take a look at the case of rehosting a virtual machine running IIS over to EC2.

# Application Migration Service (MGN)

The latest AWS offering for performing a lift-and-shift migration to EC2 is called the *Application Migration Service* or MGN⁶. This service evolved from a product called CloudEndure that AWS acquired in 2018. CloudEndure is a disaster recovery solution that works by creating and