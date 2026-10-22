# What’s the higher risk?

![](bbox=[0.19, 0.19, 0.47, 0.33])

vs

![](bbox=[0.56, 0.19, 0.84, 0.33])

Figure 3.2 – A set of perfectly normal stairs and a grizzly bear.

The reality is that stairs kill far more people than bears do. On average, 12,000 people a year die from falling down the stairs. A few dozen people are killed per year by bears. However, most people will feel instant fear when faced with bear due to many different factors. What does this have to do with risk and security? When we look at the risks that impact our organization and software, it’s important to put our risks in perspective. Most people that fall down the stairs may get right back up with some bumps and bruises, while the off chance of being attacked by a bear is likely to be much more fatal. Being under attack by an advanced persistent threat (APT) like a nation state is of course concerning for any organization. However, most organizations should be more concerned about the daily noise that comes from automated attacks and less sophisticated attackers. Although this is not as flashy as the attacks that come from an APT, the organization is more likely to see automated attacks.

Now that we understand some basic terminology and have some perspective on risk, let’s turn to threat modeling. For many, it’s easier to understand the concept of threat modeling by first taking a look at the manual method.

# 3.1.2 Manual threat modeling

Imagine that an organization has determined that they need to identify potential threats and risks to a new feature that they want to deliver to their clients. Although threat models can be done later in the development life cycle, as with most security tools and techniques, the most benefit will come from performing this activity as early as possible. There are several inputs that are required for a successful threat model:

+   - A completed architecture diagram and description of the feature.
+   - A data flow diagram that shows how data will flow through the system.
+   - A software bill of materials (SBOM) that provides a list of the software components used in the development of the application.
+   - Web service integration points such as APIs and other web services with third-party or internal systems.