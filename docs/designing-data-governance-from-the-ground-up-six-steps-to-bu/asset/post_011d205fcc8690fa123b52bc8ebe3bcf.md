
In the insurance sector, you might encounter:

- Provider
- Member

Any of these sectors could have some of the previous data domains as well. So for example, I’m sure that all 3 sectors would all have ‘Location’, ‘Transaction’, and ‘Legal’ as data domains.

Data domains define the model schema and other attributes for each respective business area. You might choose to have data domains for customers, employees, and products. The stewards who oversee each domain will define the attributes, format, and metadata which confirms how domains should expect to store the data. This last part is crucial since your data governance council must standardize how it will exchange data between systems.

Within databases, a data element is the collection of values that data elements may contain. Your databases must know how to store and categorize data across the business. So, your data governance council must make binary standards that data can fit into.

Let’s revisit the healthcare data domains. If you think of these data domains as a drop-down menu, you will see this list:

- Patient
- Facility
- Medical procedure
- Location
- Transaction
- Legal

Within a database, all data should align with one of these six data domains, and any new data should map up to one of the six. A key part of data governance involves correctly categorizing data within all organizational systems, and keeping it consistent across systems. So, each data steward will need to build their data domains by choosing how to categorize it. This includes categorizing data according to domains and relevant sub-domains.

Sub-domains help data stewards categorize data at a more granular level. If you’ve chosen Patient as a data domain, you can create sub-domains according to the types of patients (infants, children, geriatric, and so on). The steward per data domain will need to define each relevant sub-domain, ensuring a relationship between each sub-domain and its parent.

It helps to think of sub-domains as types of data domains. If you’ve chosen Facility as a data domain, consider the types of facilities your business engages. That will help you choose appropriate sub-domains which you can use to categorize facility data.

Specifics per domain and sub-domain vary across industries and organizations. Whatever you and your data stewards choose, recall that your goal is to create consistent grouping conventions for essential data types that apply across enterprise-wide business units (culture) and systems (technical).

Throughout my career in tech, I've encountered roadblocks where teams within the same organization define the same data differently. I've also seen naming conventions that differ based on which department I'm engaging, even if we are all discussing the same data. Having your data governance council define data domains and sub-domains is hugely important to avoid this blocker. It establishes your data governance council and stewards as a single source of truth for this information.

Per Wash U., these are some principles to follow when defining data domains:

- Each domain is aligned with a business function, like Research, Clinical, etc.
- Not a system or an application (Workday, Epic, RMS)
- Each domain has one administrator
- Each domain will determine who has access to data within that domain

Once you and your stewards define data domains and sub-domains, you will need to write it all down. That's where a data dictionary will come in.

## Write a Data Dictionary

A data dictionary stores information about data that lives in all your organization's systems. It gives everyone a shared vocabulary for how to discuss data in your organization. Whether it's the CEO, a developer, or a customer service representative, everyone should reference the data dictionary when they're unsure what certain data items mean. This dictionary is a comprehensive space to define rules, regulations, and definitions for your organization's data.

Data dictionaries play a key role in your governance efforts. They reinforce data domains/sub-domains and help teams avoid inconsistencies with data. Having more consistent data makes it easier to analyze, because it gives everyone shared understanding of what different terms mean. Data dictionaries also help your data governance council share data standards—rules that explain how to collect, record, and socialize data—with everyone in your organization.

You might wonder how a data dictionary differs from a business glossary. Glossaries have key terms with names and definitions. Subject matter experts write glossaries with business users in mind, so readers shouldn't need high levels of technical acumen. Business glossaries can document metadata associated with terms in the dictionary. That said, data dictionaries have some key distinctions from these glossaries.

Data dictionaries document relationships¹⁰ between data elements, like data’s origins, descriptions, and how they’re used relative to each other. The details in data dictionaries allow members of technical teams, like developers and data scientists, to share knowledge with each other and communicate quickly. These details include data structure, type, format, and length.

As your data governance council’s chair, you’ll track when folks submit new terms and ensure that your whole organization can access these changes to the data dictionary. Each data steward will manage terms for their respective data domains by soliciting terms, ensuring they’re correct, submitting them to the data dictionary, and editing as needed. As council chair, you’ll define relationships between elements; stewards define what those elements are.

If you’re not sure where to start, look at a database your organization uses. Data dictionaries act like catalogs that show how data inside databases relates to each other. These dictionaries grow in importance as you build your analytics stack. Once you start integrating data systems and warehouses, you’ll need to know which data lives in each environment, and how it relates to other data.

I’ve worked on projects with data dictionaries that were incomplete or nonexistent. Since my team’s job was to build our client a new data model, the client’s lack of knowledge about which data lived where, when new data deployed to different environments, and what certain terms meant caused a nightmare. Our team figured it out once they gained system access, but if they had this information sooner, we could have moved to production even faster.

To avoid this, check if your database has a data dictionary. Many systems have space for data dictionaries built in. If so, use that space to document your own work rather than storing your data dictionary in a separate tool. Your data governance council’s work will flow most smoothly if you keep your work stored in as few places as possible.

Data projects have a lot of moving parts, and your data stewards have a lot on their plates. So, review each database your business uses to store data and confirm if they have data dictionaries. If so, use these spaces to document data elements in each respective database.

10. https://web.stanford.edu/dept/pres-provost/cgi-bin/dg/wordpress/knowledge-nugget-business-glossary-vs-data-dictionaries/

Once that work is done, your council should confirm that the definitions for these elements are consistent across systems and store those definitions in the data dictionary. If you audit data definitions across systems and find that the same term is defined differently in one database vs. another, your data governance council will need to address this. Once your council does the work to create data standards, the next step is to uphold them across every team and system used throughout your business.

If these databases offer space for reference data, use that to your advantage as well. This feature lets you reconcile data between systems, which can help your team find missing records as you move data between systems. Since this is par for the course with big data projects, you'll want to ensure that your data governance council has documented as much reference data as possible prior to starting each respective project. Lacking this information upfront will cause delays once the project begins. Worse, it can cause consistent data quality risks. Documenting your data upfront will save you and your colleagues major headaches in the long run.

## Your Next Step

This chapter asked a lot of you. You've been tasked to find an executive sponsor for your council, confirm key principles for your council to uphold, convince colleagues to serve as data stewards, define data domains per data steward, and write a data dictionary, accounting for reference data in each database where possible.

If you're feeling overwhelmed, I don't blame you. Building your data governance council will take the most time and be the hardest aspect of this work.

If you've ever bought a plot of land and built a new house on top, you know how slow the first few months can feel. It will look like nothing's getting done as time and money accrue. You might consider cutting your losses, thinking that your end goal is too ambitious and it's best to quit while you're ahead. Then, after months (if not years) of waiting, one day you visit your new home and see it all clicking into place.

As you survey the work several people poured into this project, you realize that while your home isn't done yet, you can see the bigger picture. You've learned that projects with this scope will hit inevitable roadblocks, and you'll encounter more before your home is complete. But you also know that with the foundation firmly in place, you're equipped to overcome them.

I told you in the Preface that I want you to begin this book in Los Angeles and have a plan to build data governance upon landing in New York. While this
