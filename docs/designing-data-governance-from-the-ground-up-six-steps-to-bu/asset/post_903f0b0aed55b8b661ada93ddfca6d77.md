
## Monitor Data in Production

I subscribe to several newsletters. In January 2022, one weekly newsletter from the author Anne Helen Petersen shared the concept of policy drift. First proposed by Jacob Hacker, policy drift describes problems that arise when policies built to serve specific social needs are not updated to solve modern problems.¹

As one example, Section 230 is a piece of U.S. legislation that protects website platforms from being held liable for third-party content.² Signed in February 1996, this legislation intended to distinguish website providers from traditional publishers. Suffice to say, it fails to account for social media, big data, and a range of realities that exist today. That leaves companies like Meta largely unchecked and unregulated, which yielded scandals like the Cambridge Analytica data breach.³ Section 230 might have met the moment when President Clinton signed it into law, but it’s inadequate for today’s issues.

As I read Petersen’s newsletter, I saw parallels between policy drift in politics and model drift in data. It’s common for machine learning teams to think that reaching production equals success. The thinking goes, “If I can just monitor my data until then, I’ll know that my data governance worked.”

Here’s the more complex truth: if your data governance plan doesn’t address data drift, it will ultimately fall short.

Data drift occurs when unexpected, undocumented changes to data structure, semantics, and infrastructure affect data quality. In machine learning, environmental changes affect how the data used to train algorithmic models engages each other throughout the training process.⁴ Given how much data companies own today, and how much data models are exposed to throughout their life cycles, data drift is inevitable. The good news is that having models lose predictive power is far from a death sentence if teams catch data drift on time and stewards have a plan to govern data throughout each life cycle.

In this chapter, you’ll explore what data governance looks like once your data project is in production. You’ll see how to apply your data governance framework to the data monitoring process, from finding bottlenecks in production to ensuring that the data your models keep consuming meets quality standards. When you’ve finished this chapter—the last one in our book—you will have a plan in place to meet and beat data drift head-on.

The sources in this chapter expand on each subject and are ideal places to keep learning once you turn the final page. This chapter’s goal is to give you the steps you must take regardless of your unique data work. No matter your project(s), you’ll need life cycle plans, production monitoring, and regulation tracking to succeed. This chapter will show you how to build them.

1. https://annehelen.substack.com/p/what-a-rugged-resilient-society-demands
2. https://crsreports.congress.gov/product/pdf/R/R46751
3. https://www.wired.com/story/cambridge-analytica-facebook-privacy-awakening/
4. https://streamsets.com/why-dataops/what-is-data-drift/#:~:text=Data%20Drift%20Defined&text=Data%20drift%20is%20unexpected%20and,new%20opportunities%20for%20data%20use.

### Make a Plan to Govern Data Throughout the Life Cycle

You know from reading this book how much variance there is amongst data types, let alone data projects. So, it follows that data life cycles differ depending on context. You’ve set up your data governance council to manage data throughout each respective life cycle. That means it’s not enough to govern data through processing and storage. You will also need a plan to govern data at key phases towards the end of each life cycle: data usage, data archiving, and data destruction.⁵

### Data Usage

This stage is both the biggest opportunity and biggest risk for data governance, because this stage is where folks within and outside your organization consume your data in diverse ways. If your data team deploys analytics dashboards for your clients to use, publishing those dashboards on your company’s website enables your clients to use this data for their benefit. If your datasets have unchecked problems, you’ll find out in the data usage stage.

This is also the stage where you'll test if your data governance council's decisions work. Since data usage involves access needs, your data security will be tested at this stage. If a third party audits your data for bias, it's most likely to be in this stage as well, especially if your organization sells data as its product.

Data usage shows how your organization shares data, and how people consume it. Any constraints on your council's data governance will reveal themselves at this stage.

5. https://www.oreilly.com/library/view/data-governance-the/9781492063483/ch04.html

### Data Archiving

If you've ever done a data migration, then you're familiar with data archiving. It's the process of copying data from one active environment and moving it to another. Let's say you host public-facing data that your company owns within an on-premise server and want to migrate it to a cloud computing platform. Data archiving involves having data engineers manage the migration process through these steps:

- Moving your raw data from the on-premise environment into the cloud environment
- Putting this raw data into ETLs
- Cleaning this data within ETLs, then moving refined data to the dev/test database
- Connecting the test database to the API and user interface
- Turning on the data pipeline in your cloud environment so that data displays on the database front end
- Encrypting and decrypting data throughout the migration to meet data governance standards for security

If all goes well, your users shouldn't know that you've switched environments. To manage this process smoothly and efficiently, your data governance plan for data archiving should answer three key questions:

- How long will your team retain this data?
- Where will your team store this data?
- Which controls will you apply to this data?

No usage or maintenance should occur during the archive process. Once your data is in your cloud platform of choice, your team must destroy data from the archive storage.

### Data Destruction

I know from experience how painful this step can be. Many organizations store any and all data that they’ve ever owned. I once worked on a project that required my team to automate the process of extracting data from more than 52,000 PDFs ahead of a website migration—and that 52k was post-audit, meaning there were even more beforehand.

Your business can’t afford to keep all this data. Data centers and servers cost huge amounts of money and demand consistent maintenance. Beyond that, continuing to host data indefinitely yields compliance issues. That’s why data destruction is an ideal task for your compliance data steward. They can clarify the data you don’t need while prioritizing the data that you do need. This involves asking the following questions:

- Is your organization required to retain data for specific periods of time?
- Is all data beholden to this mandate? If not, which data can you destroy and which data must you keep?
- Where will you store the data you must keep, and which format will this data be kept in?
- Which governmental and/or industry standards is your organization required to uphold?
- How will you confirm that data destruction occurred?

To answer these questions, ask your compliance data steward to create a timeline for the data destruction plan. They are the best steward to own this task since their role requires tracking all the policies you’ll need to follow, from state regulations to industry best practices. It requires answering a few key questions, which your steward can track in template format.

MIT has a template for creating data management plans that you can use.⁶ While some of these questions are unique to MIT, you can apply most of them to your own data project.

You can also use DMPTool, an open source way to build bespoke data management plans.⁷ DMPTool nudges you to add your project details, collaborators, research outputs, and more. The tool’s “Write Plan” section asks you to answer questions about data collection, documentation, ethics, storage, and preservation.

6. https://libraries.mit.edu/data-management/plan/write/
7. https://dmptool.org/
