
Remember the mission statement you wrote for data use in Clarify Your Company’s Mission Statement, on page 2? Your data governance council must do the same for security. This statement should be a clear, concise sentence which states how your organization commits to data security at all stages of each project. It’s best for your security data steward to write this statement, with input from you to ensure that it captures organizational goals and aligns with your data mission statement. A strong data security policy must state these three things:

- Your organization’s key security need
- The circumstances under which your organization must meet this need
- Operating standards which are crucial to meet this need

As always, specifics will differ depending on your business. One organization’s data security policy will be different than another’s depending on both organizations’ needs and standards. If your own organization’s key security need is confidentiality, then you’ll need to ensure that all actions, standards, procedures, and circumstances meet this requirement. This directly impacts how you’ll govern data in production. For instance, if you want to train an algorithm using patient data, you’ll need to ensure that you use tools and techniques which mask personally identifiable information (PII).

Your data security policy thus informs every action you and your data stewards will take, from evaluating vendors based on this statement to using the right algorithmic training techniques. By the time your data project hits development, your team of data architects, engineers, scientists, and analysts should know which security standards govern their work. This ensures alignment between stewardship groups by giving security a well-deserved seat at the table early.

Include your security data steward(s) in strategic project planning, then keep them involved through the production process. It’s crucial to involve security stewards as early in your process as possible. Remember in Chapter 4 when you wrote your process map for how things work today? It’s best to include your security steward in those efforts so they can explain how security works as-is and share security restrictions/requirements that might impact production. This allows stewards to advise on what’s possible, get buy-in from the right stakeholders, and ensure that your team has the resources to fulfill security standards at the right times, from deployment through production.

Gather security requirements early. Requirements gathering for security isn’t the same as gathering business requirements. Capturing these requirements is complex, comprehensive, and can take time. It’s best to start security requirements gathering as early in the process as possible, well before beginning a data migration or deploying data to production. That way, the data team in charge of your production environment knows which security standards to uphold.

Security requirements broadly fall within three categories:¹¹

- Confidentiality: controlling who gets to read information
- Integrity: ensuring that information and programs are changed only in a specified, authorized manner
- Availability: confirming that authorized users have continued access to information and resources

It helps to track these security requirements using a security requirements traceability matrix (SRTM). This matrix shows security requirements, their implementation schedule, and essential resources you’ll need to succeed. The SRTM is a useful tool for your security steward to own and maintain as your project’s work progresses. It should show test cases, test runs, test results, bugs, and defect resolution status.¹²

Depending on your project, you’ll add case-specific best practices as well. If you’re managing a database, you’ll add tasks like segregating data from the same server as your website, implementing HTTPS for website encryption, generating backups, implementing firewalls, and deploying full-stack observability systems.

Security includes managing metadata throughout production. Your data team will need to track metrics that include data sharing, usage, and permissions per dataset. Alongside the SRTM, your security data steward can use the data catalog your council created to track which metadata you can use for which purposes. Metadata management is a crucial yet often overlooked part of data governance. It’s treated as additional information that exists outside the data at hand.

The truth is that when managed as a strategic asset, metadata boosts security in key ways. Cataloging metadata keeps it in one place where it’s less likely to go missing or exist on several servers. It also ties usage restrictions and data licensing to specific data, which creates data ownership records. From a security standpoint, metadata cataloging creates records for data usage, access, and updates.

11. https://nap.nationalacademies.org/read/1581/chapter/4
12. https://oasas.ny.gov/system/files/documents/2022/04/requirements_traceability_matrix.pdf

When your data governance council manages metadata through production, you boost transparency and trust in your company’s data. In turn, one part of your data framework (risk and security) impacts another (trust and transparency). This symbiosis helps all data stewards work towards the same goals, before and during production.

## Set Specific Goals for Your Production Monitoring

All the work you’ve done thus far, from finding your framework for data governance to writing your roadmap, aims to democratize data by building scalable ways to manage it across your organization. You achieve this by embedding the standards you’ve set into your data pipelines and production processes. That way, you can automate your data governance standards and uphold these standards once your models reach production.

Thanks to the data framework you started using in Chapter One, you already know which standards you’re using to measure data governance success. Now that your first data project is in production, it’s time to put that framework into engineering practice.

Microsoft’s blog post, “Creating a modern data governance strategy to accelerate digital transformation,” gives a great overview of how they achieved this.¹³ Given their product suite, they embed their own data governance standards into Microsoft tools, specifically its Azure cloud computing platform and its Visual Studio development environment.¹⁴

You (of course) aren’t obligated to choose Microsoft for your data management. There are many products that help companies govern data, and multiple cloud computing platforms. That said, Microsoft’s industry influence and big product suite make it a key player in the data governance space. Whichever option you choose, remember that tools matter less than implementing your governance framework into the right tools for your business. Here is an example of how Microsoft did it.

To start, Microsoft invested in creating data standards upfront. They formalized their governance standards as you’ve done throughout this book. Once those standards were written down and shared, Microsoft’s data stewards set compliance measurements against these standards. Then, the stewardship team embedded these compliance standards throughout their development environment, which in their case is a single enterprise data lake platform.

To track compliance with governance standards, Microsoft automated its review process. They have Azure generate, assign, and track data governance standards within its data lake. When the engineering team meets certain milestones, the data steward who owns the data at hand gives a governance assessment. They review the controls, scanning, measurement, and access controls by asking a few key questions:

- Which redundant/outdated data exists?
- Who has access to this data?
- How are the stewards with access to this data using it?

By this time, your data governance council should have selected an enterprise data lake (EDL) where you can move data from disparate sources into a single source of truth. Using an EDL for your data governance efforts has two key benefits.

First, it consolidates your data in one place that all data stewards can access. When all stewards know where data exists, along with how to contribute and find the data they need, far fewer questions exist about where data lives and how to access it. EDLs also solve the constant question of where data lives and originates. It's too common for crucial company data to live in an Excel sheet on someone's local machine. If a data steward needs to track that data's origin, finding it often yields a wild goose chase. Putting all the data your company uses into an EDL saves valuable time.

Secondly, it gives your stewards a single source to do data scanning. I think we've all worked with organizations that stored data across countless systems and databases. In chaotic dev environments like these, data governance gets drastically harder. EDLs give your team a single space to view and govern data across each project's life cycle. In turn, this infrastructure helps prevent data drift.

Despite their benefits, EDLs only work if the data within them is clean and accounted for. To see how different teams use the same data, you'll need to set up lineage tracking.

## Use Data Lineage Tracking

Data lineage plays a key role in your plan to embed data governance into engineering. It confirms where the data comes from, recording the path it takes over time and visualizing its flow from one system to the next. In most organizations, the same data spans several projects and pipelines. Data lineage helps data scientists track errors and find those errors' root causes. This

13. https://www.microsoft.com/en-us/insidetrack/driving-effective-data-governance-for-improved-quality-and-analytics
14. https://www.microsoft.com/en-us/insidetrack/driving-effective-data-governance-for-improved-quality-and-analytics
