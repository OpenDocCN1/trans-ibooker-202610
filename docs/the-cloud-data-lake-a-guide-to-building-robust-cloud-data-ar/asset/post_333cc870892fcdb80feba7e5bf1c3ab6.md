
be a requirement for them. In your investments, prioritize for end-to-end customer experiences that are seamless, and err on the side of saying no rather than offering a poorly implemented or half-baked solution.

Let me take a fictitious example to illustrate this point. Klodars Corporation has a data platform team that needs to serve sales, marketing, and the rain gear product line as well as a recent acquisition of a winter gear business. So the team implements a data mesh architecture and offers blueprints for a data lake domain that these customers can implement with a self-serve model. The blueprint includes templates to provision cloud resources required for the data lake, and the customers can choose the cloud services. However, the sales, marketing, and rain gear teams do not have enough information on whether they need to choose a data lake alone or a data lake and data warehouse. So the sales and marketing teams approach the data platform team with questions. But the rain gear product team chooses a data lake alone, when its need is to build a warehouse for its supply chain inventory. This ends up creating dissatisfaction for the customers while increasing the load for the data platform team, who has to react to their questions and issues.

An alternative path for the data platform team would be to do an end-to-end implementation with the sales and marketing team while deprioritizing the use cases for the time being for the other customers. Based on the learnings from this implementation, the data platform team can evaluate the readiness for a self-serve architecture with a data mesh and roll it out more broadly.

In summary, setting expectations up front is more optimal than setting the customer up for disappointment later. Be transparent and clear about your prioritizations, and set expectations with your customers about when you’ll be able to serve them.

## Lesson 6: Big Impact Does Not Happen Overnight

Finally, this is probably more of a life lesson that also applies to data lake architectures. The culmination of big impact has many steps and lessons and failures behind it. Cloud data lake architectures offer great promise, but they also require a lot of learning and iterations before you get them right. As an example, Apache Iceberg, a very popular open source technology that supports the lakehouse pattern today, had humble beginnings in Netflix, which started with an implementation to overcome the then-shortcomings of Amazon S3 object storage. The Netflix team believed in the power of data for its organization and thoughtfully implemented Apache Iceberg to serve its customers with high-quality data with strong guarantees on top of lower-cost object storage. As they say, the rest is history, and you can learn more about this by revisiting “Apache Iceberg” on page 157.

Plan for customer impact, and prepare to learn and fine-tune as you progress. Make choices based on the impact they offer to customers, and stay consistent in your implementation while keeping open-minded for learnings. Especially if you are an early adopter of a technology, you can help develop the technology with the provider and thus get ample support from the technology provider in return. Similarly, identify highly motivated early adopters within your customer base and offer to develop your solution with them. Celebrate your successes together, and share your credits.

## Summary

I was brainstorming with my editors on various ways to end this book, and I thought it fitting that I end with a set of six lessons that you can apply regardless of where you’re in your data lake journey. First and foremost, huge kudos to you for taking this step, and remember that you’re already on your path to innovating your business—it is only a matter of time before things fall into place. Data is the future, and cloud is where this future is happening as we speak. We’re all part of defining this future by investing in cloud data lakes.

The technologies and cloud offerings themselves have seen rapid improvements over time, and these changes can be overwhelming. I personally always start with the basics of what is important to a project and why it is important, and I have structured my book to build this fundamental framework that can be leveraged in spite of the technologies used. To keep on top of the various technologies, I recommend that you watch the conferences from data vendors and public cloud providers to stay abreast of trends. There are also a ton of resources available for various technologies on podcasts and YouTube. I would especially recommend that, in addition to watching the sessions from the technology builders, you watch the customer sessions on what problems they solved, which will give you great ideas. Data+AI Summit is one of my favorite conferences, and the sessions are available on demand for your viewing.

We’re at that point where this book has reached its natural conclusion, and I thank you for being a great audience. I’m really excited to learn more about where your cloud data lake journey takes you. If you want to share your experiences or ask a question, or you want to brainstorm about what is a good choice for you, reach out to me, and I’ll be more than happy to chat more. I’m on Twitter as @rukmanigopalan, and you can find me on LinkedIn at https://www.linkedin.com/in/rukmanig.

## Cloud Data Lake Decision Framework Template

In this section, I’ll provide a template that you can use to plan your cloud data lake solution. You can customize this template as needed based on your specific scenarios or customers. I recommend that you plan at least over a one- to two-year horizon to ensure that your cloud data lake design is sustainable before you have to make drastic changes.

### Phase 1: Assess Framework

*Target 60%–70% accuracy and completeness*

The objective of this phase is to define and prioritize the requirements for the cloud data lake that will drive your architecture and implementation decisions. In this phase, the data engineering team needs to identify the requirements based on two key aspects: customer requirements and business drivers. I strongly recommend doing your due diligence in the assess phase, which will set you up for smooth planning and execution of the subsequent phases.

Use Table A-1 to record the findings from your stakeholder interviews.

### Table A-1. Inventory of problems and requirements

| Customer | Problem | Severity of problem | Helpfulness of data lake | How cloud data lake can help |
|---|---|---|---|---|
|  |  | High/Medium/Low | High/Medium/Low |  |
|  |  | High/Medium/Low | High/Medium/Low |  |
|  |  | High/Medium/Low | High/Medium/Low |  |
|  |  | High/Medium/Low | High/Medium/Low |  |
|  |  | High/Medium/Low | High/Medium/Low |  |
|  |  | High/Medium/Low | High/Medium/Low |  |

Once you have defined your requirements, prioritize them against the business drivers, and use **Table A-2** to prioritize the requirements of your cloud data lake.

### Table A-2. Requirements for your cloud data lake

| Priority | Requirement |
|---|---|
| High/Medium/Low |  |
| High/Medium/Low |  |
| High/Medium/Low |  |

| Dataset to be ingested | Frequency | Data prep requirements | Data size |
|---|---|---|---|
|  |  |  |  |
|  |  |  |  |

In **Table A-5**, you can record the data processing requirements, which list the data preparation and data curation transformations and the finalized choices of what data processing technology (as an example, Spark or Hadoop) and tooling (for instance, run Spark on a VM, use a Databricks cluster) will be used.

### Table A-5. Data processing deliverables

| Input datasets | Output datasets | Data processing tool | Requirements (configurations, VMs required, etc.) |
|---|---|---|---|
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |
