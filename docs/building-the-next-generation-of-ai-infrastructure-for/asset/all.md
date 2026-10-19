![](assets/cover.png)

![](assets/cover.png)

# Canonical

[![Promotional graphic for "Open Source AI at Scale," featuring collaboration between Canonical, Microsoft, and Intel to enhance AI models using Intel Xeon-powered Azure infrastructure via Ubuntu Pro.](assets/ad.png)](https://ubuntu.com/azure?utm_medium=organic&utm_source=whitepaper&utm_campaign=701N100000hbUhdIAE) 

# Building the Next Generation of AI Infrastructure

by Adrián González Sánchez

Copyright © 2026 O’Reilly Media, Inc. All rights reserved.

Published by O’Reilly Media, Inc., 141 Stony Circle, Suite 195, Santa Rosa, CA 95401.

O’Reilly books may be purchased for educational, business, or sales promotional use. Online editions are also available for most titles ([https://oreilly.com](https://oreilly.com)). For more information, contact our corporate/institutional sales department: 800-998-9938 or *corporate@oreilly.com*.

*   Acquisitions Editor: Megan Laddusaw
*   Development Editor: Gary O’Brien
*   Production Editor: Aleeya Rahman
*   Copyeditor: Penelope Perkins
*   Cover Designer: Susan Thompson
*   Interior Designer: David Futato
*   Interior Illustrator: Kate Dullea

*   May 2026: First Edition

# Revision History for the First Edition

*   2026-04-27: First Release

The O’Reilly logo is a registered trademark of O’Reilly Media, Inc. *Building the Next Generation of AI Infrastructure*, the cover image, and related trade dress are trademarks of O’Reilly Media, Inc.

The views expressed in this work are those of the author and do not represent the publisher’s views. While the publisher and the author have used good faith efforts to ensure that the information and instructions contained in this work are accurate, the publisher and the author disclaim all responsibility for errors or omissions, including without limitation responsibility for damages resulting from the use of or reliance on this work. Use of the information and instructions contained in this work is at your own risk. If any code samples or other technology this work contains or describes is subject to open source licenses or the intellectual property rights of others, it is your responsibility to ensure that your use thereof complies with such licenses and/or rights.

PROD: Uncomment the following sentence if appropriate and add it to the above para:This book is not intended as \[legal/medical/financial; use the appropriate reference\] advice. Please consult a qualified professional if you require \[legal/medical/financial\] advice. 

This work is part of a collaboration between O’Reilly and Canonical. See our [statement of editorial independence](https://oreil.ly/editorial-independence).

979-8-341-67306-9

\[LSI\]

# Prologue

I still remember, with a certain touch of nostalgia, the endless nights in my youth spent exploring the personal computer that my parents bought us after years and years of my imploring them. At that time, technology wasn’t necessarily accessible and that purchase required investing the equivalent of one month’s salary. Other times, but such good times…

I would spend those nights analyzing every single piece of hardware and software on the computer—an Intel Pentium III 500 MHz processor. It fueled my first video games and long sessions of functional analysis and testing to understand all the details of the operating systems. For some reason, I found a lot of pleasure in trying to replicate in my Linux distros some of the OS features I saw in Microsoft Windows 98. It was a technical challenge that fueled my curiosity and helped me find tricks all by myself, based on trial and error, equipped with manuals and books in the absence of the DSL connection I would get some years later.

But I think that one of the most romantic things was the ability to get Ubuntu and Kubuntu distributions by mail. At that time, Canonical sent the CDs worldwide for free, as a way to facilitate the adoption of open source at scale. Their ShipIt program was discontinued in 2011 (you can download the images from their website instead), but that episode of our lives was a powerful reminder of how important easy access to technology is.

Now everything is different, but I still find some similarities. What before was a Pentium is today an Intel Core or Intel Xeon processor. Instead of waiting some weeks for physical disks, I install ISO images on my laptop. And when I work on my professional projects, I deploy those Ubuntu images via Linux virtual machines (VMs) from my Azure portal. I’m obviously biased because I work with cloud platforms all the time, but the feeling of getting access to those resources with specialized hardware and my classic distros in a few clicks feels kind of magical, especially in these times of highly demanding AI innovations that require a mix of power and efficiency.

While I always appreciate the ongoing progress of the generative AI (GenAI) era, with a wider variety and more efficient language models (especially the kind of small language models \[SLMs\] that I run on my laptop), in my day-to-day life of AI architecture and product management I really need to push the limits of model training and inference at scale. Fortunately for my parents, they don’t need to help me get access to technology anymore. And even better for my partners and clients, they don’t need to spend endless nights or invest the company’s entire monthly revenue to implement their new AI applications.

I personally think that’s beautiful. Big and small companies accessing the same level of capabilities. Architects, developers, scientists, and engineers getting the tools to implement their projects, regardless of their physical location. Lower entry barriers, more access to innovation.

In a world craving good news and positive impact, that’s something important. And when I compare my early and modern days, while I cherish the good memories of the vintage era, I feel lucky to be part of a generation that is able to create more thanks to powerful technology stacks that, fun enough, combine exactly the same type of pieces that I used as a kid: computing power, operating systems, and skills.

Adrián González Sánchez

O’Reilly author, AI professional, and university lecturer

P.S. Thanks mum and dad, you made it happen.

# Chapter 1. Long Live the AI Infrastructure

The notion of *AI infrastructure* is not new, but it is becoming increasingly relevant for organizations around the world as they scale their level of AI adoption. Even in the era of pretrained GenAI, organizations still need solid platforms and specialized hardware. The lack of a strong architectural foundation is a major bottleneck for companies that don’t own their own bare metal environments. Additionally, the nature of the infrastructure changes over time, as it is not the same to *train* new machine learning (ML) or deep learning (DL) models, *fine-tune* existing large language models (LLMs), or move to production and *inference* your final AI deployments.

And the role of AI infrastructure is relevant even in times of a clear switch from only science activities to a mix of science and engineering to bring AI-enabled applications at scale. That mix keeps the best of both worlds, but it also includes considerations like *experimentation* being the norm in terms of new product creation, and *scalability* (whatever you want that to mean, depending on your context) to connect existing resources in a pragmatic, efficient, and powerful way. As an organic consequence of that, the nature of day-to-day activities has now changed. What before was about preparing and mining data sources, today is—for most organizations—an exercise in technical administration and API-enabled AI capability deployments that switch the attention to the classic development activities.

But neither AI nor the buzz around this industry are new, so what did exactly change? What is happening at macro and business levels that is causing you to feel the rush and need to adopt AI? And even more important, how can you make sense of it and get ready to plan in a structured, efficient manner? Let’s start from the beginning.

# The Business Landscape Is Changing and AI Is King

Yes, AI is here to stay. With bigger or smaller impact than the industry expectations, with more or less impact depending on the company and selected use cases, artificial intelligence (understood as the *umbrella term* for all things AI, from ML, DL, and natural language processing \[NLP\], to GenAI or even operational research and optimization engines, depending on the source) is impacting the way organizations think, sell, plan, and succeed.

Compared to other eras of technology excitement and disappointment (AI had its own dose during the so-called AI “summers” and “winters,” in which the initial attention turned into massive disinvestments and criticism), there is something tangible about the way companies are approaching AI. It is getting as real as accessing information in ways you could never have imagined, but also as a more concrete ability to automate formerly manual tasks with AI agents, and to transform user interfaces and experiences dynamically, in a way no technology could have done before. This means that both the core technology blocks and the business landscape are evolving at the same time. It is not only about adopting new models and pieces of infrastructure, but about leveraging the technical capabilities based on a totally new set of needs.

Think about this. We all work with command line and graphical user interfaces, but that’s been there for a while. The world was waiting for a new way to access information, and that’s already happening with tools like ChatGPT and Microsoft Copilot, among others. But take a look at [Figure 1-1](#ch01.html#fig-1-1), in which interfaces are merging with models and the notion of AI agents to enable *new user experiences*. Don’t you think what’s next looks both complex to achieve and exciting?

![](assets/bnga_0101.png)

And it’s not only about applications and processes. The nature of companies is also changing. We are entering the era of AI-first organizations, solo entrepreneurs equipped with AI-enabled software development tools, stock markets reacting to every single GenAI innovation, highly competitive model development, and an unstoppable cycle of new tool creation (e.g., OpenClaw).

But let’s focus on you for a bit. Not your company or the AI technologies, but on you as an adopter, leader, and decision-maker at an organizational and technical level. Depending on your background, you may have worked with AI before, or you may be currently exploring, implementing, and learning all at the same time. You leverage public cloud technologies, third-party models, and keep pace with technology innovations. How do you make sense of this highly evolving ecosystem? What are the key aspects that remain relevant regardless of that level of change? And how are you dealing with the increased level of pressure? Let’s talk about it.

# You Are Feeling the Pressure (and You Are Not Alone)

Yes, there is pressure: pressure to innovate, to learn, to adopt, and to do so in a way that makes sense for your company and your clients. But pressure is not necessarily bad when you can use it to fuel your initiatives and developments.

While any AI implementation will get special attention from stakeholders, the reality is that AI infrastructure and innovations with GenAI also attract sponsors and support that help you develop new AI-enabled projects and build skills within your team. This also gives you some leeway to prove the value proposition hypotheses with a bit of a leap of faith, which is more than what other types of projects get.

Now, the complexity is based on *balance*. The balance that, in the best case scenario, lets you implement your AI projects while setting the foundations for a reliable, scalable, and hopefully secured infrastructure. And those AI projects, understood as unitary pieces of technology, feed back into your overall structure by adding new technical capabilities and requirements. That is one of the main mistakes that AI adopters make: starting with a pilot, one-off mentality and forgetting to design a plan that converges with the entire AI strategy.

Now, take a few seconds before answering this: how do you do this in a way that fits your evolving list of needs and works together to deliver your strategic outcomes? And how will you and your team plan this in a way that allows you to scope and plan those activities in an increasing, iterating manner?

Well, let’s first understand the whats and hows of AI infrastructure. That should help you understand the basics before we plan an applied approach together.

# The Multiple Ways of AI Infrastructure

The word *infrastructure* means a lot of things. The Oxford English Dictionary defines it as the “basic physical and organizational structures and facilities needed for the operation of a society or enterprise,” and, in the case of AI, infrastructure is the foundation that supports your applications at baseline compute, secure operating systems, seamless connection with the data sources, specialized hardware for complex tasks, and software development levels.

While technology providers will have multiple views of what AI infrastructure means, you can visualize it as a set of interconnected vertical layers (see [Figure 1-2](#ch01.html#fig-1-2)).

![](assets/bnga_0102.png)

Concretely, these are the key layers you need to keep in mind while working on the underlying AI infrastructure that supports your new applications:

Baseline infra
: 

This means a bit of everything. It can be your own servers, a private cloud, or in the public cloud so you can quickly deploy with hyperscalers. It could be based on regular infrastructure offerings, or include [confidential computing](https://oreil.ly/dz3z4) or [sovereign solutions](https://oreil.ly/1TAeD) for highly specialized scenarios. It usually relies on physical IT elements but also on networking private connections between different cloud assets.

In the context of AI, we usually talk about specific [landing zones](https://oreil.ly/qPTJn) that contain the physical and logical resources necessary to host and interconnect all the required elements, as a baseline that comes before the rest of the layers. For example, you can review examples of [AI landing zones](https://oreil.ly/1UU48) as well as [repositories](https://oreil.ly/aaDMS) with predefined templates (e.g., Bicep, Terraform) to deploy and replicate easily while scaling your AI implementations.

Processing power
: 

This is probably one of the most important layers (or at least the one that gets a lot of attention) of the modern concept of AI infrastructure. Processing power takes the form of classic and new acronyms such as CPU (central processing units that enable floating point arithmetic), GPU (graphic processing units optimized for matrix multiplications), PTU ([provisioned throughput units](https://oreil.ly/pSwFu)), NPU (neural processing units for optimized and more efficient processing of neural network operations), and LPU (language processing units adapted to specific LLM operations). In general terms, which one you choose depends on the stage of the AI lifecycle (e.g., training requires more computing power than inference) and the technical approach depending on your providers’ offering.

Specialized hardware like [AI-oriented CPUs](https://oreil.ly/U6xVf) and accelerators are designed for high-performance training and inference. The bridge between this silicon and the software stack is critical; enterprise-grade Linux distributions now incorporate specialized drivers and toolkits directly into their [official repositories](https://oreil.ly/kgW-w). This ensures software provenance and proactive security patching without relying on third-party repositories.

Operating systems
: 

As with my first personal computer (check the [Prologue](#preface02.html#prologue) to understand the reference), any AI development must connect with systems such as VMs and containers that allow you to scale your applications. This includes VMs that use popular open source operating systems like Ubuntu. For enterprise workloads, you need to select OS flavors that enable security, maintenance, and access to relevant software and AI libraries. Maintaining a standardized OS experience across environments ensures that an AI stack developed on a laptop runs with high fidelity in an Azure VM or an on-premises cluster.

Data foundations
: 

Expanding from the baseline infrastructure, any modern AI infrastructure leverages data layers that may include relational databases like PostgreSQL and MySQL (both of them classic open source options), NoSQL engines like Cosmos DB, and also *lakehouse* platforms like Databricks, Snowflake, or Microsoft Fabric. Beyond regular data architectures, these foundation elements are now designed to converge with the rest of AI elements, such as feature stores and experimentation platforms. This layer also incorporates specialized solutions like vector databases, including MongoDB vCore or PostgreSQL with the pgvector extension, to manage the high-dimensional data required for retrieval-augmented generation (RAG).

Development and modeling
: 

The software development layer sits on top of the baseline infra, relies on the operating systems, and certainly connects with the data and AI model portions. And the AI modeling part can rely on visual platforms and API endpoint management engines to facilitate the access to existing and newly created models. For example, public cloud AI platforms like Microsoft Foundry, which runs on Ubuntu, include the notion of catalogs for both AI models and external tools and servers. In these platforms, the processing power layer adapts in a flexible manner to the requirements that come from these development and modeling activities. And this layer relies heavily on the highly evolving list of libraries and frameworks that support any AI and agentic implementation, including classics like Keras and PyTorch, or LLM-oriented open source frameworks like Model Context Protocol (MCP), Agent-to-Agent (A2A), LangGraph, and OpenClaw.

Platform
: 

The final layer is really just an extension of all the others. It includes everything from engineering to model and agent orchestration, tracing and monitoring, and even some content-safety filters. This last layer depends on the building blocks you use. For example, it might look different if you rely on a general platform as a service (PaaS) like Microsoft Foundry or on AI-first operating systems and development platforms such as Canonical’s [AI suite](https://oreil.ly/0jaFO).

Obviously, keep in mind that some additional elements are present in all the layers, as should be the case for your security measures (you can see more details in Chapter 4, “Securing Open Source and AI Developments,” of my [*Linux and Open Source Software in Azure*](https://oreil.ly/P5hwB) report (O’Reilly), in which you can add different security measures for each infra layer).

That said, the costs associated with building top-notch AI infrastructure with all these layers are often perceived as expensive, time-consuming, and skills-intensive. However, it all depends on the implementation approach and how you adapt it to your company context, including your available talent and financial resources. We will explore potential approaches later, but regardless of the choice, there are a few elements you always need to keep in mind. Let’s explore them.

# At the End of the Day, It’s All About Foundations

You can see the AI infrastructure planning exercise as a multidimensional one. While you want to optimize all of the infra layers to get the most and best out of them, you also need to keep in mind the foundational requirements that will enable you to do so at scale, including when facing highly demanding and complex scenarios.

[Figure 1-3](#ch01.html#chapter_1_figure_3_1775793356868007) shows the key elements that will serve as foundations for any AI infrastructure planning. You can see this figure as pillars supporting that infra, or even use it as a checklist to validate requirements when you and your team design the next stage of AI infrastructure and architecture.

![](assets/bnga_0103.png)

We are not reinventing the wheel with these pillars, as they are the technical foundations for most of the modern architectures out there. However, in the context of AI infrastructure, this is what you should check and improve over time (feel free to use it as your personal checklist):

*   *Performance*. From an AI perspective, performance means getting all the layers together so they work well for a specific purpose. In quantitative terms, you need to pay attention to two key aspects:
    
    *   *Operational performance* is related to how your AI infrastructure behaves in terms of latency and perceived performance from the response time and other elements, which in reality will rely on the rest of the items in this checklist. For this to work, you obviously need high performing baseline infrastructure, processing power, and access to scalable data and AI assets such as databases and models.
        
    *   *AI model performance* focuses on the actual, quantitative performance of the AI models. Related to MLOps and LLMOps/GenAI metrics such as accuracy, precision, recall, groundedness, relevance, or coherence (depending on the type of model), it will illustrate how good or bad an AI model is in a specific situation.
        
*   *Scalability*. From an AI infra point of view, scalability is key to enabling your company’s and application’s growth. There are two dimensions:
    
    *   *Geographic scalability* is the ability to replicate or expand your AI infrastructure depending on your business and user activities, with a minimum burden on and convergence with your existing deployments.
        
    *   *Scope scalability* is the way your AI infrastructure serves as an evolving canvas in which you can add new features and capabilities without requiring a high level of investment that surpasses the actual value of your AI.
        
*   *Efficiency*. One of the key goals of an optimal AI infrastructure is to make the most of it with the minimum amount of resources. For example, your goal might be running a model that is optimized to consume fewer resources during the inference phase, or adding new elements to the modeling, development, and platform layers that help reduce the hardware requirements without impacting the quality of the AI outcomes (e.g., Microsoft’s [LoRA](https://oreil.ly/l_4oo) or Low-Rank Adaptation of Large Language Models). Solutions like automated model routers that choose the model depending on the nature of the task and distilled smaller models (those generated from the general knowledge of bigger models) help address efficiency.
    
*   *Cost*. The cost of AI infrastructure can be split into direct and indirect costs, often related to the concept of [total cost of ownership (TCO)](https://oreil.ly/X9AEK):
    
    *   *Direct cost* is the sum of the costs of all AI infrastructure layers and elements. The main chunk of the calculation will focus on the processing power layer, as CPU and GPU investments are costly and deeply impact financial operations, technical performance, and memory hardware (e.g., RAM) due to supply and demand. Both rely on physical, often scarce materials whose prices are highly sensitive to general adoption cycles. Software costs are also part of the direct cost equation.
        
    *   *Indirect cost* includes the expenses related to the initial setup, continuous maintenance, and evolution of the system. For example, ordering a physical server, shipping it to your location, and connecting it to your infrastructure incurs different costs than scaling up via new cloud deployments. From an AI perspective, you need to add these costs to make everything production-ready and at scale.
        

With the cost factor, there is also the consideration to analyze and decide on operational versus capital expenses, often known as the OPEX versus CAPEX dilemma, which impacts your financial operations and investment strategies.

*   *Flexibility*. Compared to the scalability requirement, flexibility refers to how well your AI infrastructure context allows you to adapt to changing needs and conditions, including peak usage of your AI-enabled applications.
    
*   *Reliability*. From an enterprise perspective, your business runs on trust. And that trust depends not only on what you do with your AI infra, but also how that outcome performs over time with no failures or interruptions. Reliability also depends on your scalability and flexibility aspects, as you want to have access to specific capabilities (e.g., model throughput) over time.
    
*   *Availability*. Availability is related to reliability but is a different concept. Technical and commercial requirements may dictate service level agreements (SLAs) that require your AI applications to be available and accessible for a specific percentage of time. This is important in a multilayer approach that combines pieces from your organization, but also when there are multiple providers that need to work together.
    
*   *Security*. In the AI world, security is split into two concepts. Concretely, the AI infrastructure will include two elements:
    
    *   *AI security* is related to traditional security aspects like perimeter and access control, and protection against attackers and vulnerability exploits. It also includes software supply chain security to ensure that all proprietary and open source components are patched and verified. By consuming elements like [inference snaps](https://oreil.ly/lhLPs), organizations get a trusted, immutable way to deploy models along with their optimized runtimes. This ensures the entire environment, from the kernel to the bundled AI engine, receives comprehensive patching.
        
    *   *AI safety* focuses on content protection measures, which are implemented to avoid misbehavior. These measures can be added at the development and modeling levels. Furthermore, with the rise of autonomous AI agents capable of modifying files and executing commands, security must also address the blast radius of potential errors. By leveraging system containers or VMs to sandbox these agentic workflows, architects can ensure that an agent’s potential mistakes or compromised dependencies remain strictly isolated from the host operating system.
        

We will explore more security and safety considerations in [Chapter 2](#ch02.html#ch02) as part of the full stack approach. You will understand how the choice of AI infrastructure will directly impact the way you approach security and safety as well as compliance and certifications; these topics will be critical for your enterprise-grade activities.

# Chapter 2. Compliance, Security, and Sustainability Are Not Optional Anymore

One of the main concerns of AI adopters and learners is the difficulty of keeping up with all the constant news and technology advancements. This includes the constant feed of AI models reaching the first positions of the technical benchmarks, new startups being created every week, the endless release of promising AI platforms, and the myriad protocols and frameworks that take over the social media feeds on a weekly basis.

With that evolving context, it is easy to feel confused and even overwhelmed. However, there is some good news you may want to hear, as it will impact how you implement and evolve your AI infrastructure:

*   Your AI technology stack and the overall strategy don’t rely on specific models or product announcements. They should and will be solid enough to observe all these developments and add relevant pieces when—or if—they are necessary. For example, if there is a new framework (e.g., A2A, MCP, Microsoft Agent Framework) and your team wants to adopt it, you may choose to test it first and then decide if you want to incorporate it into the development and platform layers.
    
*   Some pieces of the *AI puzzle* are already there. Some years ago, you may have had to implement your AI infrastructure and related projects without certainty of what AI compliance or even proper security meant. That has changed a lot during the last few years. And that is good news for enterprise-level AI adoption. You need to make sure that any AI infrastructure investment will be aligned with your internal and external context, including new and existing regulations and standards.
    

Because of these two aspects, you will need to define an AI infra approach that incorporates both *risk-based controls* and *continuous improvement* measures, and that leverages existing resources that become part of your infrastructure layers but also provide additional value for your security and compliance activities.

# Everything You Need to Think About

So, the question here is: how do you identify the key considerations you need to incorporate into your technical decision-​making⁠ process? And how can you actually make the most of the compliance and security requirements to convert them into a differentiator? Here are some recommendations to balance innovation and enterprise-grade requirements while evolving your AI infrastructure.

## Align Your Approach with the Pace of Innovation

Here is the thing. The pace of innovation will remain fast and sometimes overwhelming, no matter what. By using the layered approach from [Chapter 1](#ch01.html#ch01), you can at least structure your AI stack and clearly distinguish between what already exists and what’s newly possible at each layer. For example, you might identify the need to boost processing power by upgrading CPUs or GPUs, or to quickly test a new industry framework or AI model before deciding whether to adopt it.

## Keep It as Pragmatic as Possible

I wouldn’t go so far as to describe the AI infrastructure and its layers as a delicate house of cards, but it is true that choosing overly complex approaches will make your architecture and consolidation exercise a bit more difficult. Your choice is fully yours, because you will need to analyze your existing layers and check what can be simplified, improved, replaced, or expanded. For example, if you have part of your infrastructure on-premises and your level of capital investment to buy more servers cannot catch up with the requirements of your business, you may choose to expand via the public cloud. And if your team is becoming a bottleneck for maintenance and support of your physical servers, you may decide to migrate to the public cloud and modernize part of your base infrastructure and processing power layers.

If you want to see how cloud migration enables quicker AI innovation, feel free to explore my [*Cloud Migration and AI Readiness*](https://www.oreilly.com/library/view/cloud-migration-and/9781098179557/) report (O’Reilly), which explores how a cloud-first strategy can accelerate your overall level of AI maturity, not only at the technical level.

## Leverage and Make the Most of the Increasing Compliance Requirements

Compliance is often perceived as an additional exercise, something to be done after the fact. Some people even see it as a burden, or a barrier to innovation. But the reality is that compliance is a fairly good step toward generating trust (with partners, clients, and users) and self-certainty. Basically, it means using general rules and best practices as guidance to make sure you are doing the right thing.

Concretely, you can explore in [Figure 2-1](#ch02.html#chapter_2_figure_1_1775793357024448) the compliance building blocks and select those that are most relevant for your AI infrastructure and use cases.

![](assets/bnga_0201.png)

The blocks are:

Management systems
: 

International entities like the International Organization for Standardization (ISO) define a management system (MS) as “the way in which an organization manages the interrelated parts of its business in order to achieve its objectives.” From a compliance perspective, standards like ISO 9001 for quality, ISO 14001 for environment, and ISO 27001 for security topics, are classics that impact any organization at a transversal level. However, the most relevant standard for your AI initiative is ISO 42001, which focuses on AI Management Systems (AIMS).

From an AI infrastructure perspective, ISO 42001 can bring you an iterative approach to continuously improve the specific scope of your infra from a product or service perspective. For example, if you want to certify your AI governance and risk management work for a specific software as a service (SaaS) product, you can plan requirements and audits to pass the ISO 42001 certification, in the same manner as the big technology companies are already [certifying their products](https://oreil.ly/jjE6i).

Applicable regulations
: 

If we simplify the regulatory compliance exercise required for AI infrastructure layers, the main—and most straightforward—requirements are related to your data foundations and AI modeling activities. Currently, that translates directly to existing data protection laws and incipient AI regulations.

Without going into the details of cases like the EU AI Act, GDPR, CCPA, or recent executive orders related to AI, the key idea here is to connect your AI infra with specific requirements, which can then be converted into traces and logs you can keep automatically or manually. For example, saving concrete logs or files of the decision making of an AI agent that sits on top of your infrastructure requires tracking and saving every single step of the agent, including prompts, results, AI model versions, related data stores, endpoints, etc.

Vertical industry requirements
: 

As with general data and AI regulations, you need to align your AI infra layers to specific standards and regulations. For example, your AI infra may need to include [Fast Healthcare Interoperability Resources (FHIR) or Digital Imaging and Communications in Medicine (DICOM) standards](https://oreil.ly/TQ016) into the data foundations layer, but also adhere to regulations like the Health Insurance Portability and Accountability Act (HIPAA) in the United States. If you work with the US government, you may need to map requirements for the Federal Information Processing Standards (FIPS), the Federal Risk and Authorization Management Program (FedRAMP), or the Defense Information Systems Agency Security Technical Implementation Guide (DISA STIG).

All this can be achieved by finding specific elements for your infra layers, such as operating systems that include [compliance offerings](https://oreil.ly/I2yTh) in their professional versions or processing units with specific [trust features](https://oreil.ly/OE7M8).

Security essentials
: 

When planning your AI infrastructure, it is critical to take security seriously. While AI systems expand the attack surface and introduce new layers of risk, an approach driven purely by worst-case assumptions can lead to systems that are overly complex, inefficient, or even unusable. Instead, you could address AI security as a risk-based discipline, where decisions are guided by the balance between potential impact, likelihood of threats, and operational needs. Leaders like you need to plan with worst-case scenarios in mind to understand the boundaries of risk, but you need to execute based on informed trade-offs that align with your organization’s risk tolerance.

The expanded surface of attack requires a layered and pragmatic security strategy that reduces meaningful risk while maintaining system functionality and performance. This may include:

*   Securing critical layers of the AI infrastructure based on their risk exposure
    
*   Protecting access points and enforcing strong identity and access controls
    
*   Preserving data integrity across training, inference, and storage pipelines
    
*   Continuously assessing and addressing code and dependency vulnerabilities
    
*   Understanding and mitigating model-specific risks (e.g., prompt injection, data leakage)
    
*   Relying on trusted data sources and validating external inputs
    

As mentioned earlier, you can review Chapter 4 in my [*Linux and Open Source Software in Azure*](https://www.oreilly.com/library/view/linux-and-open/9781098178284/) report (O’Reilly) to see an applied example of layered protection strategy.

Supporting frameworks
: 

Last but not least, leverage industry frameworks such as the NIST Artificial Intelligence Risk Management Framework (AI RMF 1.0) for artificial intelligence risk management, or Data Management Association (DAMA) and Data Management Capability Assessment Model (DCAM) for data governance and management in your foundations layer. Rather than just understanding the framework, focus on what really matters from an AI infra perspective. What is the role of each layer in generating and providing the important information, such as data quality controls for AI or specific risk mitigation strategies from a software code perspective?

In general terms, try to create an inventory of the technologies that are part of your AI infrastructure, and then visit the [compliance and trust centers](https://oreil.ly/MZ3-T) or specific product descriptions for those technologies. And don’t forget to plan your compliance exercise with an auditor mindset, in which your team(s) can test your AI infrastructure against specific requirements.

## Treat Data as Your Most Valuable Asset

While AI models and capabilities can become commodities (to some extent), your data is yours and it is part of your core intellectual property. Besides compliance and security reasons, your key motivation from an AI infrastructure perspective is to make sure that regardless of where your data comes from (where it resides), it always remains yours. Imagine that data journey among infra layers, and try to track the virtual path that goes from your data foundations, to your code and modeling layers.

For example, you can explore the wide range of CPUs and GPUs, and look for [silicon-enhanced data protection](https://oreil.ly/INn5W) for sensitive workloads, or even [confidential computing](https://oreil.ly/49QfJ) (if your context and data require it) with specialized VMs and operating systems. And of course, you could and should add [private links](https://oreil.ly/vMau-) and managed identities between building blocks (e.g., your data knowledge base and the selected LLM that connects with it) to carefully isolate your data movements.

## Use AI Infrastructure as an Opportunity to Connect Your Teams

The infrastructure work is part of the technical pillar that supports your organization’s overall AI maturity. That maturity often relies on business and AI strategy, technology foundations, AI experimentation and experience, and general governance, organizational, and cultural aspects. But these pillars are not independent. Your general culture can influence how you implement your AI infra, and your infrastructure choice can impact the way people upskill, experiment, and adopt.

The good thing is that defining, implementing, and improving the multiple layers of your AI architecture over time can serve as a lever to enable collaboration and communication among stakeholders. For example, assuming you are leading the technology workstream, you may explore synergies with:

Legal, risk, and compliance departments
: 

For obvious reasons, these departments will be very interested in understanding the nature of the AI infrastructure and the direct impact on their activities. Regardless of your technical role, you can serve as a translator between technology assets and their actual requirements. For example, if these departments want to understand the technology readiness in terms of California Consumer Privacy Act (CCPA) compliance for activities on the West Coast, you can share with them the list of cloud-enabled products and services you are using and the [related documentation](https://oreil.ly/zEdIt) and policies, which they can review and then leverage for their internal and external audits.

Enabling this kind of collaboration and defining a mix of manual and automated processes will not only make your technical activities more efficient, but also create a more robust and trustable structure.

Financial operations (FinOps) and revenue
: 

Based on the best [FinOps practices](https://oreil.ly/To6qR), you can collaborate with finance stakeholders to proactively plan the infrastructure investment (for all layers), and engage with product and revenue departments to maximize the financial impact of those investments (e.g., by splitting the total cost for a specific set of premium clients that pay for the top tier of your SaaS, then analyzing the AI-related cost and its impact on margin and pricing).

The long-term game will allow you to make your AI deployments more sustainable and to have better control of the financial input and output of your infrastructure investments.

Human resources
: 

Defining the technology stack is a complex exercise: the specific infra choice will impact the entire set of hiring and internal skilling activities. While HR departments can own the overall goals, you can guide and share specific topics and skills that will support your AI infrastructure activities. For example, granular topics like deploying processing power at scale in one specific cloud provider, securing both networking and operating systems, and analyzing functional and compliance requirements to choose the best solution can be accelerated by providing specific people with concrete pieces of knowledge (e.g., [cloud certifications](https://oreil.ly/GjfyQ), ad hoc trainings, internal knowledge transfer).

Partnerships
: 

This area is also very interesting for any AI adopter. By collaborating with your organization’s partnerships team, you can share the technical signals that will guide them to explore specific B2B agreements, and to negotiate discounts if you anticipate higher tech usage and cost (e.g., purchasing professional OS licenses at scale, negotiating committed payments for reserved instances of processing power).

This exercise will impact other financial and skilling activities, as you will create connections with the technology providers; you will also facilitate and maybe even accelerate your AI infrastructure deployments and evolution by anticipating the needs and providing your colleagues with the relevant pieces of information.

Besides these examples, you can obviously consider everything within the technology area. If you are working on engineering activities, and depending on the size and type of organization, you may have other departments for core IT, security, and data activities.

Due to the technical nature of the activities, planning the AI infrastructure deployments as a multidisciplinary exercise (maybe in the form of a technical committee) will help you bring all the perspectives together and close potential gaps, which result in more complete, thoughtful, and holistic innovations.

## Plan the Evolution in Sprints

As with any other iterative project, and keeping in mind the complex nature of AI infrastructure and use cases, you will need to plan your initiatives in sprints. This means scoping and implementing in periods of two or three weeks, which allows you to track progress and review any issues or needs that may arise.

Regardless of who owns this planning (e.g., AI team lead, project manager), the ability to scope and manage AI initiatives is critical for you to not only continue evolving your AI infrastructure but also to connect with ongoing AI projects that will leverage and help you adjust the infrastructure approach, sprint by sprint, and project after project.

If you want to understand how to structure your AI initiative, check out my book [*Managing AI Projects*](https://www.oreilly.com/library/view/managing-ai-projects/9798341641006/) (O’Reilly) and start applying hybrid project management techniques to your infra work.

As you can see, leveraging the contextual requirements, along with the specific compliance and security needs, will feed not only your AI infrastructure activities but also your interaction with multiple departments and the way you handle day-to-day operations. But there is one more dimension: making everything efficient and sustainable in the long run. Let’s explore this aspect separately, as this topic impacts several areas of activity.

# Sustainability Requirements Will Become Even More Important

Sustainability in the AI ecosystem means a lot of things. In a world with increasing needs in terms of AI infrastructure and capacity, in which technology providers open new datacenters to power their AI models and to sustain your company innovations, finding sustainable approaches to AI infrastructure makes everything more feasible at physical, financial, and social levels.

From datacenters with [zero water consumption](https://oreil.ly/_ytm9) to an increasing level of efficiency at the processing unit level for [lower power consumption](https://oreil.ly/BJ1F9), sustainability is a must for all companies, including yours. And, of course, sustainability goes beyond regular corporate social responsibility (CSR) to land directly at the technology discussion level.

Look back at the layers of your AI infrastructure stack in [Figure 1-2](#ch01.html#fig-1-2). Any potential improvement you can make in any one of them will impact the rest of the infra mix. One of the clearest examples is to choose new LLMs or SLMs (from your modeling layer) that prove to be less resource intensive, as they will accomplish the same type of task with similar performance, while reducing the impact on the processing power and base infrastructure layers. Another example is toolkits like the previously mentioned LoRA that can help you achieve inference at scale while reducing hardware requirements, which, at the end of the day, is good for you, but also good for the planet.

While this report is not intended to be a monograph on the topic, please keep sustainability in mind while building the multiple layers of your AI infrastructure. And now, let’s go on to the next chapter, in which you will explore the trade-offs and decision-making process for your AI infra.

# Chapter 3. There Are Multiple Factors

Up to now, you have gotten a mix of industry context, layered approach, tangible checklists, and recommendations to approach your AI infrastructure exercise. But nothing in life is absolute and you will need to face some trade-offs and deal with unclear decision paths.

In this chapter, we will try to bring a new dimension to your decision-making process—specifically, all the variables that will help you adjust your AI infrastructure to your company context. Let’s start with the notion of lifecycle, as that will provide a structured scope to your AI developments.

# Think in Terms of AI Lifecycle

Imagine the AI infrastructure as that set of interconnected vertical layers, stacked one after the other, like you saw in [Figure 1-2](#ch01.html#fig-1-2). The notion of the AI lifecycle can bring you a two-dimensional approach, because you can organize it as a horizontal sequence that defines your AI developments.

The notion and details of an AI lifecycle depends on the author. You have technical lifecycles like the classic [Team Data Science Process (TDSP)](https://oreil.ly/T21_M) or compliance-oriented versions like ISO 5338 or NIST AI. Ultimately, regardless of the version you choose, those lifecycles typically follow a more or less granular sequence of ideation, data preparation, AI modeling, testing, and deployment activities. Some of them include continuous post-production monitoring and the final decommission of the AI application.

For the sake of simplicity, we will define five simple AI lifecycle stages (feel free to make yours as granular and complex as you like), and showcase in [Figure 3-1](#ch03.html#chapter_3_figure_1_1775793357121277) the relative importance of the different AI infrastructure layers, depending on the stage. This is useful for you to plan incremental upgrades to your infra or even to anticipate specific needs.

![](assets/bnga_0301.png)

While this figure is not intended to be 100% accurate, it is a good way to visualize specific AI lifecycle moments for your applications and projects, in which pieces like processing power will be very important for data and AI modeling activities, or the development and platform blocks are very important during the testing and deployment phases. Adapt this model to your specific project needs and current level of AI infrastructure maturity.

# Application-Level Considerations

There are additional considerations that go beyond the core infrastructure layers and your typical AI lifecycle. Those considerations are related to the applications you are building that are supported by that AI infrastructure, while keeping in mind their functionalities, interfaces, etc.

For example, if your application relies on processing power or prebuilt LLMs, you need to understand the usage patterns and projected scenarios. Serving users for a reduced, initial set of B2B clients is not the same as launching a B2C service that can potentially become widely adopted. From the AI infra perspective, if you expect a lot of users, and those users interact with your applications via prompts several times per session, and each interaction consumes a specific number of tokens, then that will directly affect the amount and type of processing power and AI resources your application will need.

Also, the way you evolve your user interfaces will impact what kind and how many resources you need to dedicate to your application. Remember [Figure 1-1](#ch01.html#fig-1-1) from [Chapter 1](#ch01.html#ch01). You can create, for example, a multimodal application that supports text, image, audio, and video inputs. Or you can have a generative avatar that simulates interactions between users and virtual agents. All these components will have different considerations and an impact on the different layers of the AI infrastructure.

And last but not least, we cannot forget the notions of AI security and safety. While the difference and connection between these terms is often unclear due to their continuous convergence, you can see AI security as a domain related to traditional cybersecurity aspects that are concerned with perimeter protection, access control, and general monitoring of your AI-enabled systems. On the other hand, and simplifying a bit, AI safety can be defined as the mix of activities that not only protect your systems against direct and indirect attacks but also monitor for potential threats and negative usage and topics (e.g., if you release an AI application for parents and kids, you probably want to add violence and sexual filters to avoid those topics by design). These considerations will impact your development and platform layers, and they are not an afterthought but rather an important design element for any AI application and its underlying infrastructure.

# Operating Systems for AI

Another important aspect is to decide on the OS layer based on a variety of considerations. For example, you’ll need to decide between proprietary or open source, free versus pro editions for enterprise support, and full versus lightweight implementation depending on the workloads. When deploying in the public cloud, it is also important to select an OS that provides images specifically optimized and certified for that platform. This often involves using a tuned kernel that allows the system to better leverage the provider’s underlying hardware and networking, ensuring the stability and performance required for intensive AI processing.

But it is equally important to choose your operating systems based on the ability to support your AI productivity suite. For example, you can leverage a stack that includes data processing tools like [Charmed Spark](https://oreil.ly/P6-ez) for your AI projects. For AI model deployment, an OS compatible with silicon-optimized inference snaps simplifies the process; a single command can detect host hardware and deploy the best-fit model variant along with an optimized toolkit such as OpenVINO. And, of course, that OS layer connects and interoperates with the rest of the layers, but especially with the processing power from CPUs and GPUs, to enable AI implementation teams to first experiment and then bring AI developments to production.

# Skilling to Expand Your Infrastructure

Coming back to the notion of AI maturity and readiness we briefly mentioned in [Chapter 2](#ch02.html#ch02), and keeping in mind that culture and skilling are fundamental pillars for any AI initiative at the company level, the AI infrastructure piece can guide some of your activities.

Imagine leveraging the infrastructure layers as some initial guidance to map required versus existing skills. If you realize that your team is planning to use a specific kind of AI framework, or libraries that connect to the physical processing hardware, you may want to consider including this as part of your internal skilling and external hiring activities.

One interesting asset is the AI skill matrix you can find in the [*Agile AI*](https://www.oreilly.com/library/view/agile-ai/9781492074984/) report (O’Reilly), which allows you to evaluate required project-level skills (you can aggregate them at company level if you have different types of projects), and then map them to specific activities and related pieces of your technology stack, tools, and infra layers. With that continuously updated view of what your teams need in terms of knowledge and skills, you can then redirect these topics to the specific HR and training stakeholders in your organization.

# Exploring New Scenarios

Last but not least, and even if we pointed to these topics in the first two chapters, you may want to explore advanced scenarios enabled by confidential computing, sovereign clouds, and hybrid infrastructure. Depending on the nature of your workloads, type of data, contractual requirements from your own clients, and industry context (especially for highly regulated verticals), these scenarios will require a deep dive and an evolution of your infrastructure layers.

For example, if you need to leverage the latest public cloud AI features while keeping part of your environment in your local premises, hybrid infrastructure is the way to go. If you need to protect hyper-sensitive data and you want to decentralize key management, you may choose confidential or [sovereign offerings](https://oreil.ly/QUBbU) from global providers, so that the requirements are adapted to your local geography.

Your final choice will impact not only the what, but also the how—how you connect the dots and implement your applications (e.g., the way to connect to data or to secure your system if there is an independent third-party managing your secrets and keys). Fortunately you can leverage interoperable platforms that work consistently on any cloud or environment, such as deploying containers via Kubernetes for workload portability or leveraging AI models that are available in multiple providers.

For any of these scenarios, try to explore available options with your cloud and technology providers and expand to new solutions if you cannot get access to these capabilities with your regular infrastructure, processing, and OS solutions.

# Conclusion

As you have seen throughout this report, building AI infrastructure is not a linear or isolated technical exercise; it is a highly evolving discipline that combines strategy, engineering, governance, and collaboration.

Every layer of your technology stack, from baseline compute to platform, influences the others. Every decision, whether about processing power, operating systems, compliance, or sustainability, carries trade-offs. And every trade-off is an opportunity to shape an infrastructure that not only supports your current AI ambitions but scales with everything that will come next.

The real advantage lies not in assembling the perfect architecture from day one but in adopting a structured, iterative mindset. By thinking in terms of lifecycle, embracing cross-functional collaboration, and grounding choices in both business and technical realities, you create an ecosystem that can continuously adapt by incorporating new models, new frameworks, new compliance requirements, and new expectations from your users and stakeholders.

At the end of the day, AI infrastructure is about enabling possibilities for your teams: the possibility to experiment faster, deploy safer, innovate responsibly, and reach more people with applications that feel natural, intuitive, and powerful. With a clear view of the layers, foundations, and decision-making frameworks at your disposal, you are not just keeping up with the pace of AI but also shaping how your organization grows with it.

Repeat after me: your next steps are not about chasing every new technology announcement but about using the clarity you’ve gained to evolve with purpose. Build layer by layer, sprint by sprint, and decision by decision. With the right foundations, the trade-offs become manageable, the risks become navigable, and the opportunities become truly transformative.

The journey continues, and every single piece will evolve and change over time, but now, you are ready for it. Go and start your AI infrastructure exercise.

# About the Author

**Adrián González Sánchez** is a product manager, AI and search, at Microsoft AI, an academic director at IE University, and an author of multiple O’Reilly and Packt books and reports.

He has previously worked for multiple AI companies in Europe and North America. He is part of several international initiatives, including the Spanish Observatory of Ethical and Social Impact of AI (OdiseIA) and The Linux Foundation, as well as the expert group for the EU AI Act Sandbox with the Spanish Government in Europe.

Adrián is a trainer for the École des Dirigeants at HEC Montréal and Concordia University in Canada, and he has released multiple online courses with O’Reilly, LinkedIn Learning, and The Linux Foundation. He also collaborates with 2U/GetSmarter as a tutor for MIT Sloan’s AI executive courses.

Adrián’s areas of expertise include AI leadership and project management, responsible AI systems implementation, data and AI governance and compliance, and cloud computing.

If you’d like to read more of Adrián’s work, here is a list of O’Reilly publications to check out:

Books
: 

[*Azure OpenAI Service for Cloud Native Applications*](https://learning.oreilly.com/library/view/azure-openai-service/9781098154981) by Adrián González Sánchez

[*Generative AI on Microsoft Azure*](https://learning.oreilly.com/library/view/generative-ai-on/9798341623279) by Adrián González Sánchez, Jaime De Mora, and Jorge García Ximénez

[*Kubernetes and Cloud Native Associate (KCNA) Study Guide*](https://learning.oreilly.com/library/view/kubernetes-and-cloud/9781098138936) by Jorge Valenzuela Jiménez and Adrián González Sánchez

[*Managing AI Projects*](https://learning.oreilly.com/library/view/managing-ai-projects/9798341641006) by Malini Jain Runtasewee and Adrián González Sánchez

Reports
: 

[*Cloud Migration and AI Readiness*](https://learning.oreilly.com/library/view/cloud-migration-and/9781098179557) by Adrián González Sánchez

[*Linux and Open Source Software in Azure*](https://learning.oreilly.com/library/view/linux-and-open/9781098178284) by Adrián González Sánchez