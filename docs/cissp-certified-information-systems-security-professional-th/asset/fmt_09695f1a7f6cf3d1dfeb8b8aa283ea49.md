
The topic of `Risk management`, and all the concepts within it, is at the heart of information security and is the core of every strong information security program. `Risk management` includes all the processes associated with identifying threats and vulnerabilities and quantifying and addressing the risk associated with those threats and vulnerabilities. Risk management processes provide a structured method for making security decisions such as purchasing and implementing security tools and hiring people. This section covers the key concepts behind risk management and guides you through applying these concepts in your organization.

### Identify Threats and Vulnerabilities

In security, a `risk` is the potential for negative impact on the organization, its goals or objectives, or its assets (including people, systems, and data) due to a `threat` exploiting a `vulnerability`. You should note that there are dozens of definitions for each of these terms (i.e., `risk`, `threat`, and `vulnerability`) across different industries. We'll discuss these terms further, but it's important to understand that risk lies at the intersection of the three components shown in [Figure 1.5](#c01.xhtml#c01-fig-0005).

![FIGURE 1.5 Relationship between threats, vulnerabilities, assets, and risks](images/c01f005.png)

[FIGURE 1.5](#c01.xhtml#R_c01-fig-0005) Relationship between threats, vulnerabilities, assets, and risks

#### Threats

A `threat` is a negative event that can lead to an undesired outcome, such as damage to, or loss of, an asset. A threat is posed by a `threat actor`, which is a person or entity that is capable of intentionally or accidentally compromising an asset's security. As security professionals, threats are the people and events that we work to protect our information and systems from.

Examples of security threats include the following:
* A hacker who wants to encrypt your data and charge you a ransom for it
* A disgruntled employee who wants to steal or sell corporate information
* A fire or other natural disaster that may damage or destroy your datacenter

#### Vulnerabilities

A `vulnerability` is a weakness or gap that exists within a system that may be exploited (by a threat actor) to compromise an asset's security or trigger a risk event. Vulnerabilities are the things within our systems that we try to fortify and improve.

Examples of security vulnerabilities include the following:
* Unpatched software applications
* Weak access control mechanisms (e.g., weak passwords)
* Faulty fire suppression systems

#### Assets

An `asset` is anything of value, which may include people, property, and information. Assets are the things that we, as security professionals, are trying to protect. People assets may include your company's employees, contractors, and vendors, as well as your customers. Property assets include tangible things like servers and equipment, as well as intangible things like software code and other intellectual property.

### Risk Assessment

Remember that risks are the intersection between threats, vulnerabilities, and assets, as shown in [Figure 1.5](#c01.xhtml#c01-fig-0005). A `risk assessment` is the set of activities that involve identifying the threats and vulnerabilities that exist and determining the impact and likelihood of those threats exploiting the identified vulnerabilities.

There are numerous risk frameworks (as discussed in the “Risk Frameworks” section) that provide guidance on conducting risk assessments, but generally speaking, risk assessments include the steps shown in [Figure 1.6](#c01.xhtml#c01-fig-0006).

![FIGURE 1.6 Steps for assessing risk](images/c01f006.png)

[FIGURE 1.6](#c01.xhtml#R_c01-fig-0006) Steps for assessing risk

#### Risk Identification

The first step in a typical risk assessment process is to identify your assets and determine the value of those assets; this includes identifying and classifying your sensitive data, based on its sensitivity or value to your organization. During the risk identification phase, you find the systems, applications, and information that need protecting and then identify and describe the vulnerabilities and threats that pose a risk to each of those assets.

#### Risk Analysis

Risk analysis should always begin with a vulnerability assessment (discussed in [Chapter 6](#c06.xhtml), “Security Assessment and Testing”) and a threat analysis (discussed in the section “Understand and Apply Threat Modeling Concepts and Methodologies” later in this chapter). This stage of risk assessment is focused on evaluating the likelihood of identified threats exploiting weaknesses (i.e., vulnerabilities) in your environment and determining the impact to your assets if that happens. `Likelihood` describes the probability that an event will occur, and `impact` defines how disastrous the event would be if it were to happen.

Likelihood can be identified by evaluating each threat and assessing the probability that the threats might actually exploit a vulnerability, or weakness. For example, you might determine that the risk associated with a destructive fire is relatively low if you have redundant fire suppression systems that are tested monthly; if you have mechanisms in place to detect and extinguish fires and if you are testing those mechanisms regularly, then the likelihood, or probability, of a fire destroying everything is reduced. Similarly, you might identify insider threat as high likelihood if you've contracted with a large organization without conducting thorough background checks — in this situation, there is a greater probability that something bad will happen.

Impact can be identified by establishing the value associated with each potentially affected asset and determining how that value will be destroyed or otherwise affected by an adverse event. An asset's value can be both `quantitative` (i.e., determined by its cost or market value) or `qualitative` (i.e., determined by its relative importance to you or your organization). By establishing an asset's value, you can better determine the impact of that asset's security being compromised — this allows informed decision-making when determining how much to spend on safeguarding a given resource, as you never want to spend more protecting an asset than the asset itself is worth.

#### Risk Evaluation

During risk evaluation, you compare the results of your risk analysis to your organization's established risk profile or risk tolerance (i.e., how much risk your organization is willing to take on). In doing so, you are able to determine the best course of action for each of your identified risks. We cover the various options for risk response in the following section.

### Risk Response/Treatment

Once you identify and assess your organization's threats, vulnerabilities, and risks, you must determine the best way to address each risk; this is known as `risk treatment` (or `risk response`). There are four main categories of risk treatment, as we describe in the following sections: avoid, mitigate, transfer, and accept. Each of these are ultimately leadership/management decisions that should have CISSP input and awareness.

#### Avoid

`Risk avoidance` involves eliminating an identified risk by stopping or removing the activity or technology that causes the risk in the first place. Organizations use risk avoidance when a particular risk exceeds their acceptable risk tolerance, but complete avoidance is often difficult to achieve without business disruption. While this type of risk treatment can often mean simply not doing something, policies that ban the use of removable media or personal cloud storage services are avoidance steps that require upfront investment and action.

#### Mitigate
