
YourFinFreedom intended to move between architectural styles, from a monolith to microservices, depicted as line 1 in [Figure 6-4](#ch06.html#example_of_accidental_complexity_when_m). However, without a clear direction guided by metrics, they’ve ended up following line 2, which has led them to a distributed big ball of mud. How can Anna start leading things back onto the right path?

![](assets/sarm_0604.png)

# Seeking Direction

*Anna’s promotion to Solutions Architect is announced, and she sets to work. To better understand the company’s direction, she begins holding conversations with people outside her department in all sorts of business functions. She wants to see her colleagues’ day-to-day challenges and how they relate to the big picture of the software that her department creates and maintains.*

*In one such conversation, she learns that YourFinFreedom has recently tuned its company KPIs to be better aligned with its mission: “Making financial services accessible to everyone, anywhere.” From the company’s general updates, Anna also knows that the C-suite steers activities based on earnings before interest, taxes, depreciation, and amortization (EBITDA) and customer lifecycle value. She’s intrigued by the organization’s new KPI, monthly active users (MAU), which measures how many users interact with the company’s services and/or products in a month. This KPI creates a feedback cycle from all initiatives (marketing, sales, product features) to engage customers. Anna starts to wonder: how is this connected to software architecture?*

Software exists to solve complex problems within a specific context. As you just read, Anna discovers that YourFinFreedom has an important KPI next to the financial KPIs at the organizational level ([Figure 6-5](#ch06.html#yourfinfreedomapostrophes_organizationa)). In real life, some version of an MAU KPI is widely used by social media companies, as well as companies that have a mobile-first approach to the market. With the context of a scale-up, MAU allows a company to steer growth using capabilities such as marketing, sales, customer contact center, or the features in the product or service.

![](assets/sarm_0605.png)

An organization’s software architecture needs to be connected to its mission and KPIs. If the KPIs are not well-defined or people don’t understand them, the software architecture is at odds with the KPIs. This is a prime example of Conway’s Law, illustrating how communication affects the software architecture. Software architecture exists to create boundaries between the responsibilities of different software components and define their interactions. It should respect the current constraints of the organization and the surrounding environment (technology, people, regulations, the market, and so forth) and at the same time enable the business to be agile.

# From Best Effort to Intentional Effort

*In reflecting on stakeholders’ requests and the product’s current architecture state, Anna notices a pattern. Everyone is working on a “best-effort” basis, trying to fulfill all requests for software to the best of their abilities. She realizes that an important quality of software architecture is missing in the teams: their decisions should be intentional.*

*Anna reaches out to different software communities to learn how others are tackling similar issues. She learns about EventStorming, a “flexible workshop format for collaborative exploration of complex business domains”^[5](#ch06.html#ch01fn56) used widely in the Domain-Driven Design community to visualize processes and reason about boundaries and metrics. EventStorming, she learns, starts by giving a big picture of the context across different areas of responsibility, then asking experts from different domains to share how they operate. She decides to give it a try and learns how to facilitate a workshop.*

*Anna gathers people from different areas across the organization for a Big Picture EventStorming workshop session.^[6](#ch06.html#ch01fn57) The group maps the business process as it currently stands. They focus on the flow and order of business events and create boundaries to show where the type of work changes or a process ends. Their output visualizes that flow, along with emergent domains and their boundaries.*

*Anna’s workshop produces an output similar to that in [Figure 6-6](#ch06.html#a_big_picture_eventstorming_output_with). The group visualizes pivotal domain events (the small squares) between the emergent domains. Understanding the purpose for which you are creating software (and software architecture) is paramount, so Anna’s first step is to have the group identify the business logic of each area.*

![](assets/sarm_0606.png)

*With the help of the software engineers in the workshop, the group maps the current software components onto their Big Picture EventStorming output ([Figure 6-7](#ch06.html#mapping_the_current_software_components)). As they work, it becomes evident that there is a mismatch between the process and the implementation—which explains why the software architecture has drifted into a distributed big ball of mud. They also map the ownership of those components, and at that point Anna confirms her suspicions about where the teams’ high cognitive load is coming from.*

*Mapping the current software components (microservices, monoliths, or anything in between) on top of emerging domains lets the group assess the current state of affairs—and understand why it’s so difficult for YourFinFreedom to evolve its architecture. They can see the software components on top of the emergent domains, together with the ownership of those components (for simplicity, only Team A’s ownership is shown in [Figure 6-7](#ch06.html#mapping_the_current_software_components)). With this in place, they can reason about the teams’ cognitive load. Anna understands that owning different software components spread across the landscape is a challenge for any team: they’ll need knowledge from different domains to understand the purpose of building and architecting the software.*

![](assets/sarm_0607.png)

*The workshop generates many insights and allows the group to discuss how YourFinFreedom is organized to create value for customers. People involved in business operations now understand the current architecture and the constraints around it; the people involved in creating and maintaining software now understand the details of business operations and how the software enables them.*

*Anna wants to take one last step before closing the workshop: mapping the KPIs of the domains, tapping into the collected knowledge in the room ([Figure 6-8](#ch06.html#mapping_domain_kpis_in_a_big_picture_ev)). The group happily does so, with domain experts articulating why certain KPIs are relevant for their domains. They share more knowledge that previously existed only implicitly in the organization. Everyone is happy with the outcome of the workshop and amazed by the amount of information it provided in a short time.*

![](assets/sarm_0608.png)

Big Picture EventStorming allows organizations to enhance their original notation and capture information that is needed for a specific context or case. It can unlock significant knowledge and reflection about the current state of the system and the relevance of a KPI or a metric. Mapping the KPIs and metrics and discussing their purpose can unlock powerful conversations at all levels of the organization.
