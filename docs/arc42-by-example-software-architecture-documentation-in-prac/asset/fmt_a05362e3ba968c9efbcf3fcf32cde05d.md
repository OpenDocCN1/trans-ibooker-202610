
* `HTTPUnit`: A testing framework for web applications and websites. Its main focus is web testing and it suffers from a large number of dependencies.
* `HtmlCleaner` (`http://htmlcleaner.sourceforge.net/`).

## II.10 Quality Scenarios

### Note
You may want to know about specific and operational quality requirements in the form of a specific quality tree. This section completes, augments, or refines the top-quality goals that were already given in arc42, section 1.2.

For our small example, such a quality tree is overly extensive, whereas in real-life systems, we've seen quality trees with more than 100 scenarios. Therefore, we will stick to (repeating) a few scenarios here.

### Quality Scenarios
The quality scenarios are listed here:

![Figure 2.26: Description of quality parameters](image/B15546_02_26.jpg)

*Figure 2.26: Description of quality parameters*

## II.11 Risks and Technical Debt

### Note
You may want to know about the technical risks of your system so that you can address potential future problems.

In addition, you may want to support your management stakeholders (that is, project management, product owner, and so on) by identifying technical risks.

In our small example, we can't see any *real* risks for architecture and implementation. Therefore, the risks that follow are a bit artificial.

### 11.1 Technical Risks
The technical risks are as follows:

![Figure 2.27: Technical risks](image/B15546_02_27.jpg)

*Figure 2.27: Technical risks*

### 11.2 Business or Domain Risks
The business risks are as follows:

![Figure 2.28: Business risks](image/B15546_02_28.jpg)

*Figure 2.28: Business risks*

## II.12 Glossary

### Note
You need to understand the most important domain terms and expressions that stakeholders use when communicating with the system, its interfaces, goals, and requirements.

The glossary is one manifestation of our pledge to "explicit, not implicit," as it associates words or terms with sense, meaning, and semantics.

In the case of our small example, the terms given here should be good friends to most developers.

The following table lists the important terms that have been used throughout:

![Figure 2.29: Glossary of common terms](image/B15546_02_29.jpg)

*Figure 2.29: Glossary of common terms*

# Chapter 3

## III - Mass Market Customer Relationship Management

By **Dr. Gernot Starke**

**Mass Market Customer Relationship Management** (**MaMa-CRM**) takes on the burden of (usually paper-based) customer contacts for organizations working in mass markets, such as insurance companies, credit card providers, mobile telecommunication providers, energy and water providers, and large real estate companies (in MaMa speak, these are called **Mandators**).

MaMa-CRM was initially ordered for an independent, mid-sized data center to support the launch of the German (government-enforced) e-Health card, which is now used to support campaigns such as telephone billing and electrical power metering.

For every mandator, there is at least one completely independent MaMa-CRM instance running, which is specifically configured for its mandator and a campaign.

The MaMa-CRM architecture documentation is quite heavy in terms of its requirements, describing several aspects of flexibility that triggered many central architecture decisions.

The team that built the system consisted of 7-10 people working in 2-4-week iterations for about 15 months.

I (Dr. Gernot Starke) had the honor of being part of that team in a responsible role. The original client allowed me to talk and write about the system without disclosing the company's name. I was not allowed to use any of the original documentation or source code.

Thanks to Thorsten, Sven, Arno, and others for their great cooperation.

## III.1 Introduction and Requirements
This chapter describes the MaMa-CRM platform, a software-product family that coordinates processes between mass market service providers and their clients. MaMa-CRM provides the foundation for individual systems, which have to be customized or configured for specific CRM domains.

MaMa-CRM has been built and is being operated by an **Innovative DAta Center** (let's call it **InDAC**) – a company that provides a variety of hosted software services to its clients. Within business process outsourcing initiatives, InDAC is operating specific instances of MaMa-CRM for its customers.

Let's clarify MaMa-CRM's scope with a few brief examples (you'll find more details in the *Requirements Overview* section):

* Telecommunication providers offer new or updated tariffs to their customers. This is achieved through several possible channels, such as email, phone, letter, or fax, so that the service providers can respond or issue support queries regarding such offers.
* Retail organizations send specific advertising or marketing material to certain customers. Again, these can react to several different communication channels. These kinds of advertisements are part of target group marketing.
* Insurance companies propose modifications or enhancements for the existing contracts to specific customer groups. People who are insured, or organizations, can react over different communication channels, such as phone, mail, email, or even in person. If the consumers accept this proposal, they have to submit a legally binding signature.

### Note
The real company behind the MaMa-CRM system prefers not to be disclosed here. The name "InDAC" is therefore pure fantasy. The original system wasn't called MaMa but had a different name – I hope you don't mind.

The following diagram shows a generalized overview of MaMa-CRM:

![Figure 3.1: MaMa – generalized overview](image/B15546_03_01.jpg)

*Figure 3.1: MaMa – generalized overview*

In the preceding diagram, the **mandator** represents an arbitrary company or organization serving mass market customers. **MaMa**, our system, can host CRM campaigns for many different mandators from different industries.

### 1.1 Requirements Overview
MaMa-CRM controls customer relationship CRM campaigns, which InDAC (the contractor of MaMa) conducts for their mandators. InDAC's mandators are companies such as insurance or telco enterprises, which themselves offer services or products to mass market customers. These mandators outsource their CRM-related business processes to InDAC.

MaMa-CRM supports the following campaign types:

* Changes to tariffs or contracts for insurance companies, telecom and internet service providers, and energy suppliers. You'll find a detailed example of this process next.
* Processing of billing information for call-by-call telephone service providers: The mandator submits billing data to MaMa, which coordinates printing, submission, and support for the resulting invoices. The management or handling of payments is out of scope for MaMa-CRM.
* Management of binary images for credit card and insurance companies, which print the cardholders' images onto the cards to prevent misuse. The German e-Health card belongs in this category.
* Meter reading for either energy or water for companies or large-scale real estate enterprises.

As MaMa is the foundation for a whole family of CRM systems, it adapts to a variety of different input and output interfaces and channels without any source code modification needed!

#### 1.1.1 Campaign Example: Mobile Phone Contract Modification
This example should clarify the complex business and technical requirements of MaMa.

**MoPho** is a (hypothetical) mobile phone provider with a large customer base and a variety of different tariff options (flat fee, time-based, volume-based, and so on). Some of these tariffs or tariff combinations are not marketable any more (a shorthand for **MoPho** does not earn its desired profit from them – but that's another story). Others are technically outdated.
