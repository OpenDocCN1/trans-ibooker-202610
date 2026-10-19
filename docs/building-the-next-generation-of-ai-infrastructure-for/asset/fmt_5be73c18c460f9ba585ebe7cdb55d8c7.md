
## Leverage and Make the Most of the Increasing Compliance Requirements

Compliance is often perceived as an additional exercise, something to be done after the fact. Some people even see it as a burden, or a barrier to innovation. But the reality is that compliance is a fairly good step toward generating trust (with partners, clients, and users) and self-certainty. Basically, it means using general rules and best practices as guidance to make sure you are doing the right thing.

Concretely, you can explore in [Figure 2-1](#ch02.html#chapter_2_figure_1_1775793357024448) the compliance building blocks and select those that are most relevant for your AI infrastructure and use cases.

![](assets/bnga_0201.png)

The blocks are:

### Management systems

International entities like the International Organization for Standardization (ISO) define a management system (MS) as “the way in which an organization manages the interrelated parts of its business in order to achieve its objectives.” From a compliance perspective, standards like ISO 9001 for quality, ISO 14001 for environment, and ISO 27001 for security topics, are classics that impact any organization at a transversal level. However, the most relevant standard for your AI initiative is ISO 42001, which focuses on AI Management Systems (AIMS).

From an AI infrastructure perspective, ISO 42001 can bring you an iterative approach to continuously improve the specific scope of your infra from a product or service perspective. For example, if you want to certify your AI governance and risk management work for a specific software as a service (SaaS) product, you can plan requirements and audits to pass the ISO 42001 certification, in the same manner as the big technology companies are already [certifying their products](https://oreil.ly/jjE6i).

### Applicable regulations

If we simplify the regulatory compliance exercise required for AI infrastructure layers, the main—and most straightforward—requirements are related to your data foundations and AI modeling activities. Currently, that translates directly to existing data protection laws and incipient AI regulations.

Without going into the details of cases like the EU AI Act, GDPR, CCPA, or recent executive orders related to AI, the key idea here is to connect your AI infra with specific requirements, which can then be converted into traces and logs you can keep automatically or manually. For example, saving concrete logs or files of the decision making of an AI agent that sits on top of your infrastructure requires tracking and saving every single step of the agent, including prompts, results, AI model versions, related data stores, endpoints, etc.

### Vertical industry requirements

As with general data and AI regulations, you need to align your AI infra layers to specific standards and regulations. For example, your AI infra may need to include [Fast Healthcare Interoperability Resources (FHIR) or Digital Imaging and Communications in Medicine (DICOM) standards](https://oreil.ly/TQ016) into the data foundations layer, but also adhere to regulations like the Health Insurance Portability and Accountability Act (HIPAA) in the United States. If you work with the US government, you may need to map requirements for the Federal Information Processing Standards (FIPS), the Federal Risk and Authorization Management Program (FedRAMP), or the Defense Information Systems Agency Security Technical Implementation Guide (DISA STIG).

All this can be achieved by finding specific elements for your infra layers, such as operating systems that include [compliance offerings](https://oreil.ly/I2yTh) in their professional versions or processing units with specific [trust features](https://oreil.ly/OE7M8).

### Security essentials

When planning your AI infrastructure, it is critical to take security seriously. While AI systems expand the attack surface and introduce new layers of risk, an approach driven purely by worst-case assumptions can lead to systems that are overly complex, inefficient, or even unusable. Instead, you could address AI security as a risk-based discipline, where decisions are guided by the balance between potential impact, likelihood of threats, and operational needs. Leaders like you need to plan with worst-case scenarios in mind to understand the boundaries of risk, but you need to execute based on informed trade-offs that align with your organization’s risk tolerance.

The expanded surface of attack requires a layered and pragmatic security strategy that reduces meaningful risk while maintaining system functionality and performance. This may include:
* Securing critical layers of the AI infrastructure based on their risk exposure
* Protecting access points and enforcing strong identity and access controls
* Preserving data integrity across training, inference, and storage pipelines
* Continuously assessing and addressing code and dependency vulnerabilities
* Understanding and mitigating model-specific risks (e.g., prompt injection, data leakage)
* Relying on trusted data sources and validating external inputs

As mentioned earlier, you can review Chapter 4 in my [*Linux and Open Source Software in Azure*](https://www.oreilly.com/library/view/linux-and-open/9781098178284/) report (O’Reilly) to see an applied example of layered protection strategy.

### Supporting frameworks

Last but not least, leverage industry frameworks such as the NIST Artificial Intelligence Risk Management Framework (AI RMF 1.0) for artificial intelligence risk management, or Data Management Association (DAMA) and Data Management Capability Assessment Model (DCAM) for data governance and management in your foundations layer. Rather than just understanding the framework, focus on what really matters from an AI infra perspective. What is the role of each layer in generating and providing the important information, such as data quality controls for AI or specific risk mitigation strategies from a software code perspective?

In general terms, try to create an inventory of the technologies that are part of your AI infrastructure, and then visit the [compliance and trust centers](https://oreil.ly/MZ3-T) or specific product descriptions for those technologies. And don’t forget to plan your compliance exercise with an auditor mindset, in which your team(s) can test your AI infrastructure against specific requirements.

## Treat Data as Your Most Valuable Asset

While AI models and capabilities can become commodities (to some extent), your data is yours and it is part of your core intellectual property. Besides compliance and security reasons, your key motivation from an AI infrastructure perspective is to make sure that regardless of where your data comes from (where it resides), it always remains yours. Imagine that data journey among infra layers, and try to track the virtual path that goes from your data foundations, to your code and modeling layers.

For example, you can explore the wide range of CPUs and GPUs, and look for [silicon-enhanced data protection](https://oreil.ly/INn5W) for sensitive workloads, or even [confidential computing](https://oreil.ly/49QfJ) (if your context and data require it) with specialized VMs and operating systems. And of course, you could and should add [private links](https://oreil.ly/vMau-) and managed identities between building blocks (e.g., your data knowledge base and the selected LLM that connects with it) to carefully isolate your data movements.

## Use AI Infrastructure as an Opportunity to Connect Your Teams
