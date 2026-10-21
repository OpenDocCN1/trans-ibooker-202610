levels of technical acumen. Business glossaries can document metadata associated with terms in the dictionary. That said, data dictionaries have some key distinctions from these glossaries.

Data dictionaries document relationships¹⁰ between data elements, like data’s origins, descriptions, and how they’re used relative to each other. The details in data dictionaries allow members of technical teams, like developers and data scientists, to share knowledge with each other and communicate quickly. These details include data structure, type, format, and length.

As your data governance council’s chair, you’ll track when folks submit new terms and ensure that your whole organization can access these changes to the data dictionary. Each data steward will manage terms for their respective data domains by soliciting terms, ensuring they’re correct, submitting them to the data dictionary, and editing as needed. As council chair, you’ll define relationships between elements; stewards define what those elements are.

If you’re not sure where to start, look at a database your organization uses. Data dictionaries act like catalogs that show how data inside databases relates to each other. These dictionaries grow in importance as you build your analytics stack. Once you start integrating data systems and warehouses, you’ll need to know which data lives in each environment, and how it relates to other data.

I’ve worked on projects with data dictionaries that were incomplete or nonexistent. Since my team’s job was to build our client a new data model, the client’s lack of knowledge about which data lived where, when new data deployed to different environments, and what certain terms meant caused a nightmare. Our team figured it out once they gained system access, but if they had this information sooner, we could have moved to production even faster.

To avoid this, check if your database has a data dictionary. Many systems have space for data dictionaries built in. If so, use that space to document your own work rather than storing your data dictionary in a separate tool. Your data governance council’s work will flow most smoothly if you keep your work stored in as few places as possible.

Data projects have a lot of moving parts, and your data stewards have a lot on their plates. So, review each database your business uses to store data and confirm if they have data dictionaries. If so, use these spaces to document data elements in each respective database.

10. https://web.stanford.edu/dept/pres-provost/cgi-bin/dg/wordpress/knowledge-nugget-business-glossary-vs-data-dictionaries/