
Let’s formalize our view for an agent-based system a bit. What we mean by an agent is a Generative AI LLM wrapped around a module, function, service, or database. This allows an end user to interact with that functionality in natural language (see [Figure I-6](#introduction01.html#ch00_figure_6_1771364788596736)). It also allows us to log the intent of the agent upon servicing every input from the user. In other words, we can log the why and what of the agent’s behavior, because the agent understands and can produce natural language describing every step it is taking. This also allows us to introduce safeguards on the agent’s behavior, by having it observed by another agent, let’s call it the Safeguard Agent. This agent can be tasked with our ethics, compliance, or regulation guidelines, and intervene if what is asked of the system is not compliant.

![](assets/ch00_figure_6_1771364788596736.png)

By the same token, let’s now assume that, in addition to the database in figure 6, our application also includes a microservice (e.g., an API call to a CRM back-end), and some of the user’s queries or commands are better handled through that. We have a choice of having the user to separately use an agent for that service, or to have the agent collaborate with another agent that is in direct interaction with the user, making it seamless and more usable.

To make this happen, we can have the original agent servicing the user aware of the agent wrapping the microservice and have them communicate to fulfill user queries. This communication can be in natural language, which means it can capture the intent and be robust to changes in the format and syntax of the specific calls to the underlying services.

Before you know it, an enterprise’s entire software and applications can be agentified in this way (see [Figure I-7](#introduction01.html#ch00_figure_7_1771364788596751)).

![](assets/ch00_figure_7_1771364788596751.png)

[Figure I-8](#introduction01.html#ch00_figure_8_1771364788596765) will help us clarify the distinction between a model and an agent. A model is a transformation function. It takes data as input, and produces data as its output. A modern-day AI agent consists of, at least, an LLM that has been enabled to call some tools.

A Gen-AI model (or LLM) takes some input and, through the process of predicting what is expected, produces some output. For example, we give it a coding task, and it produces some code, and depending on the complexity of the task, the code may be usable as is, but to run it, you will have to copy-paste it into your programming environment and manually hit ‘run’.

By contrast, an agent, given the right set of tools for coding, would start by having its LLM generate the code, then run it in a container, observe the results, modify the code, and therefore have a better chance of producing useful code. In fact, taking an LLM and running it on a coding benchmark, results in lower accuracy rates, than when the same LLM is powering a coding agent and is given the same coding benchmark.

![](assets/ch00_figure_8_1771364788596765.png)

The field of multi-agent AI calls for agents that can communicate and coordinate. This can be in the context of a team of agents, which implies collaboration, or it may be across teams of agents with varying levels of trust and even adversity.

# Agentified Applications and Software

To be effective, agents should be allowed to talk to each other. For example, imagine your company intranet with its useful search box, directing you to the apps and resources you need. If you are a large enough company, these apps, owned by different departments, each have their own search boxes. It makes a lot of sense to create agents, maybe by using techniques like RAG, to augment the search boxes. What does not make sense is to force the user to repeat their query into an app’s search box once the intranet search box has identified it as useful given your initial query. Rather, we would prefer the top agent to coordinate with agents representing the various apps and present a consolidated and unified chat interface to you, the user (see [Figure I-9](#introduction01.html#ch00_figure_9_1771364788596786)).

![](assets/ch00_figure_9_1771364788596786.png)

A multi-agent system representing an organization’s software or various workflows can have several interesting advantages, including improved productivity, operational resilience, improved robustness, and the ability for faster upgrades of different modules. This book will help you see how this is achieved and provide you with examples illustrating these advantages.

Migrating a software/application stack to an agent-based architecture offers several distinct benefits:

### Intent Logging

Each agent can log its intent on every transaction or call it makes to various services, enhancing transparency and traceability. Something that is harder to achieve with software logs, which tend to be terse while voluminous, and lack explanation of the various context and rationale behind the software’s behavior.

### Robust Inter-Agent Communication

Communications between software and apps represented by agents are intent-based and often in natural language, making the interactions more understandable and reducing the complexity of integration.

### Safeguard Agents for Responsible AI

The ability to add safeguard agents to monitor and check the behavior of each agent ensures compliance with responsible AI practices and ethical standards.

### Modularity and Flexibility

Agent-based systems are highly modular, allowing for easier updates and maintenance. Each agent can be independently developed, deployed, and scaled without affecting the entire system.

### Upgradeability

Software and application modules being represented by agents provide a more seamless and less disruptive way to upgrade the underlying software, as the complexity of mapping intent to the actual back-end calls is handled by the LLMs, which simplifies and even does away with much of the manual rewiring often involved in API and software upgrades.

### Interoperability

Agents can interact with diverse systems and services, providing a unified interface and facilitating integration across different platforms and technologies.

### Autonomy and Specialization

Each agent can be specialized for a particular task, leading to more efficient and optimized execution of specific functions within the application stack.

### Enhanced Fault Tolerance

The failure of one agent or software back-end does not necessarily disrupt the entire system. Agents can be designed to handle failures gracefully, improving overall system reliability.

### Improved Data Handling

Agents can handle data locally and only communicate relevant information, reducing data transfer costs and improving performance. Disparate data sources can be represented by different agents, helping mitigate issues with data consolidation or being behind on data modernization.

### Better Resource Management

Agents can optimize resource usage by adjusting their operations based on current system states and workloads.

### Customizability and Extensibility

New agents can be added or existing ones modified with minimal disruption, allowing for continuous improvement and adaptation to new requirements.

### Breaking Software Silos

Redundancies and overlaps in various enterprise software are a constant source of frustration for users. Agents can help reduce or eliminate such silos by operating the software modules on behalf of users, and consolidating and simplifying responses across multiple software modules as they serve a user’s intent.

These advantages can result in major operational efficiencies, IT and infrastructure savings, and measurable enhancements to the quality of business decision making.

However, the benefits from multi-agents systems also come with the need for careful and safe design considerations.
