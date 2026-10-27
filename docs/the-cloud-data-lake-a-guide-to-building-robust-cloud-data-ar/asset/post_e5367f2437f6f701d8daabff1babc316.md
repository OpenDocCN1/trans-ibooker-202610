
> ![This element indicates a warning or caution.](img/7a3ef505ef42a8f03ad37f9a013ac15a_15_0.png)

## O’Reilly Online Learning

O’REILLY

For more than 40 years, *O’Reilly Media* has provided technology and business training, knowledge, and insight to help companies succeed.

Our unique network of experts and innovators share their knowledge and expertise through books, articles, and our online learning platform. O’Reilly’s online learning platform gives you on-demand access to live training courses, in-depth learning paths, interactive coding environments, and a vast collection of text and video from O’Reilly and 200+ other publishers. For more information, visit *http://oreilly.com*.

## How to Contact Us

Please address comments and questions concerning this book to the publisher:

O’Reilly Media, Inc.  
1005 Gravenstein Highway North  
Sebastopol, CA 95472  
800-998-9938 (in the United States or Canada)  
707-829-0515 (international or local)  
707-829-0104 (fax)

We have a web page for this book, where we list errata, examples, and any additional information. You can access this page at *https://oreil.ly/the-cloud-data-lake-1e*.

Email *bookquestions@oreilly.com* to comment or ask technical questions about this book.

For news and information about our books and courses, visit *https://oreilly.com*.

Find us on LinkedIn: *https://linkedin.com/company/oreilly-media*

Follow us on Twitter: *https://twitter.com/oreillymedia*

Watch us on YouTube: *https://www.youtube.com/oreillymedia*

## Acknowledgments

As the proverbial idiom goes, it takes a village to write a book, and I’m eternally grateful to the multitude of folks who helped in making this book a reality.

First and foremost, I would like to deeply thank my teams, customers, and partners at Microsoft during my tenure with Microsoft Office, Azure HDInsight, and Azure Data Lake Storage/Cosmos for building my understanding of the data space and trusting my instincts and approach as I impacted the transformational insights of various organizations with these offerings. The list of people here is so long that I could fill a book with all their names.

Thanks to Tomer Shiran from Dremio and the team at Monte Carlo—Barr Moses, Lior Gavish, and Molly Vorwerck—for your enlightening interviews on data lakehouse and data observability that turned into great sidebars for the book.

The team at O’Reilly has been amazing at helping me shape my thoughts and approach into this book. Jill Leonard and Andy Kwan—thanks for being there every step of the way, whether it was discussing the structuring of certain topics and the appropriate level of details or, many times, helping me with my multiple bouts of impostor syndrome.

A huge shoutout to the tech reviewers who took the time to read through the book and shared their very valuable insights and feedback: Shreya Pal, Andrei Ionescu, Alicia Moniz, Prasanna Sundararajan, Chidamber Kulkarni, Gordon Wong, Gareth Eager, Vinoth Chandar, and Vini Jaiswal. You really helped me understand a reader’s journey, which offered valuable lessons for life.

Finally, no words will ever express the gratitude I feel toward my family—my awesome husband, Sriram Govindarajan, and my amazing kids, Anish Bharadwaj and Dhanya Bharadwaj, for being the constant source of inspiration and support for me, not just for this book but for life. Thanks to Janaki Gopalan and Gopalan Krishnamachari, my mom and dad, who are not with me anymore physically but stay with me forever in the core values they have instilled in me around hard work, accountability, and unconditional giving.

## Big Data—Beyond the Buzz

> *Without big data, you are blind and deaf and in the middle of a freeway.*
>
> —Geoffrey Moore

If we were playing workplace bingo, there is a big chance you would win by crossing off all these terms that you have heard in your organization in the past three months: digital transformation, data strategy, transformational insights, data lake, warehouse, data science, machine learning, and intelligence. It is now common knowledge that data is a key ingredient for organizations to succeed, and organizations that rely on data and AI clearly outperform their contenders. According to an [IDC study sponsored by Seagate](#), the amount of data that is captured, collected, or replicated is expected to grow to *175 zettabytes (ZB)* by the year 2025. This data that is captured, collected, or replicated is referred to as the *Global DataSphere*. This data comes from three classes of sources:

- *The core*: traditional or cloud-based datacenters
- *The edge*: hardened infrastructure, such as cell towers
- *The endpoints*: PCs, tablets, smartphones, and Internet of Things (IoT) devices

This study also predicts that *49% of this Global DataSphere* will be residing in public cloud environments by the year 2025.

If you have ever wondered, “Why does this data need to be stored? What is it good for?” the answer is very simple. Think of all of this data as pieces of words strewn around the globe in different languages, each sharing a sliver of information, like pieces of a puzzle. Stitching them together in a meaningful fashion tells a story that not only informs but also could transform businesses, people, and even the way the world runs. Most successful organizations already leverage data to understand the growth drivers for their businesses and perceived customer experiences and to take the rightful action; looking at “the funnel,” or customer acquisition, adoption, engagement, and retention, is now largely the lingua franca of funding product investments. These types of data processing and analysis are referred to as *business intelligence*, or BI, and classified as “offline insights.” Essentially, the data and the insights are crucial in presenting the trend that shows growth so business leaders can take action; however, this workstream is separate from the core business logic used to run the business itself. As the maturity of the data platform grows, an inevitable signal we get from all customers is that they start getting more requests to run more scenarios on their data lakes, truly adhering to the idiom “Data is the new oil.”

Organizations leverage data to understand the growth drivers for their businesses and perceived customer experience. They can then use data to set targets and drive improvements in customer experience with better support and newer features. They can also create better marketing strategies to grow their businesses and drive efficiencies to lower their costs of building their products and organizations. Starbucks, the coffee shop that is present around the globe, uses data in every place possible to continuously measure and improve its business. As explained in [this YouTube video](https://www.youtube.com/), Starbucks uses the data from its mobile applications and correlates the data with its ordering system to better understand customer usage patterns and send targeted marketing campaigns. It uses sensors on its coffee machines that emit health data every few seconds, and this data is analyzed to drive improvements into its predictive maintenance. It also uses these connected coffee machines to download recipes to them without involving human intervention.

As the world is just learning to cope with the COVID-19 pandemic, organizations are leveraging data heavily not only to transform their businesses but also to measure the health and productivity of their organizations to help their employees feel connected and minimize burnout. Overall, data is also used for world-saving initiatives such as [Project Zamba](https://projectzamba.org/), which uses AI for wildlife research and conservation in the remote jungles of Africa, as well as leverages IoT and data science to create a circular economy to promote environmental sustainability.

### What Is Big Data?

All of the examples given previously share a few things in common:

- These scenarios illustrate that data can be explored and consumed in a variety of ways, and when the data is generated, there is not really a clear idea of the consumption patterns. This is different from traditional online transaction processing (OLTP) and online analytical processing (OLAP) systems, where the data is specifically designed and curated to solve specific business problems.
