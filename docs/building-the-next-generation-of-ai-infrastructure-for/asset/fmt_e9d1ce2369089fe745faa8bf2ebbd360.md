
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
This is probably one of the most important layers (or at least the one that gets a lot of attention) of the modern concept of AI infrastructure. Processing power takes the form of classic and new acronyms such as `CPU` (central processing units that enable floating point arithmetic), `GPU` (graphic processing units optimized for matrix multiplications), `PTU` ([provisioned throughput units](https://oreil.ly/pSwFu)), `NPU` (neural processing units for optimized and more efficient processing of neural network operations), and `LPU` (language processing units adapted to specific LLM operations). In general terms, which one you choose depends on the stage of the AI lifecycle (e.g., training requires more computing power than inference) and the technical approach depending on your providers’ offering.
