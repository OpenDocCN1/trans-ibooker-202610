![](assets/cover.png)

![](assets/cover.png)

# Implementing MLOps in the Enterprise

by Yaron  Haviv and Noah  Gift

Copyright © 2023 Yaron Haviv and Noah Gift. All rights reserved.

Printed in the United States of America.

Published by O’Reilly Media, Inc., 1005 Gravenstein Highway North, Sebastopol, CA 95472.

O’Reilly books may be purchased for educational, business, or sales promotional use. Online editions are also available for most titles ( [http://oreilly.com](http://oreilly.com) ). For more information, contact our corporate/institutional sales department: 800-998-9938 or *corporate@oreilly.com* .

*   Editors: Corbin Collins and Nicole Butterfield
*   Production Editor: Beth Kelly
*   Copyeditor: FILL IN COPYEDITOR
*   Proofreader: FILL IN PROOFREADER
*   Indexer: FILL IN INDEXER
*   Interior Designer: David Futato
*   Cover Designer: Karen Montgomery
*   Illustrator: Kate Dullea

*   October 2023: First Edition

Add additional revdate spans below as needed. 

# Revision History for the Early Release

*   2022-09-13: First Release
*   2022-10-25: Second Release
*   2023-02-23: Third Release
*   2023-06-21: Fourth Release

See [http://oreilly.com/catalog/errata.csp?isbn=9781098136581](http://oreilly.com/catalog/errata.csp?isbn=9781098136581) for release details.

The O’Reilly logo is a registered trademark of O’Reilly Media, Inc. *Implementing MLOps in the Enterprise*, the cover image, and related trade dress are trademarks of O’Reilly Media, Inc.

The views expressed in this work are those of the authors and do not represent the publisher’s views. While the publisher and the authors have used good faith efforts to ensure that the information and instructions contained in this work are accurate, the publisher and the authors disclaim all responsibility for errors or omissions, including without limitation responsibility for damages resulting from the use of or reliance on this work. Use of the information and instructions contained in this work is at your own risk. If any code samples or other technology this work contains or describes is subject to open source licenses or the intellectual property rights of others, it is your responsibility to ensure that your use thereof complies with such licenses and/or rights. PROD: Uncomment the following sentence if appropriate and add it to the above para: This book is not intended as \[legal/medical/financial; use the appropriate reference\] advice. Please consult a qualified professional if you require \[legal/medical/financial\] advice.

978-1-098-13652-9

\[FILL IN\]

# Preface

The importance of adapting to and leveraging emerging technologies cannot overstate in the ever-evolving landscape of the modern business world. This book delves into cutting-edge developments and their transformative impact on how businesses operate, compete, and thrive. From ONNX and Rust to Mojo, Large Language Models (LLMs), ChatGPT, and Hugging Face, this book provides readers with a comprehensive understanding of the most recent technological advancements and their practical applications within the enterprise ecosystem.

The first chapter sets the stage by exploring the potential of emerging technologies, methodologies and their implications for businesses. Later chapters delve into how ONNX (Open Neural Network Exchange) has revolutionized how AI models are shared and deployed across various platforms. At the same time, Rust, a systems programming language, has garnered attention for its performance, reliability, and safety features. It also covers new languages like Mojo and the growing influence of LLMs like ChatGPT by OpenAI and StarCoder by Hugging Face. These technologies have transformed natural language processing, allowing more human-like interactions between users and machines. The chapter concludes with analyzing these technologies’ competitive advantages and how enterprises can leverage them for success in the rapidly changing global market.

Finally, in the appendix are interviews with many of the top names in cutting-edge AI and ML, as well as resources to help the reader get up to speed on ML certifications and the Rust language.

# Who Should Read This Book

This book is for enterprise developers and data scientists eager to delve into MLOps, focusing on emerging technologies such as ONNX, Rust, Mojo, Kubernetes LLMs, ChatGPT, and Hugging Face. The book aims to introduce alternative MLOps workflows that surpass the traditional Jupyter, Conda, Pandas, Numpy, and Sklearn stack. It offers a pathway to discovering more efficient, environmentally friendly, groundbreaking solutions in this domain.

The driving force behind this book is the desire to investigate MLOps and data science possibilities without depending on the conventional Jupyter, Conda, Pandas, Numpy, and Sklearn stack. We aim to uncover solutions emphasizing performance, simplicity, computational speed, and eco-friendliness. Given Rust’s growing popularity as one of the most loved and sought-after languages, we will examine its potential in the MLOps landscape and other emerging technologies.

# How This Book Is Organized

This book covers various aspects of MLOps, focusing on emerging technologies like ONNX, Rust, Kubernetes, LLMs, ChatGPT, and Hugging Face. The chapters are structured as follows: [Chapter 1, “MLOps - What is it, and why do we need it?”](#ch01.xhtml#Chapter1), provides an introduction to MLOps, its importance, and how it differs from DevOps. [Chapter 2, “The Stages of MLOps”](#ch02.xhtml#Chapter2), dives into the stages of MLOps, detailing the necessary components and processes involved in an MLOps project. [Chapter 3, “Getting Started with your First MLOps Project”](#ch03.xhtml#Chapter3), guides you through getting started with your first MLOps project, offering a framework for planning and justifying MLOps projects. [Chapter 4, “Working with Data and Feature Stores”](#ch04.xhtml#Chapter4), covers working with data and feature stores, discussing data access, versioning, and feature store ingestion. [Chapter 5, “Developing Models for Production”](#ch05.xhtml#Chapter5), focuses on developing models for production, including hyper-parameter tuning, AutoML, and training at scale. [Chapter 6, “Deployment of Models and AI Applications”](#ch06.xhtml#Chapter6), deals with the deployment of models and AI applications, exploring model registry, management, and real-time serving pipelines. Chapter 7 provides a step-by-step guide to building a production-grade MLOps project from start to finish with real code examples. [Chapter 7, “Building Scalable Deep Learning and Large Language Model Projects”](#ch07.xhtml#Chapter08), delves into building a scalable deep learning project, discussing storage foundations, data labeling, and deployment strategies. [Chapter 8, “Solutions for Advanced Data Types”](#ch08.xhtml#Chapter09), explores solutions for advanced data types such as time series, NLP, video analysis, and image classification. [Chapter 9, “Solutions for Common Use Cases”](#ch09.xhtml#Chapter10), offers solutions for common use cases like fraud prediction, predictive maintenance, and real-time recommendation engines. [Chapter 10, “Implementing MLOps using Rust”](#ch10.xhtml#Chapter11), examines the role of Rust in MLOps, its advantages, and its potential applications in the field. Each chapter includes critical thinking questions and exercises, perfect for team discussions, user groups, job interview preparation, or certification. The exercises help you learn by doing, applying the chapter’s context to working code examples that could make excellent portfolio pieces.

# Additional Resources

For those with access to the O’Reilly platform, an optional resource that may help you with your AWS journey is [52 Weeks of AWS-The Complete Series](https://oreil.ly/kDf0Q); it contains hours of walk-throughs on all significant certifications. This series is also available as a free podcast on all [major podcast channels](https://podcast.paiml.com).

In addition to those resources, there are daily and weekly updates on AWS from Noah Gift at the following locations:

Noah Gift O’Reilly
: 

On [Noah Gift’s O’Reilly profile](https://www.oreilly.com/pub/au/3039), there are hundreds of videos and courses covering a variety of technologies, including AWS, as well as frequent Live Trainings. Two other O’Reilly books covering AWS may also be helpful: [*Python for DevOps*](https://www.oreilly.com/library/view/python-for-devops/9781492057680) and [*Practical MLOps*](https://www.oreilly.com/library/view/practical-mlops/9781098103002).

Noah Gift Linkedin
: 

On [Noah Gift’s LinkedIn](https://www.linkedin.com/in/noahgift), he periodically streams training on AWS and in-progress notes on O’Reilly books.

Noah Gift Website
: 

[*Noahgift.com*](https://noahgift.com) is the best place to find the latest updates on new courses, articles, and talks.

Noah Gift GitHub
: 

On [Noah Gift’s GitHub profile](https://github.com/noahgift), you can find hundreds of repositories and almost daily updates.

Noah Gift AWS Hero Profile
: 

On [Noah Gift’s AWS Hero Profile](https://oreil.ly/FyFvf), you can find links to current resources involving work on the AWS platform.

Noah Gift Coursera Profile
: 

On [Noah Gift’s Coursera profile](https://www.coursera.org/instructor/noahgift), you can find multiple specializations covering a wide range of topics in cloud computing with a heavy emphasis on MLOPs, Data Engineering and MLOps. These courses include:

*   [DevOps, DataOps, MLOps](https://www.coursera.org/learn/devops-dataops-mlops-duke)
    
*   [MLOps Platforms: Amazon SageMaker and Azure ML](https://www.coursera.org/learn/mlops-aws-azure-duke)
    
*   [Open Source Platforms for MLOps](https://www.coursera.org/learn/open-source-mlops-platforms-duke)
    
*   [Python Essentials for MLOps](https://www.coursera.org/learn/python-essentials-mlops-duke)
    
*   [Cloud Computing Foundations](https://oreil.ly/XcxeC)
    
*   [Cloud Virtualization, Containers and APIs](https://oreil.ly/mcacu)
    
*   [Cloud Data Engineering](https://oreil.ly/pVHGm)
    
*   [Cloud Machine Learning Engineering and MLOps](https://oreil.ly/724Sz)
    
*   [Linux and Bash for Data Engineering](https://oreil.ly/Ps1Mz)
    
*   [Python and Pandas for Data Engineering](https://oreil.ly/07ywX)
    
*   [Scripting with Python and SQL for Data Engineering](https://oreil.ly/rSBMG)
    
*   [Web Applications and Command-Line Tools for Data Engineering](https://oreil.ly/gDDHy)
    
*   [Building Cloud Computing Solutions at Scale Specialization](https://oreil.ly/VZo7x)
    
*   [Python, Bash and SQL Essentials for Data Engineering Specialization](https://oreil.ly/TpIUU)
    

Pragmatic AI Labs Website
: 

Many free resources related to AWS are available at the [*Pragmatic AI Labs and Solutions website*](https://paiml.com). These include several free books:

*   [*Cloud Computing for Data Analysis*](https://oreil.ly/I4ADa)
    
*   [*Minimal Python*](https://oreil.ly/JD2Ha)
    
*   [*Python Command Line Tools: Design Powerful Apps with Click*](https://oreil.ly/7zxNa)
    
*   [*Testing in Python*](https://oreil.ly/uJ5cC)
    

# Conventions Used in This Book

The following typographical conventions are used in this book:

*Italic*
: 

Indicates new terms, URLs, email addresses, filenames, and file extensions.

`Constant width`
: 

Used for program listings, as well as within paragraphs to refer to program elements such as variable or function names, databases, data types, environment variables, statements, and keywords.

**`Constant width bold`**
: 

Shows commands or other text that should be typed literally by the user.

*`Constant width italic`*
: 

Shows text that should be replaced with user-supplied values or by values determined by context.

###### Tip

This element signifies a tip or suggestion.

###### Note

This element signifies a general note.

###### Warning

This element indicates a warning or caution.

# Using Code Examples

PROD: Please reach out to author to find out if they will be uploading code examples to oreilly.com or their own site (e.g., GitHub). If there is no code download, delete this whole section. If there is, when you email digidist with the link, let them know what you filled in for title\_title (should be as close to book title as possible, i.e., learning\_python\_2e). This info will determine where digidist loads the files. 

Supplemental material (code examples, exercises, etc.) is available for download at [*https://github.com/nogibjj/rust-mlops-template*](https://github.com/nogibjj/rust-mlops-template).

If you have a technical question or a problem using the code examples, please send email to [*bookquestions@oreilly.com*](mailto:bookquestions@oreilly.com).

This book is here to help you get your job done. In general, if example code is offered with this book, you may use it in your programs and documentation. You do not need to contact us for permission unless you’re reproducing a significant portion of the code. For example, writing a program that uses several chunks of code from this book does not require permission. Selling or distributing examples from O’Reilly books does require permission. Answering a question by citing this book and quoting example code does not require permission. Incorporating a significant amount of example code from this book into your product’s documentation does require permission.

We appreciate, but generally do not require, attribution. An attribution usually includes the title, author, publisher, and ISBN. For example:

If you feel your use of code examples falls outside fair use or the permission given above, feel free to contact us at [*permissions@oreilly.com*](mailto:permissions@oreilly.com).

# O’Reilly Online Learning

###### Note

For more than 40 years, [*O’Reilly Media*](https://oreilly.com) has provided technology and business training, knowledge, and insight to help companies succeed.

Our unique network of experts and innovators share their knowledge and expertise through books, articles, and our online learning platform. O’Reilly’s online learning platform gives you on-demand access to live training courses, in-depth learning paths, interactive coding environments, and a vast collection of text and video from O’Reilly and 200+ other publishers. For more information, visit [*https://oreilly.com*](https://oreilly.com).

# How to Contact Us

Please address comments and questions concerning this book to the publisher:

*   O’Reilly Media, Inc.
*   1005 Gravenstein Highway North
*   Sebastopol, CA 95472
*   800-998-9938 (in the United States or Canada)
*   707-829-0515 (international or local)
*   707-829-0104 (fax)

Don't forget to update the link above. 

Email [*bookquestions@oreilly.com*](mailto:bookquestions@oreilly.com) to comment or ask technical questions about this book.

For news and information about our books and courses, visit [*https://oreilly.com*](https://oreilly.com).

Find us on LinkedIn: [*https://linkedin.com/company/oreilly-media*](https://linkedin.com/company/oreilly-media).

Follow us on Twitter: [*https://twitter.com/oreillymedia*](https://twitter.com/oreillymedia).

Watch us on YouTube: [*https://youtube.com/oreillymedia*](https://youtube.com/oreillymedia).

# Acknowledgments

## Noah

It is always an honor to have the opportunity to work on an O’Reilly book. This book marks my fifth O’Reilly and likely my last technical book as I shift to other writing and content creation forms. Thank you to everyone I worked with at O’Reilly, including current and former editors and collaborators and collaborators and authors of the recent book.

Also, thanks to many of my current and former students, faculty, and staff at [Duke MIDS](https://oreil.ly/2SWqF), [Duke Artificial Intelligence Masters in Engineering](https://oreil.ly/bTcbr), as many ideas in this book came from courses I taught, and questions brought up from students.

Finally, thank you to my family, Leah, Liam, and Theodore, who put up with me working on weekends and late at night to hit deadlines.

# Chapter 1. MLOps - What is it, and why do we need it?

At the root of inefficient systems is an interconnected web of incorrect decisions that compound over time. It is tempting to look for a silver bullet fix to a system that doesn’t perform well, but that strategy rarely, if ever, pays off. Consider the human body; there is no shortage of quick fixes sold to make you healthy, but the solution to health longevity is more tricky because it requires a systematic approach^([1](#ch01.xhtml#idm46247718218256)). We want to hear that a magic pill will make us healthy instead of developing habits that lead to optimal health.

Similarly, there is no shortage of advice on “getting rich quick.” Here again, the data conflicts with what we want to hear. In the book *Don’t Trust Your Gut (HarperCollins, 2022)*, Seth Stephens-Davidowitz shows that 84% of the top .1 percent of earners receive at least some money from owning a business. Further, the average age of a business founder is about 42, and some of the most successful companies are real estate or automobile dealerships. These are hardly get-rich-quick businesses but businesses that require significant skill, expertise, and wisdom through life experience.

Cities are another example of complex systems that don’t have silver bullet fixes. Recently WalletHub created a list of best-run cities in America. San Francisco ranked 149 out of 150 despite having many theoretical advantages over other cities, like beautiful weather, being home to the top tech companies in the world, and a 2022-2023 budget of 14 billion dollars for a population of [842 thousand people](https://www.sfchronicle.com/projects/2022/san-francisco-budget/). The budget is similar to the entire country of Panama, with a population of [4.4 million people](https://www.economy.com/panama/government-budget-balance). As the case of San Francisco shows, revenue or natural beauty alone isn’t enough to have a well-run city; there needs to be a comprehensive plan, i.e., execution and strategy matter. No single solution is going to either make or break a city. The WalletHub survey points to extensive criteria for a well-run city, including infrastructure, economy, safety, health, education, and financial stability.

Similarly, with MLOps, searching for a single answer to getting models into production, perhaps by getting better data or using a specific deep learning framework, is tempting. Instead, just like these other domains, it is essential to have an evidence-based, comprehensive strategy.

# What Is MLOps?

At the heart of MLOps is the continuous improvement of all business activity. The Japanese automobile industry refers to this concept as *kaizen*, meaning literally “improvement.” For building production machine learning systems, this manifests itself in both the noticeable aspects of improving the model’s accuracy as well the entire ecosystem supporting the model.

A great example of one of the non-obvious components of the machine learning system is the business requirements. If the company needs an accurate model to predict how much inventory to store in the warehouse, but the data science team creates a computer vision system to keep track of the inventory already in the warehouse, the wrong problem is solved. No matter how accurate the inventory tracking computer vision system is, the business asked for a different requirement, and it cannot meet the goals of the organization as a result.

So what is *MLOps*? A compound of *Machine Learning* (ML) and *Operations* (Ops), MLOps is the processes and practices for designing, building, enabling, and supporting the efficient deployment of ML models in production, to continuously improve business activity. Similar to *DevOps*, MLOps is based on automation, agility, and collaboration to improve quality. If you’re thinking CI/CD, you’re not wrong. MLOps supports CI/CD. According to Gartner, “MLOps aims to standardize the deployment and management of ML models alongside the operationalization of the ML pipeline. It supports the release, activation, monitoring, performance tracking, management, reuse, maintenance, and [governance of ML artifacts](https://www.gartner.com/document/4000966).”

## MLOps in the Enterprise

There are substantial differences between an enterprise company and a startup company. Scott Shane, an entrepreneurship expert, wrote in *The Illusions of Entrepreneurship (Yale University Press, 2010)* “that only one percent of people work in companies less than two years old, while 60 percent work in companies more than ten years old.” Longevity is a characteristic of the enterprise company.

He also says, “it takes 43 startups to end up with just one company that employs anyone other than the founder after ten years.” In essence, the enterprise builds for scale and longevity. As a result, it is essential to consider technologies and services that support these attributes.

###### Note

Startups have technological advantages for users, but they also have different risk profiles for the investors versus the employees. Venture capitalists have a portfolio of many companies, diversifying their risk. According to [FundersClub](https://fundersclub.com/learn/guides/vc-101/understanding-venture-capital), a typical fund “contains 135 million” and is “spread between 30-85 startups.” Meanwhile, startup employees have their salary and equity invested in one company.

Using the [expected value](https://www.wolframalpha.com/input/?i=expected+value+calculator) to generate the actual equity value at a probability of 1/43, an enterprise offering a yearly 50k bonus returns 200k at year four. A startup produces $4,651.16 in year four. For most people, on average, startups are a risky decision if judged on finance alone. However, they might offer an excellent reward via an accelerated chance to learn new technology or skills with the slight chance of a huge payout.

On the flip side, if a startup’s life is dynamic, it must pick very different technology solutions than the enterprise. If there is a 2.3% chance a startup will be around in 10 years, why care about vendor lock-in or multi-cloud deployment? Only the mathematically challenged startups build what they don’t yet need.

Likewise, if you are a profitable enterprise looking to build upon your existing success, consider looking beyond solutions that startups use. Other metrics like the ability to hire, enterprise support, business continuity, and price become critical KPIs (Key Performance Indicators).

## Understanding ROI in Enterprise Solutions

The appeal of a “free” solution is that you get something for nothing. In practice, this is rarely the case. In [Figure 1-1](#ch01.xhtml#Figure-1.2-value-roi) there are three scenarios presented. In the first scenario, the solution costs nothing but delivers nothing, so the ROI is zero. In a second scenario, there is high value at stake, but the cost exceeds the value, resulting in a negative ROI. In the third scenario, a value of one million with a cost of 1/2 million delivers 1/2 of a million in value.

![](assets/1.2-value-roi.png)

The big takeaway is that cost alone shouldn’t be the driver of technology platform solutions. The best choice isn’t free, but is the solution that delivers the highest ROI since this ROI increases the velocity of the profitable enterprise. Let’s expand on the concept of ROI even more by digging into bespoke solutions, which in some sense are also “free” since an employee built the solution.

In [Figure 1-2](#ch01.xhtml#Figure-1.3-bespoke-system), a genuinely brilliant engineer convinces management to allow them to build a bespoke system that solves a particular problem for the Fortune 100 company. Despite skepticism from some people, the engineer not only delivers quickly, but the system exceeds expectations. It would be tempting to think this is a success story, but it is actually a story of failure. One year later, the brilliant engineer gets a job offer from a trillion-dollar company and leaves. About three months later, the system breaks, and no one is smart enough to fix it. The company reluctantly replaces the entire system and retrains the company on the new proprietary system.

![](assets/1.3-bespoke-system.png)

The ultimate cost to the organization is the lack of momentum from using a superior system for a year, alongside the training time necessary to switch from the old system to the new system. A “free” solution with positive ROI can have long-term negative ROI for an organization. This scenario is not only not hypothetical, but you may have seen it yourself^([2](#ch01.xhtml#idm46247715452016)).

In the book Fooled by Randomness: The Hidden Role of Chance in Life and the Markets (Random House, 2008), the author Nassim Taleb mentions, “it does not matter how frequently something succeeds if failure is too costly to bear.” This statement directly applies to a successful enterprise that wants to implement MLOps. Taking the right kind of strategic risk is of critical importance. In the following section, let’s discuss the concept of risk in more detail.

## Understanding Risk and Uncertainty in the Enterprise

Not all risk is the same, just as not all uncertainty is the same. Unlike a startup, an enterprise has made it to the survival phase. There are some risks that enterprises do not need to take. In a book about the enterprise, *Good to Great (Harper Business, 2011)*, Jim Collins asks, “How do good-to-great organizations think differently about technology?” He found that in every case of a “good-to-great” company, they found technological sophistication and became a pioneer in applying technology. Further, he states that technology is an accelerator, not a creator, of momentum.

###### Note

Mark Spitznagel makes a case for considering the geometric mean in financial investment in the book Safe Haven (Wiley, 2021). He states, “Profit is finite. Risk is infinite”. The percentage of your wealth you can lose is more important than the absolute value of the wealth you could lose when investing. This fact is a point well suited to the enterprise. Why take a risk with unbounded loss?

Collins’ key point about technology directly applies to MLOps in the enterprise. The purpose of machine learning is to accelerate the business value that is already there. The reason to use machine learning isn’t to pivot the organization to becoming machine learning researchers competing with companies that specialize in research; it is to accelerate the strategic advantages of the organization through technology.

The calculated risk of adopting machine learning as a business accelerator is acceptable if done in a manner that allows an organization to limit the downsides of technology change management. There is essentially unbounded risk in a company creating bespoke machine learning solutions and platforms when its core strength is in some other industry, such as manufacturing, hospitality, or financial services.

Many options exist to accelerate technological advancement in the enterprise, including using pre-trained models like [Hugging Face](https://huggingface.co) or [TensorFlow Hub](https://tfhub.dev), [computer vision APIs like AWS Rekognition](https://aws.amazon.com/rekognition/), or open-source AutoML solutions like [Ludwig](https://ludwig-ai.github.io/ludwig-docs/0.5/) or MLOps orchestration frameworks like [MLRun](https://www.mlrun.org/). Enterprises that adopt MLOps with an approach of using the right level of abstraction give themselves a “good-to-great” advantage over organizations that “hired 15 data scientists” who do “research.” In the latter example, it is often the case that after years of research, in the best case, nothing is done, but in the worst case, a lousy solution creates a worse outcome than doing nothing.

Frank Knight, a professor from the University of Chicago, clearly [articulates the difference](https://archive.org/details/riskuncertaintyp00knigrich) between risk and uncertainty. He goes on to state that reward for taking a known risk is much different than a risk where it is immeasurable and impossible to calculate. This form of risk is called *Knightian uncertainty* after Frank Knight. An enterprise doing machine learning should deeply consider which risk they are taking, a regular risk that is knowable, or they are embarking on a path with Knightian uncertainty. In almost all cases, it is better to take knowable risks in machine learning and AI since technology is not the creator of growth; instead, it is the accelerator.

Knowing that acceleration is the crucial insight into great companies that use technology, let’s look at some of the differences in technology acceleration differences between MLOps and DevOps.

## MLOps vs. DevOps

Without DevOps, you cannot do MLOps. DevOps is a foundational building block for doing MLOps, and there is no substitute. DevOps is a methodology for releasing software in an agile manner while constantly improving the quality of both business outcomes and the software itself. A high-level DevOps practitioner has much in common with a master gourmet chef. The chef has deep knowledge of ingredients and years of practical experience creating beautiful and delicious meals, and they can make these meals in an industrialized and repeatable manner. The repeatability allows a restaurant to stay open and earn a profit.

Similarly, with DevOps, an expert in the domain has detailed knowledge of how to build software and deploy it in a high-quality and repeatable manner. One of the biggest challenges for experts in data science to transition to MLOps is a lack of experience doing DevOps. There is no substitute for experience; many data science practitioners and machine learning researchers should get experience building and deploying software with the DevOps methodology to get the foundational knowledge and experience necessary to be an expert at MLOps.

###### Note

You can learn more about DevOps from the book *Python for DevOps (O’Reilly, 2019)*, also available on the [O’Reilly platform](https://learning.oreilly.com/library/view/python-for-devops/9781492057680/).

There are apparent differences, though, between traditional DevOps vs. MLOps. One clear difference is the concept of *data drift*; when a model trains on data, it can gradually lose usefulness as the underlying data changes. A tremendous theoretical example of this concept comes from Nassim Taleb in the book, *Fooled by Randomness, Random House, 2001*, where he describes how a “naughty child” shown in [Figure 1-3](#ch01.xhtml#Figure-1.4-data-drift-taleb) could disrupt the understanding of the underlying distribution of red vs. black balls in a container.

![](assets/1.4-data-drift-taleb.png)

In a static condition, the more balls pulled from a container, the more confident a person can be of the underlying distribution of red vs. black balls. In a dynamic condition, if the balls are constantly changed, then a model trained on an older data version won’t be accurate. This example effectively captures one of many unique elements specific to MLOps not found in DevOps.

###### Note

Microsoft [notes](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-monitor-datasets?tabs=python#what-is-data-drift), “Data drift is one of the top reasons model accuracy degrades over time.”

The takeaway is that DevOps is a necessary foundation for MLOPs, but MLOps’ additional requirements, like data drift, don’t appear in traditional DevOps.

## What Isn’t MLOps?

One way to understand more about MLOps is to define what it is not. Here are some common MLOps anti-patterns:

*   Hiring a team of data scientists and hoping for the best: Perhaps the most common of the MLOps anti-patterns is hiring a team of data scientists and expecting an excellent solution to appear. Without organization support that understands MLOps and technology infrastructure to support them, there will not be an ideal outcome.
    
*   Only building bespoke machine learning solutions: A fundamental problem with only building customized solutions is that they may not be necessary for an organization’s business goals. Training a bespoke machine learning model on propriety data for a self-driving company is essential to a competitive advantage. Training a similar model for a Fortune 500 delivery company could be a costly experiment adding no real value to the business.
    
*   Dismissing DevOps importance: Teams that work in silos are not following the best practices of DevOps. It is impractical to have a data science team in, say “Texas” that builds models in R, then “throws them over to the DevOps team to put into the software stack in the financial district of San Francisco in Python.”
    

Ultimately, MLOps requires a business and production-first mindset. The purpose of machine learning is to accelerate business value. This fact means the teams building solutions must be agile in their approach to solving machine learning problems.

# Mainstream Definitions of MLOps

A challenging problem in technology is separating a marketing strategy from a technology strategy. In the case of MLOps, it is not a marketing strategy; it is a specific solution to a severe problem in the enterprise. The bottom line is that models are not making it into production; if they do, they are brittle and fall apart when faced with the complexities of the actual world. Various surveys show that 50-70% of organizations have failed to deliver AI pilots or models to [production](https://services.google.com/fh/files/misc/practitioners_guide_to_mlops_whitepaper.pd).

With the condition identified, let’s find the cure. The cure needs to address the following key issues (among others):

*   model deployment and development time
    
*   the collaboration between different teams
    
*   operational excellence of ML systems
    
*   data governance
    
*   enhancing the ROI of the enterprise deploying the model.
    

One minimalist way to then define MLOps is that it supports ML development like DevOps supports software development.

## What Is ML Engineering

One way to define ML engineering is to look at popular certifications. [Google Machine Learning Engineer](https://cloud.google.com/certification/machine-learning-engineer) explains the following criteria for a professional machine-learning engineer:

*   Frame ML problems: [Which model to choose](https://developers.google.com/machine-learning/problem-framing/ml-framing?hl=en) depends on business constraints and the context. For example, a business may decide to classify damaged shipped boxes vs. successfully delivered packages. In that context, a classification model would be more appropriate than a regression model.
    
*   Architect ML solutions: An ML engineer develops a solution to solve the correctly framed using machine learning alongside other team members.
    
*   Design data preparation and processing systems: Two [critical steps](https://developers.google.com/machine-learning/data-prep) in data preparation and processing are constructing the dataset and then transforming the data.
    
*   Develop ML models: The [detailed modeling process](https://developers.google.com/machine-learning/problem-framing/implement-model?hl=en) involves a team or individual that creates a model correctly suited to initial model framing.
    
*   Automate and orchestrate ML pipelines: A [pipeline](https://developers.google.com/machine-learning/guides/rules-of-ml#ml_phase_i_your_first_pipeline) serves to create a process for reproducible and maintainable ML.
    
*   Monitor, optimize, and maintain: It is better to be proactive vs. reactive in building complex systems. Building monitoring allows for a [proactive approach](https://developers.google.com/machine-learning/guides/rules-of-ml#monitoring) to maintaining ML systems.
    

###### Note

Several O’Reilly books discuss machine learning engineering, including [*Data Science on the Google Cloud Platform*](https://learning.oreilly.com/library/view/data-science-on/9781491974551/), [*Machine Learning Design Patterns*](https://learning.oreilly.com/library/view/machine-learning-design/9781098115777/) and [*Practical MLOps*](https://learning.oreilly.com/library/view/practical-mlops/9781098103002/).

ML engineering aims to build high-quality ML models that solve specific business problems while creating ROI.

## MLOps and Business Incentives

A classic problem in business school is incentives. One way to put incentives is “who moved the cheese?” This scenario refers to a rat in a maze that only moves depending on where the cheese is. Similarly, there are two common incentives worth discussing in MLOPs: hiring data scientists without regard for ROI and negative externalities.

*   Negative Externalities: Negative externalities, like a company creating a profit from manufacturing but dumping toxic waste into a river and hurting downstream citizens of the town are classic examples of the fundamental problems in capitalism. In machine learning, the negative externalities could be unfairly biased algorithms that send an innocent person to jail or deny a person credit based on federally protected laws like the equal credit opportunity act, which makes it illegal for a creditor to discriminate in any aspect of a credit transaction based based on race, color, religion, national origin, and other categories. Even an unintentionally created bias in a model is still illegal, i.e., denying credit based on age. Enterprises that fail to look into the future could expose themselves to existential risk if system bias against elderly applications, for example, were accidentally baked into a machine learning model.
    
*   Hiring data scientists without regard for ROI: It has recently been in vogue to “hire data scientists” without regard for what problem they are solving. As discussed earlier, this strategy ultimately doesn’t work because models are not in production at most organizations doing AI and ML.
    

# Technical & Organizational Challenges - Why Most Models Never Make it to production

# MLOps in the Cloud

There are several critical advantages of cloud computing that MLOps methodology leverages. First, the cloud is an elastic resource that enables both the efficient use of computing and storage and the ability to scale to meet almost any demand. This capability means that cloud computing has on-demand access to essentially infinite resources.

Second, the cloud has a network effect in that cloud technologies benefit from integrating other cloud technologies. A great example is AWS Lambda, a serverless technology. AWS Lambda is a valuable service to build applications with not because of what it does alone but because of the deep integration with other AWS services like AWS Step Functions, AWS Sagemaker, or AWS S3. For any active cloud platform, you can assume that the integrated network of services further strengthens its capabilities as the platform develops more features.

Third, all cloud vendors have MLOps platforms. AWS has [Sagemaker](https://aws.amazon.com/sagemaker/studio/). Azure has [Azure Machine Learning](https://azure.microsoft.com/en-us/services/machine-learning/), and [Google has Vertex AI](https://cloud.google.com/vertex-ai); even smaller niche clouds like Alibaba Cloud have platforms like their [Machine Learning Platform for AI](https://www.alibabacloud.com/product/machine-learning). By using a cloud platform, an organization will likely use some of the offerings of the native ML platform and potentially augment it with custom solutions and third-party solutions.

Fourth, all cloud vendors have Cloud Development Environments. A significant trend is the use of a combination of lightweight CloudShell environments like [AWS CloudShell](https://aws.amazon.com/cloudshell/), heavier full IDE (Interactive Development Environment) options like [AWS Cloud9](https://aws.amazon.com/cloud9), and notebook environments, both free like [Sagemaker Studio Lab](https://aws.amazon.com/sagemaker/studio-lab/) or [Google Colab](https://research.google.com/colaboratory/) and those with rich IDE integration like [Sagemaker Studio](https://aws.amazon.com/sagemaker/studio/).

Finally, depending on what a company is doing, it may have no option but to use cloud computing. Some cloud computing components are a hard requirement for organizations specializing in building bespoke deep learning solutions because deep learning requires extensive storage and compute capabilities.

In addition to the public cloud vendors, several additional players offer MLOps solutions in the cloud (see later in this section). These vendors can operate on the public cloud or on private clouds. The advantage of using a smaller vendor is the customization level that such a company provides its customers. In addition, an MLOps vendor will have more in-depth expertise in MLOps since that is its only focus. Integrated vendors often ensure more relevant features and many more integrations. Finally, by choosing a vendor that is agnostic to a specific cloud provider, you, as a customer, aren’t connected to it either. Instead, you can use the vendor across multiple clouds or on additional infrastructure that you may have (see later in this section).

###### Note

One helpful resource for machine learning vendor analysis is the AI Infrastructure Alliance (AIIA). This organization provides data scientists and engineers with clarity and information about AI/ML tools to build robust, scalable, end-to-end enterprise platforms. One resource is a comprehensive MLOps landscape that maps out all the players in the industry. This document includes an updated MLOps landscape that will map out open-source and enterprise solutions for MLOps. The new landscape will encompass multiple categories and hundreds of companies while detailing the capabilities of each vendor solution. [The following is a link to](https://ai-infrastructure.org/the-new-landscape-of-machine-learning-for-the-new-year/) the existing AIIA landscape.

In [Figure 1-4](#ch01.xhtml#Figure-1.8-cloud-landscape-mlops), notice a typical pattern amongst all clouds in which there is a set of cloud development environments, flexible storage systems, elastic compute systems, serverless and containerized managed services, and third-party vendor integration.

![](assets/1.8-cloud-landscape-mlops.png)

Let’s break these categories down into more detail:

*   Cloud development environments: generally, developer-centric tools like cloud shells and IDEs are on one extreme, and machine learning-centric tools on the other. Storage query tools like [Google BigQuery](https://cloud.google.com/bigquery), [Amazon Athena](https://aws.amazon.com/athena), or [Azure Databricks Integration](https://azure.microsoft.com/en-us/services/databricks/) are in the middle.
    
*   MLOps platforms that operate in the cloud: MLOps platforms are built specifically for running MLOps for enterprises on the cloud or across any environment. Solutions like [Iguazio](http://iguazio.com/), [Valohai](https://valohai.com/), [DataRobot](https://www.datarobot.com/), [Azure Databricks](https://www.databricks.com/) and [Outerbounds](https://outerbounds.com/) and many others offer a wide variety of MLOps solutions for the enterprise.
    
*   Elastic storage systems and elastic computing systems: Deep learning systems thrive on big data, and flexible compute capabilities from GPUs, CPUs, and AI Accelerator ASICs (application-specific integrated circuits) like TPU (Tensor Processing Units). As a result, MLOps platforms, both native and 3rd party, heavily use this elastic capability to provide managed solutions.
    
*   Serverless and containerized managed services: Cloud platforms evolve towards more serverless solutions like [AWS Lambda](https://aws.amazon.com/lambda/) or [Google Cloud functions](https://cloud.google.com/functions) and solutions with fully managed containerized solutions such as Google [Cloud Run](https://cloud.google.com/run) or [AWS Fargate](https://aws.amazon.com/fargate/). These managed services, in turn, have deep platform integration, which enhances the value proposition of the cloud platform through a network effect.
    
*   Third-party vendor integrations: A cloud platform can’t have the exact right mix of everything and at the right quality. A trip to a large warehouse store yields a wide variety of offerings at a reasonable price. On the other hand, though they may not have the authentic gourmet food you like or the exact appliance features you need, a cloud provider cannot go deep on everything. As a result, 3rd party integrations handle these specialized or advanced use cases.
    

With the common aspects of cloud computing for MLOps covered, let’s move on now to discuss the cloud environments in more detail.

## Key Cloud Development Environments

One of the best new products from Microsoft is [Github CodeSpaces](https://github.com/features/codespaces). It is a cloud-based development environment with many customizable features and a great place to practice MLOps. In particular, what is helpful about this environment is the deep integration with GitHub and the ability to customize it with a specialized runtime. Finally, the synergy with [Github Actions](https://docs.github.com/en/actions) allows for a great CI/CD story.

###### Note

You can learn more about GitHub CodeSpaces with the following O’Reilly videos:

*   [Building with the GitHub EcoSystem: Copilot, CodeSpaces, and GitHub Actions](https://learning.oreilly.com/videos/building-with-the/10052022VIDEOPAIML/)
    
*   [GitHub Codespaces and custom dotfiles](https://learning.oreilly.com/videos/github-codespaces-and/50142VIDEOPAIML/)
    
*   [Compiling Python from scratch with Github Codespaces](https://learning.oreilly.com/videos/compiling-python-from/062212022VIDEOPAIML/)
    
*   [GitHub Copilot Driven: Python DevOps from Functions to Continuous Delivery of Microservices on AWS](https://learning.oreilly.com/videos/github-copilot-driven/10182022VIDEOPAIML/)
    
*   [GitHub Codespaces Course](https://learning.oreilly.com/videos/github-codespaces-course/27724023VIDEOPAIML/)
    

There are three different flavors of cloud-based developments available from Google. The first is [Colab Notebooks](https://colab.research.google.com), the second is [Google Cloud Shell](https://cloud.google.com/shell), and the third is [Google Cloud Shell Editor](https://cloud.google.com/shell/docs/editor-overview).

In [Figure 1-5](#ch01.xhtml#Figure-1.5-google-cloud-editor-default) there is a full editor available for GCP.

![](assets/1.5-google-cloud-editor-default.png)

In [Figure 1-6](#ch01.xhtml#Figure-1.6-google-cloud-editor-api) API docs integrate with the development environment.

![](assets/1.6-google-cloud-editor-api.png)

In [Figure 1-7](#ch01.xhtml#Figure-1.7-google-cloud-shell), the terminal shows a standard view of the experience using the cloud shell.

![](assets/1.7-google-cloud-shell.png)

###### Note

You can learn more about Colab notebooks with the following O’Reilly videos:

*   [*https://learning.oreilly.com/videos/data-science-on/01222022VIDEOPAIML/*](https://learning.oreilly.com/videos/data-science-on/01222022VIDEOPAIML/)\[Data Science On Your First Day with Python
    
*   [Python for Data Science with Colab and Pandas in One Hour Video Course](https://learning.oreilly.com/videos/python-for-data/62062021VIDEOPAIML/)
    

[What are Google Colab Notebooks, and How do you share them for Data Science projects?](https://learning.oreilly.com/videos/what-is-google/01142022VIDEOPAIML/)

Finally, the AWS platform has cloud shell environments, as shown in [Figure 1-8](#ch01.xhtml#Figure-1-9-cloudshell-cloud9).

![](assets/1-9-cloudshell-cloud9.png)

###### Note

One quick way to learn about multiple clouds simultaneously is by setting up a multi-cloud continuous integration. You can learn how to set this up with [GitHubActions in this O’Reilly video](https://learning.oreilly.com/videos/github-actions-hello/011812022VIDEOPAIML/).

All of this leads to the concept of the cloud-developer workspace advantage, as shown in [Figure 1-9](#ch01.xhtml#Figure-1.14-cloud-workspaces). A laptop or workstation is expensive and non-deterministic due to pre-installed software and, by definition, not the deploy target. When you look at a cloud-based workspace, it has many incredible advantages, including power, disposability, pre-loading, and deep integration with advanced tools.

![](assets/1.14-cloud-workspaces.png)

###### Note

You learn more about the cloud-developer workspace advantage in the following O’Reilly video [Cloud Developer Workspace Advantage](https://learning.oreilly.com/videos/52-weeks-of/080232022VIDEOPAIML/080232022VIDEOPAIML-c1_s4/) or on [YouTube](https://www.youtube.com/watch?v=5gw30mFmDzc).

## The Key Players in Cloud Computing

Know someone that wants to earn 200k or more a year? According to the [2022 Cloud Salary Survey](https://get.oreilly.com/rs/107-FMS-070/images/2022-Cloud-Salary-Survey.pdf?mkt_tok=MTA3LUZNUy0wNzAAAAGF3zSlFTeLd278VcGMchi3YXTWozijVa0a0VMJOLv2wKHAC_ymMLgXwgqNFr1g8Rd-QAtOlsehKlghdBQHO5f_tC0SEwwEbDO1o2dO-5N7uoGflw) by Mike Louikides at O’Reilly, the average salary for certified professionals on AWS, Azure, and GCP is over 200k.

Further backing this up is the data from Statista, as shown in [Figure 1-10](#ch01.xhtml#Figure-1-0-0-cloud-market). As of Q4 2021, there are three key players in the worldwide market. AWS has about 33% of the market share, Azure has about 21%, and Google Cloud has about 10%. Combined, these three vendors have 2/3 of a market that generates almost 200 billion in revenue. Service revenue increased in size by 37% from the last year.

![](assets/1.0-cloud-market18819.png)

A reasonable strategy for an organization wishing to use cloud computing is to use the platform of the largest providers. The Matthew^([3](#ch01.xhtml#idm46247717831424)) effect saying, “the rich get richer, and the poor get poorer,” applies to cloud computing for several reasons.

*   Available Employees and Vendors to Hire: Leveraging the most prominent cloud platforms makes hiring employers and finding vendors that work with the platform more accessible.
    
*   Training material available: The availability of training material for the most prominent platforms makes it easier to train employees.
    
*   Services Available: Larger platforms can hire more software engineers and product managers, meaning you can count on a continuation of new features and maintenance in their platform.
    
*   Cost of Service: Economies of scale mean that the most significant providers benefit the most from economies of scale. They can leverage pricing advantages by buying in bulk and then passing them down to the customer.
    

###### Note

You can study for the AWS Cloud Certifications by viewing the [AWS Solutions Architect Professional Course](https://learning.oreilly.com/videos/aws-solutions-architect/021222022VIDEOPAIML/) and [AWS Certified Cloud Practitioner Video Course](https://learning.oreilly.com/videos/aws-certified-cloud/60644VIDEOPAIML) on the O’Reilly platform and authored by [Noah Gift](https://www.oreilly.com/pub/au/3039).

Now that the top providers in cloud computing are known, let’s discuss how each vendor views the world of cloud computing as it relates to MLOps.

### AWS View of cloud computing as it relates to MLOps

The best place to get a high-level summary of AWS cloud computing is the [Overview of Amazon Web Services AWS Whitepaper](https://docs.aws.amazon.com/whitepapers/latest/aws-overview). In particular, they mention [six advantages of cloud computing](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/six-advantages-of-cloud-computing.xhtml).

###### Note

You can learn more about AWS in the O’Reilly book “Developing on AWS with C# (O’Reilly 2022)”, also available on the [O’Reilly platform](https://learning.oreilly.com/library/view/developing-on-aws/9781492095866/).

*   Trade fixed expense for variable expense: By avoiding large capital expenditures, it encourages agility and efficiency.
    
*   Benefit from massive economies of scale: As prices decrease for the supplier, they fall for the customer allowing for lower pricing than if the customer bought the same product. Similarly, managed services on the platform will have a steady schedule of new features.
    
*   Stop guessing capacity: There isn’t a need to pre-provision resources since systems get built with an elastic ability to scale as needed.
    
*   Increase speed and agility: Focusing on an organization’s comparative advantage and not building non-essential-to-business IT allows an organization to move faster.
    
*   Stop spending money running and maintaining data centers: Cost savings accumulate from outsourcing this component of IT.
    
*   Go global in minutes: Going global is a highly challenging problem that goes away with AWS due to its comprehensive offerings.
    

These features ultimately drive into the core MLOPs offering of Amazon Sagemaker in [Figure 1-11](#ch01.xhtml#Figure-1-9-cloudshell-cloud92) as the project’s lifecycle goes from preparation to building to training, to finally deploying and managing the solution. At the center of the workflow is tight integration with developer tools from Studio and RStudio.

![](assets/1-10-sagemaker.png)

###### Note

On the O’Reilly platform video, you can see a complete walkthrough of Sagemaker Studio Lab [Sagemaker Studio Lab](https://learning.oreilly.com/videos/aws-sagemaker-studio/12162021VIDEOPAIML/).

With the AWS view of the MLOPs complete, let’s look at Azure next.

### Azure View of Cloud Computing as it relates to MLOps

Microsoft Azure [sees the world of MLOPs](https://docs.microsoft.com/en-us/learn/paths/introduction-machine-learn-operations/) as a way to “efficiently scale from a proof of concept or pilot project to a machine learning workload in production.” In [Figure 1-12](#ch01.xhtml#Figure-1-1-Azure-MLOps), the model’s lifecycle goes from training, packaging, validating, deploying, monitoring, and retraining.

![](assets/1.1-azure-mlops.png)

With Azure MLOps view of MLOps covered, let’s next look at how Google views MLOps.

### GCP View of Cloud Computing as it relates to MLOPs

An ideal place to look at how Google sees the world is by looking through the [production-ml-systems crash course](https://developers.google.com/machine-learning/crash-course/production-ml-systems). One of the items the company points out is how tiny the actual modeling part of the problem is in [Figure 1-13](#ch01.xhtml#Figure-1-11-ml-problem) instead, the combination of other tasks, including data collection, serving infrastructure, and monitoring, take up much more of the problem space.

![](assets/1-11-ml-problems.png)

Ultimately this leads to how Google’s [Vertex AI platform](https://cloud.google.com/vertex-ai) handles the MLOps workflow in [Figure 1-14](#ch01.xhtml#Figure-1-12-vertex-ai): The ML development process occurs, including model framing for the business problem. The data processing phase leads to an operationalized training process that can scale up as needed. Then the model deployment occurs along with a workflow orchestration alongside artifact organization. The model has monitoring baked into the deployment process.

![](assets/1-12-vertex-ai.png)

While public cloud providers offer their own solutions, sometimes enterprises might need a solution that is more tailored to their specific needs. Let’s look at two more deployment options: on-premises deployment and hybrid cloud deployment.

# MLOps On-Premises

In some use cases, enterprises cannot use the public cloud. Business restrictions like the need to secure sensitive data or having to adhere to strict regulations (e.g data localization privacy regulations) require an MLOps solution that can operate on-premises. Many MLOps solutions offer the ability to deploy them either in the cloud or on-premises. The only downside to this approach is that on-premises solutions require the enterprise to provide the servers and equipment that will support the intense computing power needed to run ML algorithms at scale. They will also need to update and maintain the infrastructure.

On the other hand, an on-premises deployment will almost certainly require some sort of customization. This installation gives enterprises more control over the product, and they can make specific requests to tailor it to their needs. More specifically, if the deployed solution is a startup solution, they will be attentive and work hard to ensure satisfaction and adoption. If it’s an open-source product, then enterprises cannot only leverage the community’s development power but also go inside with their own developers and tinker with the product to ensure it suits their needs.

# MLOps in Hybrid Environments

Similar to on-premises deployment, some enterprises might prefer a hybrid cloud deployment. This would involve deploying on the public cloud(s), on-premises, and perhaps even on a private cloud or on edge devices. Naturally, this makes things a lot more complex, since the MLOps solution must enable total separation of the data path from the control path and must be delivered by a highly available, scalable entity that orchestrates, tracks and manages ML pipelines across types of infrastructure deployments. Lest we forget, this has to occur at high speed and with optimal performance. Finally, the solution would ideally provide a single development and deployment stack for engineers across all infrastructure types.

Finding a vendor or open source solution that answers all these requirements might not be as simple, but as mentioned before, your best bet is probably with startups or mature OSS solutions that can be customized to the specific needs of your infrastructure.

# Enterprise MLOps Strategy

With a high-level overview of the critical issues involved in MLOPs, it is time to turn to strategy, as shown in [Figure 1-15](#ch01.xhtml#Figure-1-13-mlops-strategy). There are four key categories to consider when implementing an MLOPs strategy: cloud, training and talent, vendor, and executive focus on ROI.

![](assets/1-13-mlops-strategy.png)

Let’s discuss each of these four categories:

*   Cloud: there is no perfect answer for which cloud platform to use. Any central platform will offer the advantages of economies of scale. What is essential in an MLOps strategy is to be aware of how a cloud platform fits into the unique goals of each specific organization and how it aligns with other strategic components like hiring or 3rd party vendor integration.
    
*   Training & Talent: often, organizations only look at the power of new technology and don’t consider the training and talent component of using the technology. In almost all cases, an organization should use a less powerful technology if hiring and training are better with a less powerful solution. This fact means widespread technology is crucial when implementing new technology. Ultimately, the latest technology is dead on arrival if you cannot hire or train your staff.
    
*   Vendor: An often overlooked issue with using cloud computing is that it usually needs to be augmented by specialized vendors to help an organization reach its goals with the technology. These strategic choices can lead to better ROI for both the cloud and the business strategies. Examples include using vendor technology specializing in Hadoop, Kubernetes, or pre-trained models. The vendors will be unique to each organization and its business goals.
    

###### Note

In an interview for this book, the CEO of Outerbounds and the author of Metaflow, Ville Tuulos, mentions that while all companies use the base layer of the cloud, say storage and databases, they often may need to augment with vendors at higher layers: [Enterprise MLOps Interviews](https://learning.oreilly.com/videos/enterprise-mlops-interviews/08012022VIDEOPAIML/)

*   Executive Focus on ROI: Ultimately, the preceding three categories don’t mean anything if the executive focus isn’t on ROI. The purpose of technology is to drive long-term business value, meaning problems need accurate scoping.
    

# Conclusion

This chapter sets the stage for understanding the crisis in enterprises getting machine learning and AI in production. From a common sense approach, the idea of just “hiring more data scientists” to increase ROI is as sensible as “just hiring more software engineers” to make a traditional software project go faster. In the case of the conventional software company, if there is no product, no goal, and no oversight, then hiring more developers increases the capital expenditure of the organization without any added value.

Instead of this scenario, MLOps aims to add a methodology that builds off the successful lessons of DevOps while handling the unique characteristics of machine learning. Finally, at the enterprise level, ultimately, it comes down to ROI with data science. Technology is an accelerant of value for most organizations, not the value. Organizations that create a hunger for ROI can quickly adopt the MLOps mindset.

# Critical Thinking Discussion Questions

*   There are many methods to deploy machine learning models to production, including pre-trained models, APIs, AutoML, and bespoke training. What are the pros and cons of each of these approaches?
    
*   What strategies could an enterprise implement to attract new machine learning engineering talent and train and retrain current talent?
    
*   If your organization doesn’t currently do any DevOps, a foundational component necessary for MLOps, how could they start a first DevOps project to test concepts like CI/CD and IaC?
    
*   If your organization doesn’t have large quantities of proprietary data, how can it use machine learning to gain a competitive advantage anyway?
    
*   What is your organization’s cloud strategy: single cloud, multi-cloud, hybrid cloud, private cloud, or something else? How does this help your organization reach your MLOps goals?
    

# Exercises

*   Go to a popular model hosting site like [TensorFlow Hub](https://www.tensorflow.org/hub) or [Hugging Face](https://huggingface.co/) and deploy one of their models to your favorite cloud platform.
    
*   Pick a cloud-based development environment like [Github CodeSpaces](https://github.com/features/codespaces), [Amazon SageMaker Studio Lab](https://aws.amazon.com/sagemaker/studio-lab/), or [Google Colab](https://colab.research.google.com/) and explore the interface with an eye for building a machine learning engineering project.
    
*   Use a machine learning app framework like [Gradio](https://gradio.app/) or [Streamlit](https://streamlit.io/) to build a simple machine learning application.
    
*   Brainstorm several organizational problems that may benefit from using machine learning and build a simple prototype using an MLOps technology.
    
*   Convert a Kaggle project to an MLOps project by downloading the dataset and coding an MLOps technology to serve predictions.
    

^([1](#ch01.xhtml#idm46247718218256-marker)) Dr. Luks summarizes the systematic evidence-based strategy: “Create a caloric deficit, then stay lean. Get sleep. Eat real food. Move often, throughout the day. Push and pull heavy things. Socialize. Have a sense of purpose.”

^([2](#ch01.xhtml#idm46247715452016-marker)) In the book *Principles of Macroeconomics (McGraw Hill, 2009)*, the authors share the story of how a talented chef could extract all of the profit from restaurants in a scenario of perfect competition since they would continuously leave for a higher salary to a competing restaurant, thus ultimately removing all profit for the owner.

^([3](#ch01.xhtml#idm46247717831424-marker)) Sociologists Robert K. Merton and Harriet Zuckerman [first coined](http://garfield.library.upenn.edu/merton/matthewii.pdf) this term.

# Chapter 2. The Stages of MLOps

MLOps is not about tracking the local experiments and is not about placing an ML model behind an API endpoint. Instead, MLOps is about building an automated environment and processes for continuously delivering ML projects to production.

MLOps consists of four major components (and is not confined to model training):

*   Data collection and preparation
    
*   Model development and training
    
*   ML service deployment
    
*   Continuous feedback and monitoring
    

The chapter explores these components in detail.

# Getting Started

Begin with the end in mind - The first step in any ML project is to articulate:

1) The problem that needs to be solved using ML.

2) What you want to predict.

3) How to extract business value from the answer. Examples of business value we might require include decreasing fraud, increasing revenue by attracting new customers, cutting operational costs by automating various manual processes, and so on.

Once you define the goal, don’t rush straight into the implementation phase. First, consider the following:

*   Which historical and operational data can be gathered and used in both the training and [serving](https://www.iguazio.com/glossary/model-serving/) pipelines.
    
*   How to incorporate the ML model results in a new or existing application in a way that can make an impact.
    
*   How to verify and reliably measure that the ML model meets the target and generates valuable business outcomes.
    

[Figure 2-1](#ch02.xhtml#ml_project_life_cyle) illustrates the different stages in an ML project. Note the feedback loop where the observations are used to recalibrate the business goals, data collection and preparation logic.

![](assets/project-life-cycle.png)

If you only focus on the ML model, you may encounter pitfalls such as these:

*   Using the wrong datasets, which can easily lead to inaccurate or biased results.
    
*   Lacking enough labeled data to build a model.
    
*   Finding out historical features used to train the model are unavailable in the production or real-time environment.
    
*   Discovering there is no practical way to integrate the model predictions into the current application.
    
*   Realizing the ML project costs are higher than the generated value, or, in a worst case scenario, cause losses in revenue or customer satisfaction.
    

## Choose Your Algorithm

The next phase is to determine the type of ML problem and algorithm.

In supervised learning, labels are required and known:

**Classification**
: 

The algorithm will answer binary yes-or-no questions (fraud or not, is it an apple, will the customer churn) or make a multiclass classification (type of tree, and so on). You also need enough labeled data for the algorithm to learn from.

**Regression**
: 

The algorithm predicts continuous numeric values based on various independent variables. For example, regression algorithms can aid in estimating the right price for a stock, the expected lifetime of a component, temperature, and so on.

[Figure 2-2](#ch02.xhtml#regression-vs-classification) compares those two algorithms.

![](assets/regression-vs-classification-in-machine-learning.png)

In unsupervised learning, labels are not required and known:

**Clustering**
: 

The algorithm will look for meaningful groups or collections in the data (customers segmentation, medical imaging, music genre, anomaly detection, and so on) based on their similarity without the help of pre-labeled data.

**Dimensionality reduction**
: 

The algorithm will reduce the *dimensionality* (the number of input variables in a dataset) from a high-dimensional space into a low-dimensional space so that the low-dimensional representation retains some meaningful properties of the original data, ideally close to its intrinsic dimension. Dimensionality reduction allows you to avoid overfitting, reduce the model computation overhead, and handle fewer features than originally required.

**Recommendation and ranking**
: 

The algorithm recommends or ranks objects by considering their relevance, importance, and content score. Recommendation algorithms can be used to rank web pages, recommend movies or music in streaming services, or show the products that a customer might purchase with a high probability based on their previous search and purchase activities. Recommendation engines can be used either for supervised or unsupervised learning.

Note that ranking algorithms rely on search queries provided by users, who know what they are looking for. Recommender systems, on the other hand, operate without any explicit inputs from users and aim to discover things the users might not have found otherwise.

Some applications may incorporate multiple algorithms. For example, using an NLP algorithm to determine the sentiment in the text and using the sentiment as an input for making a purchase decision.

## Design Your Pipelines

ML models have a limited lifetime since data patterns change ([drift](https://www.iguazio.com/glossary/concept-drift/)) over time, and models may have limited scope. For example, creating specific models per each user or device (trained on the relevant subset of the data). In many cases, we would like to train multiple models using different parameters or algorithms and compare or combine them.

For those reasons, the goal is *not* to build a model but rather to create an automated [ML pipeline](https://www.iguazio.com/glossary/machine-learning-pipeline/) (factory) that can accept inputs (code, data, and parameters), produce high-quality model artifacts, and deploy them in the application pipeline.

The ML pipelines can be triggered every time the data, code, or parameters change or can be executed in a loop (each time with a different dataset or parameters) to produce multiple models. To understand, compare, or explain the model results, all the inputs (code, data, parameters), operational data (type of hardware, logs, and so on), and results must be recorded and versioned.

A model is usually deployed as part of a more extensive application pipeline, including API integration, real-time data enrichment and preparation, model serving, actions, and monitoring. The automated deployment cannot focus solely on the model but on deploying or updating the entire application pipeline.

The typical ML pipeline consists of data preparation, training, testing, registering, and deployment. In real life, the ML pipelines can incorporate additional steps for data validation, optimization, and so on. In addition, some ML pipelines build and use multiple models.

[Figure 2-3](#ch02.xhtml#product-recommendations) demonstrates a recommendation engine application that uses two models in cascade. The first model is used to identify similar products. The second model will use the output from the first model and other user data to determine the buying probability (and filter the results).

![](assets/product-recommendations.png)

# Data Collection and Preparation

There is no ML without data. Before everything else, ML teams need access to historical or online data from multiple sources. They must ingest, prepare and explore the data before building any model.

The first step is to define your goal, which problem or challenge you intend to solve, and which data sources or features can help you predict the outcome. Once you identify the target and raw datasets, you must gather enough data, prepare, label, and explore it for use in your model.

In most cases, the raw data cannot be used as-is for machine learning algorithms, for various reasons, including the following:

*   The data is low quality (missing fields, wrong spelling, null values, and so on) and requires cleaning and imputing.
    
*   The data needs to be grouped or aggregated to make it meaningful.
    
*   The data needs to be converted to numerical or categorical values, which algorithms can process.
    
*   Feature values should be normalized and scaled to guarantee they have equal importance.
    
*   The data is encoded or requires joins with reference information.
    

According to IDC, by 2025, 80% of data will be [unstructured](https://www.delltechnologies.com/asset/en-th/products/storage/industry-market/h18811-wp-idc-new-unstructured-storage-requirements-for-enterprises.pdf), so an essential part of building operational data pipelines is to convert unstructured textual, audio, and visual data into machine learning- or [deep learning](https://www.iguazio.com/glossary/deep-learning-pipelines/)\-friendly data organization or vector formats.

The ML process starts with manual exploratory data analysis and [feature engineering](https://www.iguazio.com/glossary/feature-engineering/) on small extractions from historical data. However, to bring accurate models into production, ML and data engineering teams must work on larger, more up-to-date datasets and automate the collection and preparation process. Furthermore, batch collection and preparation methodologies and batch analytics don’t work well for operational or [real-time](https://www.iguazio.com/glossary/real-time-ml/) pipelines. As a result, ML teams often build separate *real-time data pipelines* (pipelines that handle a very large number of events at scale in real-time) that use *stream processing* (the ingestion and processing of a continuous data stream). Some vendors provide data labeling as a service using a combination of automated tools and crowd-sourcing (for example [Amazon SageMaker Ground Truth](https://aws.amazon.com/sagemaker/data-labeling)). Many algorithms require labeled data for [training](https://www.iguazio.com/glossary/model-training/) the model. Therefore, you must design and implement labeling solutions for the historical data as part of the data preparation process.

In addition, many applications require constant re-training to maintain the model’s accuracy and relevancy. Therefore, you should design a pipeline for automatically generating data labels in such cases.

Models are as good as the data they are trained on. To compare or explain model behavior and to address regulatory compliance, you must have access to the data used in training. Therefore, you must save information about the data origin with the model or save a unique copy of the dataset used for every training run. Data lineage and versioning solutions are a must in every MLOps solution.

A key solution in any modern MLOps solution is a [feature store](https://www.iguazio.com/feature-store/), which automates the collection, transformation, cataloging, versioning, and serving of offline and online data.

## Data Storage and Ingestion

Data is the foundation for AI and ML. It can be persistent or in transit and can be broken into two main categories: structured and unstructured. *Unstructured* data is usually stored in file systems, object storage (data lakes), logging, or messaging systems (such as email). *Structured* data has some schema and is stored in tables, documents, or graphs.

Since it is scalable and cost-effective, we usually use object storage for deep learning workloads that process images, video, and text (NLP). In some cases, we will use local or distributed file storage.

When the data is structured, we can use files (CSV, Excel, and so on) to do simple exploration and model training, but this cannot scale for production. In production, we store data in one of those two categories:

*   Archival data systems: These are data warehouses or objects with structured file formats like CSV, Parquet, JSON, and so on. They record all the historical transactions and allow efficient analytics queries.
    
*   Operational or real-time databases: These are frequently updated and enable fast data retrieval by index.
    

Use archival storage (data warehouses or data lakes with structured objects) for the training process since a model is an equation that learns how to predict results based on historical data patterns. Suppose the data source is a real-time or operational data system. In that case, you first need to copy and transform the data to the archival system, which is better at analytical workloads, for example, using an ETL process (Extract, Transform, Load). Structured object formats are usually the cheapest storage option, especially when using efficient compression techniques (like Parquet files). But data warehouses (like [Google BigQuery](https://cloud.google.com/bigquery), [Snowflake](https://www.snowflake.com/), [Amazon Redshift](https://aws.amazon.com/redshift/), and so on) support faster and more flexible data queries and are easier to update.

# Warning Title

\=== When you collect data for training, it is essential to make sure there is no bias in the data since this can lead to poor model results and even a total failure of your project (see [Amazon scrapped *sexist AI* tool](https://www.bbc.com/news/technology-45809919)). ===

MLOps solutions and the training flow should incorporate data version control. Every training job should point to a unique version of the data, which allows for reconstructing the exact content of the data. While this may be simple for static historical content, it is harder for continuous and dynamic data like user information or transactions, which can change frequently.

The solution is to snapshot and store the dataset in archival storage and add the appropriate link (data lineage) to the job and model objects, allowing viewing the data associated with each run easily. Some MLOps frameworks and feature stores (like [MLRun](https://www.mlrun.org/)) provide this as a built-in feature.

In the serving process, a request arrives with partial data, for example, a user ID; you enrich the data with additional features for that user (such as age, gender, income, and so on) from an online database and pass it to the model. You cannot use archival storage for serving since it’s too slow and cannot support a high number of concurrent requests. Instead, indexed NoSQL or SQL databases (like [Redis](https://redis.io/), [DynamoDB](https://aws.amazon.com/dynamodb/), or [MySQL](https://www.mysql.com/)), also referred to as the *online feature store*, are better since they are faster and you have the index key (user ID).

To use the online features, you must first copy them to the online database; this can be simple with static features (like age or gender) but challenging with transactional features (like the total amount of purchases in the last hour) that are frequently updated. Stream processing is usually used to calculate and update real-time features efficiently. This means the real-time data pipeline uses a different implementation than the offline feature calculation (implemented for training).

[Figure 2-4](#ch02.xhtml#data-ingest-flow) demonstrates different components used in the data ingestion flow.

![](assets/data-ingest-flow.png)

Using different databases and data processing technologies in training and serving leads to higher complexity and data synchronization challenges. Feature Stores are used to abstract away much of that complexity and will be discussed in the following sections.

###### Note

Learn more about feature stores from [this online resource](https://www.iguazio.com/blog/what-are-feature-stores-and-why-are-they-critical-for-scaling-data-science/).

## Data Exploration and Preparation

In most cases, you cannot use data in its raw format, so the first step is applying cleansing, transformations, or calculations to the data. Once you have a clean set of meaningful features, you can start evaluating the data and selecting the best features for our model.

Here are some examples of required data conversions:

*   Data arrives in a JSON format, and you need to convert it to an array or vector.
    
*   Data contains a string (like a city name), and you need to convert it to a numeric value using some encoding strategy.
    
*   You have a transaction log, but you need the total value of transactions in the last month.
    
*   You have a person’s zip code, but you need to translate it to a numeric value representing a social-economical score.
    
*   Dataset has missing values or misspelled names.
    

It is easier to start data exploration with a subset of the data and use interactive visual tools or standard python packages like [Pandas](https://pandas.pydata.org/), [Matplotlib](https://matplotlib.org/), [Bokeh](https://docs.bokeh.org/) and [Plotly](https://plotly.com/).

First you should visually inspect the data’s nature and quality (inconsistencies, outliers, anomalies, missing data, and so on) and clean the data. Next, transform and add derived features, examine the correlation between the data or its derivatives and the target feature (goal), to support or disprove your theory, and generate a training set (feature vector). Creating new derived features to improve a model’s output, is the main craft of data scientists. Choosing relevant features to analyze and eliminate irrelevant or redundant ones is also essential.

Note that in the production implementation, there is a need to process more significant amounts of data in an automated way. Therefore, you must re-implement the data cleansing and transformations steps as part of a scalable and automated data processing pipeline and may need to use scalable or real-time data processing engines (like [Spark](https://spark.apache.org/), [Flink](https://flink.apache.org/), [Nuclio](https://nuclio.io/), and so on) instead of interactive tools.

[Figure 2-5](#ch02.xhtml#feature-engineering-flow) illustrates the data preparation and feature engineering flow.

![](assets/feature-engineering.png)

These are some of the most common data transformations operations:

*   Drop rows/columns with too much missing data.
    
*   Imputing: Replace missing values with a constant or a statistical value (for example, median of the column)
    
*   Outlier detection: Drop rows where the values don’t fall under the expected range (for example, compare the row value with mean +/- N \* stddev).
    
*   Binning: Group multiple values into a single category (for example, Chile and Brazil map to South America).
    
*   Log transform: Convert a linear scale to a log scale.
    
*   One-hot encoding: Map different categorical values to a binary (yes/no) feature.
    
*   Grouping and aggregations: Aggregate column values by time (hour, day, month, and so on) or by category (for example, number of units sold by product type).
    
*   Scaling: Re-scale column values (normalization, standardization).
    
*   Date extractions: Convert a date time to the hour, day of the week, month, season, is it a holiday, and so on.
    
*   Time recency: The time distance between two events (for example, time from the last login).
    

For unstructured data, there can be many more types of transformations (extract text elements, resize or rotate an image, and so on).

In training and during serving, you must use the same features; this requires you to implement two data pipelines: a batch pipeline for training and a real-time (streaming) pipeline for serving.

Some feature stores provide simple ways to define the data transformation logic and will automatically deploy and manage both offline and online data pipelines for you.

## Data Labeling

*Data labeling*, or *data annotation*, is part of the preprocessing stage required for supervised learning. You add tags to raw data (numeric, text, images, and so on) to show a machine learning model the expected target attribute (prediction). Some prominent examples include [Amazon Sagemaker Ground Truth](https://aws.amazon.com/sagemaker/data-labeling/), [Label Studio](https://labelstud.io/), [DataTurks](https://github.com/DataTurks), and [CVAT](https://github.com/openvinotoolkit/cvat).

For numeric values, labeling can be deducted from the raw data. So, for example, in a churn model that tries to predict which customers are about to churn, you can examine historical records and mark the customers who churned by looking to see whether they remained a customer in the consecutive month. A simple analytics query will do the trick and shift the results back by one month.

Labeling is harder for unstructured data (text, images, video, audio, and so on) and usually involves a manual labeling process (by a human). However, many solutions in the market can simplify and automate parts of the process. Nevertheless, some challenges remain, like the need for domain expertise, the risk of inconsistency, and the error proneness of the process.

When the historical datasets are static, the labeling is done once. So, for example, the problem of classifying images as cats or dogs probably won’t change anytime soon. But when the data is dynamic, for instance, in a face or finger recognition application, new people can be added any day. In such cases, the labeling solution must be part of the application. For example, new users can take their pictures and attach their ID (for the application to verify their identity). If an image is not classified, it should alert or fall into a manual identification flow. When new pictures are added, the model training process needs to be triggered, and the online models must be refreshed to take the new images into account.

Data can be associated with labels and tags during ingestion time. For example, images arrive from a car along with metadata (car ID, model, driver, .) and telemetry (geolocation, timestamp, speed, weather, sensor metrics, and so on). This information should be stored and linked to the image and can be used to generate labels.

When considering MLOps with automated (re)training flow, you should consider a mechanism for automated labeling. In some applications, the labels arrive in a delay (for example, if the user churned, if the stock price went up, or if the customer purchased the product). Therefore, the training dataset should be shifted to accommodate the delay (if you re-train the churn model based on the last three months, the data range should be between 4 and 1 month ago).

## Feature Stores

As we’ve established, most of the complexities in any ML project arise from the data:

*   Work is typically done in silos (data scientists and engineers).
    
*   Labor-intensive data engineering to produce high-quality features.
    
*   Duplicate efforts and resources in generating offline and online features which also lead to inaccurate results.
    
*   Hard to incorporate data versioning and governance.
    
*   Feature development work is duplicated for every new project.
    
*   Lack of simple access to production-ready features at scale.
    
*   Disjointed or nonexistent model and feature monitoring.
    

ML teams need a way to continuously deploy AI applications in a way that creates real, ongoing business value for the organization. Features are the fuel driving AI for the organization, and feature stores are the architectural answer that can simplify processes, increase [model accuracy](https://www.iguazio.com/glossary/model-accuracy-in-ml/) and accelerate the path to production.

A feature store provides a single pane of glass for sharing all available features across the organization along with their metadata. When data scientists start a new project, they can access this catalog and easily find features. But a feature store is not just a data layer; it is also a data transformation service enabling users to manipulate raw data and store it as features ready to be used for offline (training) and online (serving), without duplicating the work. In addition, some feature stores support strong security, versioning, and data snapshots, enabling better data lineage, compliance, and manageability.

Some of the largest tech companies that deal extensively with AI have built their own feature stores (Uber, Twitter, Google, Netflix, Facebook, Airbnb, and so on). The open-source and commercial landscape for feature stores has exploded in the last few years. This is a good indication to the rest of the industry of how important it is to use a feature store as a part of an efficient ML pipeline.

Most feature stores are limited to structured data handling (ML), but some can support both structured data and unstructured (text, documents, images, audio, and so on).

Feature stores are described in detail later in chapter four book. As illustrated in [Figure 2-6](#ch02.xhtml#feature-store-arch), they provide a mechanism to read data from various online or offline sources, conduct a set of data transformations, and persist the data in online and offline storage. Features are stored and cataloged along with all their metadata (schema, labels, statistics, and so on), allowing users to compose *feature Vectors* (joint multiple features from different feature sets) and use them for training or serving. The feature vectors are generated when needed, taking into account data versioning and time correctness (time traveling). Different engines are used for feature retrieval, real-time engine for serving, and batch one for training.

![](assets/feature-store-arch.png)

Here are some major benefits of a feature store:

*   Faster development with far fewer engineering resources.
    
*   Smooth migration from development to production.
    
*   Increased model accuracy (same pipeline for online and offline).
    
*   Better collaboration and security across teams.
    
*   The ability to track lineage and address regulatory compliance.
    

###### Note

Not all feature stores are born equal. Some are focused on cataloging and don’t automate the process of ingestion and online or offline transformation, which are the most labor-intensive tasks. Therefore, make sure you conduct a proper evaluation before selecting a solution.

# Model Development and Training

Data scientists generally go through the following process when developing models:

1.  Extracting data manually from external sources.
    
2.  Data labeling, exploration, and enrichment to identify potential patterns and features.
    
3.  Model training and validation.
    
4.  Model evaluation and testing.
    
5.  Going back to step one and repeating until the desired outcomes (accuracy, loss, and so on.) have been achieved.
    

The traditional way is to use notebooks, small-scale data, and manual processes, but this does not scale and is not reproducible. Furthermore, in order to achieve maximum accuracy, experiments often need to be run with different parameters or algorithms (AutoML).

With MLOps, ML teams build machine learning pipelines that automatically collect and prepare data, select optimal features, run training using different parameter sets or algorithms, evaluate models, and run various model and system tests. All the executions, along with their data, metadata, code, and results, must be versioned and logged, providing quick results visualization, comparing them with past results, and understanding which data was used to produce each model.

Pipelines can be more complex—for example, when ML teams need to develop a combination of models or use deep learning or NLP. You can see a basic model development flow example in [Figure 2-7](#ch02.xhtml#model-development).

![](assets/model-dev.png)

ML pipelines can be started manually or preferably triggered automatically when:

1.  The code, packages, or parameters change.
    
2.  The input data or feature engineering logic change.
    
3.  Concept drift is detected, and the model needs to be retrained with fresh data.
    

ML pipelines have the following features:

*   Are built using microservices (containers or serverless functions), usually over Kubernetes.
    
*   Have all their inputs (code, package dependencies, data, parameters) and the outputs (logs, metrics, data/features, artifacts, models) tracked for every step in the pipeline in order to reproduce or explain our experiment results.
    
*   Use versioning for all the data and artifacts used throughout the pipeline.
    
*   Store code and configuration in versioned Git repositories.
    
*   Use continuous integration (CI) techniques to automate the pipeline initiation, test automation, review, and approval process.
    

Pipelines should be executed over scalable services or functions, which can span elastically over multiple servers or containers. This way, jobs complete faster, and computation resources are freed up once they are complete, saving high costs.

The resulting models are stored in a versioned model repository along with metadata, performance metrics, required parameters, statistical information, and so on. Models can be loaded later into batch or real-time serving microservices or functions.

## Writing and Maintaining Production ML Code

Many data scientists like the usability and interactivity of Jupyter Notebooks when they develop and evaluate models. It is convenient indeed to manipulate some code and immediately see a visual table or a chart, and most ML tutorials, examples, and Kagle projects are consumed as Notebooks.

You can find projects where the data preparation, training, evaluation, and even prediction are all made in one huge Notebook, but this approach can lead to challenges when moving to production, for example:

*   Very hard to track the code changes across versions (in Git).
    
*   Almost impossible to implement test harnesses and unit testing.
    
*   Functions cannot be reused in various projects.
    
*   Moving to production requires code refactoring and removal of visualization or scratch code.
    
*   Lack of proper documentation.
    

The best approach is to use functional programming for code segments and Notebooks for interactive and visualization parts. [Example 2-1](#ch02.xhtml#data-prep-func) implements a data preparation function that accepts a dataset (dataframe) and some properties as inputs and returns the manipulated dataset. The function is documented and allows users to understand the purpose and usage.

##### Example 2-1. Data prep function (data\_prep.py)

```
import pandas as pd

def add_date_features(
    data, time_column: str = "timestamp", drop_timestamp: bool = False
):
    """Add numeric date features (day of week, hour, month) to a dataframe

    :param time_column:    The name of the timestamps column in the data
    :param drop_timestamp: set to True to drop the timestamp column from
                           the original dataframe
    :return datafarame
    """
    timestamp = pd.to_datetime(data[time_column])
    data["day_of_week"] = timestamp.dt.day_of_week
    data["hour"] = timestamp.dt.hour
    data["month"] = timestamp.dt.month
    if drop_timestamp:
        data.drop([time_column], axis=1, inplace=True)
    return data
```

Place the function in a separate python file *data\_prep.py*, and now you can call it from the Notebook, inject data and examine or visualize its output using the following code cell:

```
import pandas as pd
from data_prep import add_date_features

df = pd.read_csv("data.csv")
df = add_date_features(df, "timestamp", drop_timestamp=True)
df.head()
```

Once the code is well defined, use the python test framework (pytest) and implement unit testing for each of the functions as show in [Example 2-2](#ch02.xhtml#data-prep-test):

##### Example 2-2. Data prep test function (test\_data\_prep.py)

```
import pytest
import data_prep
import pandas as pd

# tell pytest to test both drop values (True/False)
@pytest.mark.parametrize("drop_timestamp", [True, False])
def test_add_date_features(drop_timestamp):
    df = pd.DataFrame({'times':['2022-01-01 08:00',
                                '2022-02-02 09:00',
                                '2022-03-03 10:00'],
                       'vals':[1,2,3]})
    new_df = data_prep.add_date_features(df, "times", drop_timestamp=drop_timestamp)

    # verify the results are as expected
    assert new_df["day_of_week"].to_list() == [5, 2, 3]
    assert new_df["month"].to_list() == [1, 2, 3]
    assert new_df["hour"].to_list() == [8, 9, 10]
    assert ("times" in new_df.columns.values) != drop_timestamp
```

The code in [Example 2-2](#ch02.xhtml#data-prep-test) will execute the `add_date_features()` function with different input options and verify that the outputs are correct.

Using this approach, you gain some immediate benefits:

*   Easily see changes to your data prep code in the version control.
    
*   The same code can be tested later with a test harness (for example, using pytest).
    
*   The function can be moved to production without the need to refactor the notebook.
    
*   The function is documented, and you can easily understand how to use it and what to expect.
    
*   The function can later be saved to a shared library and used across different projects.
    
*   The code becomes more readable.
    

Another benefit of the functional approach is demonstrated in the upcoming chapters: an automated way to convert development code into production services and pipelines using tools such as [MLRun](https://www.mlrun.org/) (MLOps orchestration framework).

## Tracking and Comparing Experiment Results

When running ML experiments, it is essential to track every run so that you can reproduce experiment results (for example, which parameters and inputs yield the best results), visualize the various metrics, and compare the results of different algorithms or parameters sets.

Each execution involves input and output datasets. It is crucial to track and version the datasets and not only the parameters. Any MLOps solution should provide a mechanism to version data and track the data propagation (lineage) together with the rest of the execution parameters, outputs, and metadata.

Today various open-source and commercial frameworks track the results of every experiment run, store it in a database, and visualize it. Some examples shown in [Figure 2-8](#ch02.xhtml#experiment-tracking-tools) include [MLFlow](https://mlflow.org/), [Weights & Biases](https://wandb.ai/), MLRun and [ClearML](https://clear.ml/).

![](assets/experiment-tracking-tools.png)

In the real world, experiments can run in an automated ML pipeline (see [Figure 2-9](#ch02.xhtml#pipeline-tracking)), which comprises different steps (data prep, train, test, and so on). Each stage of the pipeline accepts parameters and inputs data and generates results such as output values, metrics, and data, to be used in subsequent pipeline steps. In addition, the tracking should be extended to operational data (which code was used, packages, allocated and used resources, systems, and so on).

![](assets/pipeline-tracking.png)

[Figure 2-10](#ch02.xhtml#experiment-tracking-arch) shows the general architecture of an execution tracking system. Inputs may include parameters, the user or system-defined tags (to allow filtering and comparisons), secrets (hidden credentials used by the execution), and data objects (files, tables, and so on). Outputs include the result metrics, logs, usage data, output data objects, and artifacts. A good tracking system also records the code version, used packages, runtime environment and parameters, resources, code profiling, and so on

![](assets/experiment-tracking-arch.png)

The downside of execution tracking is that it requires *code instrumentation* (adding code to explicitly log parameters, tags, results, and data). Some MLOps frameworks provide auto-logging for ML/DL workloads where you can import a library that automatically records all the ML framework-specific metrics.

A new technology, AutoMLOps, is pioneered in the MLRun framework, which not only records metrics along with the parameters, data lineage, code versioning, and operational data, but also automatically adds production features for auto-scaling, resource management, auto-documentation, parameter detection, code profiling, security, model registry, and so on. eliminating significant engineering efforts.

## Distributed Training and Hyper-Parameter Optimization

To get to the best model results, try out various algorithms or parameter combinations and choose the best one based on a target metric like best accuracy. This work can be automated using multiple hyper-parameter optimization and AutoML frameworks, which try out the different combinations, record all the metrics for each run and mark the best. To shorten training time, some frameworks support executing each individual run on a different compute resource. [Figure 2-11](#ch02.xhtml#hyper-param-run) shows the tracking of multiple children runs in a hyper-parameter job and the best-selected result.

![](assets/hyper-param-run.png)

Parallel Hyper-parameter jobs are not limited to model training. They can be used for parallel loading and preparation of many data objects, parallel testing of different test cases, and so on

There are several hyper-parameter execution strategies:

*   Grid search: Running all the parameter combinations.
    
*   Random: Running a sampled set from all the parameter combinations.
    
*   Bayesian optimization: Buiding a probability model of the objective function and use it to select the most promising hyper parameters to evaluate in the true objective function.
    
*   List: Running the first parameter from each list followed by the second from each list and so on.
    

You can specify selection criteria to select the best run among the different child runs (for example, the model’s accuracy) and the stop condition to stop the execution of child runs when some criteria, based on the returned results, are met (for example: `stop condition="accuracy>=0.9"`).

Some data engineering, ML, or DL jobs cannot fit into a single container or virtual machine and must be distributed across multiple containers. A few open-source frameworks, including Spark, [Dask](https://www.dask.org/), [Horovod](https://horovod.ai/), and Nuclio, support workload distribution. When distributing the workload in combination with the parallel run of child (hyper-parameter) tasks, you need to control and limit the total amount of resources used.

Tracking a distributed workload may be more challenging. Make sure the MLOps framework you use supports that.

## Building and Testing Models For Production

When models are used in real-world applications, it is critical to ensure they are robust and well-tested. Therefore, in addition to traditional software testing (unit tests, static tests, and so on), tests should cover the following categories:

*   Data quality tests: The dataset used for training is of high quality and does not carry bias.
    
*   Model performance tests: The model produces accurate results.
    
*   Serving application tests: The deployed model along with the data pre/post-processing steps are robust and provide adequate performance.
    
*   Pipeline tests: Ensuring the automated development pipeline handles various exceptions and the desired scale.
    

When the training dataset is of low quality, you may presume that the model is accurate, but it can make harmful predictions. Therefore, it is essential to validate that the data is high quality. Here are some examples of data quality tests:

*   There are no missing values.
    
*   Values are of the correct type and fall under an expected range (for example, user age is between 0-120, with anticipated average and standard deviation).
    
*   Category values fall within the possible options (for example, city names match the options in a city name list).
    
*   There is no bias in the data (for example, the gender feature has the anticipated percentage of men and women).
    

The data quality tests can be implemented in the data pipeline (and feature store) or in the ML pipeline before the training. Note that some feature stores automate the data quality validation using built-in functions.

Once you train the model, the next step is to make sure it is accurate and resilient. Beyond the common practice of setting aside a test dataset and measuring the model accuracy using that dataset, here are several additional tests which can improve the model quality:

*   Verify the performance is maintained across essential slices of the data (for example, devices by model, users by country or other categories, movies by genre) and that it does not drop significantly for a specific group.
    
*   Compare the model results with previous versions or a baseline version and verify the performance does not degrade.
    
*   Test different parameter combinations (hyper-parameter search) to verify we choose the best parameter combination.
    
*   Test for bias and fairness - verify that the performance is maintained per gender and specific populations.
    
*   Check feature importances and if there are features with a marginal contribution that can be removed from the model.
    
*   Test for immunity to fake, random, or malicious input vectors to increase robustness and defend against adversarial attacks.
    

Particular attention should be given to how you generate the test set independently that considers fairness and unbias and minimizes the dependencies with the training set.

When the models are deployed into production serving applications, they contain additional data pre or post-processing logic (extraction, formatting, validation, transformations, API integration, and so on). In addition, the model code may depend on various software packages or infrastructure (memory, CPUs, GPUs, and so on). Therefore, models must be thoroughly tested in their target serving application environment and through the API before they are deployed into the production environment.

Here are some examples for serving application tests:

*   API coverage: All serving APIs behave as expected.
    
*   Performance tests: Verify the serving application can sustain the target number of requests per second and respond within the required latency.
    
*   Package consistency: Verify that the model training and serving are using the same framework version (for example, Sklearn).
    
*   Test data validation logic: Verify the model endpoint fails or logs the request if improper data is sent to the model.
    
*   Test resiliency: Test that the serving application can sustain malicious attacks and impersonation.
    
*   Test correctness: Verify that the model prediction results via the serving API are the same as those in the model validation step.
    
*   Test the outcome: Verify that prediction results translate to the proper action (writing to a database, generating an alert, updating the user interface, and so on).
    

The different tests should all be part of an automated CI/CD pipeline. Every time the dataset or code changes, the pipeline is executed and produces a new set of deployable objects (models, applications, features, and so on) and logs all the results to enable reproducibility and explainability.

Some attention should be given to testing the ML pipeline, ensuring that it will run correctly every time it’s triggered, will not fail due to missing parameters or inadequate resources, and can handle data at scale.

Once the model and other production artifacts are ready, they must be stored in a versioned artifacts repository along with all their metadata and the parameters required to generate the production deployments.

In many cases, the trained model can be further optimized for production and higher performance, for example, by performing feature selection and removing redundant features or by compressing the models and storing them in more machine-efficient formants like [ONNX](https://onnx.ai/). Therefore, ML pipelines may incorporate model optimization steps.

[Figure 2-12](#ch02.xhtml#model-test-pipe) illustrates how different test and optimization steps can be used as part of an ML pipeline.

![](assets/model-test-pipe.png)

# Deployment (and Online ML Services)

Once an ML model has been built, it needs to be integrated with real-world data and the business application or front-end services. The whole application or parts thereof need to be deployed without disrupting the service. Deployment can be extremely challenging if the ML components aren’t treated as an integral part of the application or production pipeline.

ML Application pipelines usually consist of the following:

*   API services or application integration logic.
    
*   Real-time data collection, enrichment, validation, and feature engineering logic.
    
*   One or more model serving endpoints.
    
*   Data and model monitoring services.
    
*   Resource monitoring and alerting services.
    
*   Event, telemetry, and data/features logging services.
    
*   A set of actions following the prediction results.
    

You can see a real-time pipeline example in [Figure 2-13](#ch02.xhtml#build-online-ml-services).

The different services are interdependent. For example, if the inputs to a model change, the feature engineering logic must be upgraded along with the model serving and model monitoring services. These dependencies require online production pipelines (graphs) to reflect these changes.

![](assets/build-online-ml-services.png)

Application pipelines can be more complex when using unstructured data, deep learning, NLP, or model ensembles, so having flexible mechanisms to build and wire up our pipeline graphs is critical.

Application pipelines are usually interconnected with fast streaming or messaging protocols, so they should be elastic to address traffic and demand fluctuations, and they should allow non-disruptive upgrades to one or more elements of the pipeline. These requirements are best addressed with fast serverless technologies.

Application pipeline development and deployment flow do the following:

1.  Develop production components:
    
    *   API services and application integration logic.
        
    *   Feature collection, validation, and transformation.
        
    *   Model serving graphs.
        
2.  Test online pipelines with simulated data.
    
3.  Deploy online pipelines to production.
    
4.  Monitor models and data and detect drift.
    
5.  Retrain models and re-engineer data when needed.
    
6.  Upgrade pipeline components (non-disruptively) when needed.
    

## From Model Endpoints to Application Pipelines

Today’s common practice is to build model serving endpoints that merely accept the numeric feature vector and respond with a prediction. The pre or post-processing logic, which is usually tightly coupled with the model, is done in separate microservices. This complicates the delivery, scaling, and maintenance of the ML application.

In some cases, the prediction is made using a combination of models, for example, by implementing an ensemble of models which cover different time scopes (recent time and seasonal models) or other algorithms. Another example is cascading two models. The first extracts sentiments from text, and the second makes a prediction based on the sentiments and other features.

A preferred approach is to design online (or real-time) application pipelines where the model serving is just one step in it and be able to deploy, upgrade or roll back that pipeline as a whole. Unlike the data and model training pipelines which run slow batch tasks, the application pipeline should process thousands of requests per second and use streaming or serverless processing engines.

[Figure 2-14](#ch02.xhtml#online-pipe-ex) Demonstrates a simple application pipeline that accepts a user request (via HTTP or a stream message), processes it, predicts a result using a three-model ensemble, and does post-processing (response to the user, updated the result in a database, generates an alert, and so on).

![](assets/online-pipe-ex.png)

Machine learning (ML) or deep learning (DL) applications may work with unstructured data and complex processing stages such as image manipulations (detect objects, resize, sample, re-color, crop, and so on) or text manipulations (parse, format, tokenize, and so on). Application pipelines are not limited to structured data. As illustrated in [Figure 2-15](#ch02.xhtml#online-pipe-nlp) a pipeline can branch and process different parts of the data using various technologies and models. In the example, a document URL is sent to the pipeline (via a Kafka stream), the first step fetches the document from an object storage repository, following text and image processing steps, and finally combines the results and updates a search database which hosts the document information.

![](assets/online-pipe-nlp.png)

Some examples of open source and commercial frameworks for building multi-stage online pipelines:

*   [AWS Step Functions](https://aws.amazon.com/step-functions): AWS cloud service composing online pipelines from AWS Lambda serverless functions and other AWS cloud services.
    
*   [MLRun Serving Graphs](https://www.mlrun.org/): Open source and commercial MLOps framework, its serving layer enables the composition of online data and ML/DL pipelines (graphs), provisioned automatically into auto-scaling real-time serverless functions.
    
*   [Apache Beam](https://beam.apache.org/): Open source stream processing framework, focused on online structured data processing. ([Google Dataflow](https://cloud.google.com/dataflow) is a managed version of Apache Beam).
    
*   [Seldon](https://www.seldon.io/): Open source and commercial model serving framework with basic online pipeline capabilities.
    

## Online Data Preparation

A dominant part of the online application pipeline is data processing, with tasks such as data parsing, formatting, validations, transformations, aggregations, logging, persisting, joining, and so on

Processing data in a batch is quite a common practice. For example, you can use data warehouse queries, ETL processes, Spark, and so on But the same technologies don’t work for online pipelines where thousands of events or user requests arrive every second and may need to be answered within milliseconds.

In online data pipelines, the features are accumulated in memory or a fast SQL/NoSQL database, fetched per event to enrich the user request, and passed into the model for prediction. When the features are based on historical or static data (gender, age, annual income, and so on), you can use a periodic batch process to copy such features to the online database. However, this won’t work when the features are frequently updated (current geo-location, last transaction value, money spent last hour, time from the previous login, and so on).

Online data pipelines are implemented using stream processing (Spark Streaming, Flink, [Amazon Kinesis Data Analytics](https://aws.amazon.com/kinesis/data-analytics/), Nuclio, and so on), where events are ingested, transformed, or aggregated on the fly to form the real-time feature vectors. In addition, a fast key/value database is used to persist and share the distributed state. [Figure 2-16](#ch02.xhtml#stream-processing) illustrates how stateful stream processing work. Events arrive and are distributed to stream workers (partitioned by the user key). Each worker processes the data and merges or aggregates it with the accumulated state.

![](assets/stream-processing.png)

The major challenge is that stream processing code and methodologies differ quite a bit from batch data analytics approaches and require re-implementing the batch pipeline used for the training into a real-time streaming pipeline. However, some feature stores allow you to define the data pipeline using high-level primitives and automatically generate the batch or streaming pipelines, ensuring the same logic is preserved and saving you a significant engineering effort.

Online data pipelines are not limited to structured data. Modern applications need to process unstructured visual and textual data with operations such as resizing or rotating images, parsing text, tokenizing statements, and so on Therefore, the technology and framework you select need to support such applications.

###### Note

The line between data processing and ML or DL can be blurred. For example, text can be converted to sentiment or a category feature using an NLP model, so is it model serving or a data transformation?

# Continuous Model and Data Monitoring

AI services and applications are becoming an essential part of any business. Poor model performance can lead to liabilities, revenue loss, damage to the brand, and unsatisfied customers. Therefore, it is critical to monitor the data, the models, and the entire online applications pipeline and guarantee that models continue to perform and that business KPIs are met. Thanks to well-implemented monitoring solutions, you can quickly react to the problems by notifying users, retraining models, or adjusting the application pipeline.

Monitoring systems track various infrastructure, data, model, and application metrics and can report or alert on different situations, including the following:

*   Data or concept drift: The statistical attributes of the model inputs or outputs change (an indication that the model will underperform).
    
*   Model performance problems: The results of the model are inaccurate.
    
*   Data quality problems: The data provided to the model is of low quality (missing values, NaNs, values are out of the expected range, anomalies, and so on).
    
*   Model bias: Detect changes between the overall scoring and scoring for specific populations (like male and female, minorities, and so on).
    
*   Adversarial attacks: Malicious attempts have been made to deceive the model.
    
*   Business KPIs: Verify that the model meets the target business goals (revenue increase, customer retention, and so on).
    
*   Application performance: The application manages to properly serve requests and without delays.
    
*   Infrastructure usage: Track the usage of computing resources.
    
*   Model staleness: Alert if it is too long since the last time a model version was deployed.
    
*   Anomaly detection: Model data or results don’t fall under the expected norm or classes (for example, using an encoder-decoder neural network model).
    

[Figure 2-17](#ch02.xhtml#model-monitoring-arch) shows a typical model monitoring architecture. The data inputs, outputs, and application metrics are sent to a stream. A real-time stream processing application reads the data. It can detect or alert on immediate problems, aggregate the information, and write to various data targets (key/value, time-series, and files or data warehouse).

![](assets/model-monitoring-arch.png)

Alerts generated by the monitoring system can notify users (via emails, Slack, and so on) or trigger a corrective action such as retraining a model with newer data, changing model weights, and so on

Feature stores can play a significant part in monitoring data and models. They store the schema and statistics per feature, which can be used in the different validation and analysis tasks. If the production data is returned to the feature store, it’s easier to analyze, join, and compare production datasets with other historical or offline datasets.

## Monitoring Data and Concept Drift

Concept drift is a phenomenon where the statistical properties of the target variable (y - which the model is trying to predict) change over time. Data drift (virtual drift) happens when the statistical properties of the inputs changes. In drift, the model built on past data does not apply anymore, and assumptions made by the model on past data need to be revised based on current data.

[Figure 2-18](#ch02.xhtml#concept-vs-virtual-drift) illustrates the differences between concept drift and virtual (data) drift.

![](assets/concept-vs-virtual-drift.png)

Going back to the business level, you can see examples of drift in the following use cases:

*   *Wind power prediction*: When predicting the electric power generated from wind from an off-line dataset based model, we have concept drift vs. on-line training models due to the non-stationary properties of winds and weather.
    
*   *Spam detection*: Email content and presentation change constantly (data drift). Since the collection of documents used for training changes frequently, the feature space representing the collection changes. Also, users themselves change their interests, causing them to start or stop considering some emails as spam (concept drift).
    

Concept drift changes can be:

*   Sudden: The move between an old concept and a new one happens simultaneously. The behavioral patterns associated with the COVID-19 pandemic have provided us with striking examples, like the lockdowns that abruptly changed population behaviors worldwide.
    
*   Incremental / Gradual: The change between concepts happens over time as the new concept emerges and starts to adapt. The move from summer to winter could be an example of gradual drift.
    
*   Recurring / Seasonal: The changes re-occur after the first observed occurrence. An example is a seasonal shift in weather, which dictates that consumers buy coats in colder months, cease these purchases when spring arrives, and then begin again in the fall.
    

In [Figure 2-19](#ch02.xhtml#drift-detection-logic) you can see how model drift detection works. First, the model inputs and outputs are collected, and the system calculates the statistics over a time window and compares them with the sample set statistics (saved at training time) or with the data from an older time window.

![](assets/drift-detection-logic.png)

The monitoring system saves the various feature statistics (min, max, average, stddev, histogram, and so on), and the drift level is calculated using one or more of the following metrics:

*   Kolmogorov–Smirnov test
    
*   Kullback–Leibler divergence
    
*   Jensen–Shannon divergence
    
*   Hellinger distance
    
*   Standard score (Z-score)
    
*   Chi-squared test
    
*   Total Variance Distance
    

[Figure 2-20](#ch02.xhtml#drift-detection-types) demonstrates how drift can be detected.

![](assets/drift-detection-types.png)

Drift is easily detected using those methods when the data consists of simple numeric metrics, but how can it be detected when the data is unstructured, an image, or a piece of text?

One trick is to convert the input data to flat metrics that represent the data and monitor the drift on those metrics. For example, let’s say you classify images of fruits. Then, you can convert the images to their RGB color metrics and check that the color distribution in production is the same as in training.

## Monitoring Model Performance and Accuracy

An important metric is to measure model accuracy in production. For that, you must have the *ground truth* (the actual result which matches the prediction). In some models obtaining the ground truth is relatively simple. For example, if we predict that a stock price will go up today, we can wait a few hours and know if the prediction was accurate. This is the same with other prediction applications like predicting customer churn or machine failure where the actual result arrives in some delay.

In some applications, a prediction is made for a specific transaction (for example, exposure or click on an advertisement). The transaction or prediction can be tagged with a UID (unique identifier) in such a case. Once the actual result is known (the customer bought the product), you can update the transaction (identified by the UID) with the ground truth value. This requires that the model serving and monitoring frameworks will have the ability to store or generate a UID per prediction and add the ground truth values to specific transactions/predictions.

The accuracy monitoring is done periodically (for example, every hour or day). First, a dataset is generated with the predicted Y values (calculated by the model) and the ground truth values (the actual result with the proper time shifting or obtained using the UID). Then, it is used to calculate the accuracy metrics and compare them with the accuracy during training.

This is illustrated in [Figure 2-21](#ch02.xhtml#model-accuracy-monitor).

![](assets/model-accuracy-monitor.png)

###### Note

The ground truth values calculated for the accuracy monitoring are the same Y labels required for retraining the model. Therefore, the best approach is to generate them once, store them in the feature store, and use them for both retraining and accuracy monitoring.

Just like in training, it is recommended to use several metrics to determine the prediction accuracy, especially in the case when classes are not balanced:

*   Accuracy: General overall accuracy.
    
*   Recall: What fraction of overall positives were correct.
    
*   Precision: Determine when the costs of False Positive are high.
    
*   F1 Score: Analysis of the trade-off between Recall and Precision.
    

###### Note

Be aware that the ground truth may contain bias. For example, in an application that predicts fraud to approve or reject transactions, the ground truth only includes information on the approved transactions. There is no data about declined transactions that may not have been fraudulent, which can lead to bias.

# The Strategy of Pre-Trained Models

One of the most prolific authors on business strategy is the [Harvard Business School professor Michael Porter](https://www.hbs.edu/faculty/Pages/profile.aspx?facId=6532), who has often said, “The essence of strategy is choosing what not to do.” With most organizations struggling to implement machine learning projects that provide ROI, there is a need for a better strategy. In particular, organizations should ask what they should not be doing while doing machine learning projects. In many cases, they shouldn’t be building a specific type of model and should instead use pre-trained models.

In the book *Understanding Michael Porter* (Harvard Business Review Press, 2011), the author Joan Magretta summarizes the essence of competitive advantage as outlined by Michael Porter in [Figure 2-22](#ch02.xhtml#Figure-2-0-1-competitive-advantage). Companies that compete on execution become part of a prisoner’s dilemma game theory problem, where both they are their competitors increasingly lower prices and costs while lowering the company’s profit. And this is the best-case scenario; in many cases, it is impossible for a company to out-execute a bigger rival, say training a better NLP or computer vision model.

![](assets/2-0-1-competitive-advantage.png)

This conceptual understanding of strategy is the essence of why pre-trained models are an essential component of a holistic strategy to create unique competitive advantages while implementing machine learning projects.

There are several vendors of pre-trained models. The most popular platform is [Hugging Face](https://huggingface.co/models), which has over 60 thousand models. Google’s [TensorFlow Hub](https://www.tensorflow.org/hub) has a unique collection of pre-trained models in various formats, including formats targeting runtimes like [Javascript](https://www.tensorflow.org/js) or [embedded hardware or mobile](https://www.tensorflow.org/lite/examples). One more format and repository is [ONNX](https://github.com/onnx/models#vision) which contains many examples of pre-trained computer vision and language models.

## Building an End-to-End Hugging Face Application

The best way to understand pre-trained models is to build an end-to-end solution with one. Fortunately, Hugging Face makes it simple to do this. First, you need to [sign up for a free account](https://huggingface.co/join).

###### Note

You can view a walkthrough of this Hugging Face application on [YouTube](https://youtu.be/EmsJG8RVhWY) or the [O’Reilly platform](https://learning.oreilly.com/videos/mlops-with-hugging/05282022VIDEOPAIML/05282022VIDEOPAIML-c1_s0/). The application source code is in [Github](https://github.com/nogibjj/hugging-face).

Next, let’s look at the application architectures in [Figure 2-23](#ch02.xhtml#Figure-2-0-2-mlops-hugging-face). A user account creates an authentication token that later becomes part of a continuous delivery pipeline in a cloud-based build system in [Github Actions](https://github.com/features/actions). The code itself develops in Github CodeSpaces. A Hugging Face model then lives inside a Gradio application, allowing for quick prototyping of an MLOps workflow by providing a user interface. Finally, the Hugging Face Spaces functionality allows users to create applications hosted on the platform using [Gradio](https://www.gradio.app) a technology for building machine learning apps.

![](assets/2-0-2-mlops-hugging-face.png)

```
from transformers import pipeline
import gradio as gr

model = pipeline("summarization")

def predict(prompt): 
    summary = model(prompt)[0]["summary_text"]
    return summary

with gr.Blocks() as demo: 
    textbox = gr.Textbox(placeholder="Enter text block to summarize", lines=4)
    gr.Interface(fn=predict, inputs=textbox, outputs="text")

demo.launch()
```

Let’s break each core application file; first there is the *app.py*.

[![1](assets/1.png)](#ch02.xhtml#co_the_stages_of_mlops_CO1-1)
: 

Use hugging face transformers (pre-trained model).

[![2](assets/2.png)](#ch02.xhtml#co_the_stages_of_mlops_CO1-2)
: 

Create the predict function.

[![3](assets/3.png)](#ch02.xhtml#co_the_stages_of_mlops_CO1-3)
: 

Build the gradio UI.

```
name: Sync to Hugging Face hub
on:
  push: 
    branches: [main]

  # to run this workflow manually from the Actions tab
  workflow_dispatch:

jobs:
  sync-to-hub:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
        with:
          fetch-depth: 0
      - name: Add remote
        env: 
          HF: ${{ secrets.HF }}  Use the token from Hugging Face
        run: git remote add space <your account>
      - name: Push to hub
        env:
          HF: ${{ secrets.HF }}
        run: git push --force <your account>
```

The other key file is *main.yml*, which controls the continuous delivery to Hugging Face. The actions are as follows:

[![1](assets/1.png)](#ch02.xhtml#co_the_stages_of_mlops_CO2-1)
: 

On push to Github build the project.

[![2](assets/2.png)](#ch02.xhtml#co_the_stages_of_mlops_CO2-2)
: 

Use the Hugging Face authentication token:

Finally, with the build process set up, you can see the working application in [Figure 2-24](#ch02.xhtml#Figure-2-0-3-hugging-face-demo). Any text passed into the submit box is then summarized using the Hugging Face pre-trained model. Later different models could be swapped out with just a line of code changed, and the entire application and the model would go live. A key takeaway is pre-trained models deployed in this MLOps fashion allow for rapid prototyping of what could later become a more sophisticated MLOps system.

![](assets/2-0-3-hugging-face-demo.png)

# Flow Automation (CI/CD for ML)

Continuous integration and continuous delivery (CI/CD) is an agile development approach for managing the life cycle of software and continuously deploying robust code updates to production. Using CI/CD, multiple developers can contribute code updates to a shared project repository, conduct automated testing, and have a controlled and continuous deployment process. The outcome is faster time to market using fewer resources and lower software failure rates.

The development of ML models and applications brings additional challenges which are not present in traditional software development:

*   Multiple personas participate in the development (data scientists, data engineers, software developers, ML engineers, and so on), each with different development skills, tools, and practices.
    
*   A version definition extends beyond code and incorporates data source objects, parameters, and multiple artifacts.
    
*   The different data and ML workloads (data preparation, model training, model, data and application testing, and so on) require high scalability and distributed processing using CPUs and GPUs.
    
*   Deployment of new versions to production involves merging different data assets and states (tables may change the schema, streams may be partially processed, new features are added and require historical values or imputing missing values, and so on).
    
*   Monitoring and observability are far more complex and less deterministic (as discussed in the previous section about the model and data monitoring).
    

To address the data and ML-specific challenges, organizations must extend their [CI/CD practices with MLOps automation](https://www.iguazio.com/glossary/ci-cd-for-machine-learning/) practices and ensure that the engineering and data science teams are aligned on the same development methodologies and tools. Here are some practices to follow:

*   Data scientists’ code can no longer be maintained in giant notebooks but rather broken into smaller functional code components (see: [“Writing and Maintaining Production ML Code”](#ch02.xhtml#write-production-ml-code)).
    
*   All data, code, parameters, artifacts, and results must be automatically collected, versioned, and correlated (see: [“Tracking and Comparing Experiment Results”](#ch02.xhtml#tracking-experiment-results)).
    
*   Tests should be extensive and cover all data, model, application aspects (see: [“Building and Testing Models For Production”](#ch02.xhtml#testing-models-for-production)).
    
*   Pipelines must support high-performance and distributed processing and efficient movement and versioning of data assets across the pipeline.
    
*   Model and data monitoring solutions should provide a feedback loop and be incorporated into the automation flow (see: [“Continuous Model and Data Monitoring”](#ch02.xhtml#model-and-data-monitoring)).
    

[Figure 2-25](#ch02.xhtml#ci-cd-pipe) demonstrates a typical CI/CD flow for ML applications. It consists of three main parts:

1.  Development: A user (data scientists, data engineers, software developers, and so on) creates a development branch from the latest code, adds his features, and conducts local tests using some sample data.
    
2.  Staging (or integration): The user requests to merge the new feature into the development branch. At this point, automated test procedures run over the new code, user larger datasets, and distributed or more scalable computation resources. Once the new code passes the tests and is approved, it merges into the development release and may undergo additional stress testing.
    
3.  Deployment to production: The development release is partially promoted to production (use canary or A/B testing deployment method to process small parts of the actual transactions). Once the new version is verified to work correctly and is compared to the prior release, it is approved and released to production. In case of failures or lower model performance, the system can be rolled back to the previous release.
    

![](assets/ci-cd-pipe.png)

The MLOps framework must have tight integration with the source control (GIT) and CI/CD framework you choose ([Jenkins](https://www.jenkins.io/), [GitHub Actions](https://github.com/features/actions), [Gitlab CI/CD](https://docs.gitlab.com/ee/ci/), and so on). Various metadata and configuration objects must be stored in the source repository along with the code, data referencing should be abstract and versioned, and reporting and APIs should be integrated to provide a holistic and avoid manual or complex integrations. You can see an example for integration in [Figure 2-26](#ch02.xhtml#cicd-results-in-github).

![](assets/cicd-results-in-github.png)

# Critical Thinking Discussion Questions

Let’s review the topics we discussed in this chapter. Can you answer the following questions?

*   Why is problem framing the initial suggested step for implementing a project following the MLOPs philosophy?
    
*   Name two or three examples of problems an organization could solve better with a heuristic than with machine learning?
    
*   How could your organization design an effective data governance strategy that proactively prevents PII, bias, or regulatory risk problems?
    
*   How could you use a Feature Store to decrease the computational training time of a model?
    
*   Consider a situation when your organization could face issues with data drift and when it faces problems with concept drift. Which would be the most significant impact if not resolved?
    

# Exercises

*   Use [mlrun serving](https://docs.mlrun.org/en/latest/serving/custom-model-serving-class.xhtml) to serve out a [Hugging Face model](https://huggingface.co/).
    
*   Use a Feature Store on an MLOPs platform to train a model that requires data transformation before training.
    
*   Use an experiment tracking technology like MLFlow, MLRun, ClearML, Sagemaker, or another MLOps platform to train multiple versions of a model and compare the accuracy of numerous runs.
    
*   Use an open-source framework like Spark, Dask, Horovod, or Nuclio for workload distribution to perform distributed hyperparameter tuning.
    
*   Write a serving application test using one of the examples covered earlier in the book.
    

# Chapter 3. Getting Started with your First MLOps Project

TBD: Chapter overview

# Identifying the Business Use Case and Goals

AI is transforming businesses and global economies. [PwC report predicts](https://www.pwc.com/gx/en/issues/data-and-analytics/publications/artificial-intelligence-study.xhtml) that AI could contribute as much as $15.7 trillion to the global economy by 2030. Moreover, 45% of total economic gains will come from product enhancements, stimulating consumer demand. This is because AI will drive greater product variety, with increased personalization, attractiveness and affordability over time. AI helps businesses increase their revenue, cut operational costs, improve productivity and reduce friction. Furthermore, it helps long-term strategic goals such as increasing competitiveness, reducing risks, growing user base and consumer loyalty, enhancing employee retention and enhancing brand value, which will translate to higher profitability and valuation in the long run.

For example, [according to McKinsey](https://www.mckinsey.com/industries/retail/our-insights/how-retailers-can-keep-up-with-consumers) up to 35% of Amazon’s revenue comes from AI-powered recommendations. By introducing the *Frequently Bought Together* recommendations (and other recommendations), Amazon was able to increase the average customer shopping cart size and order amount (upselling and cross-selling), which in turn increases average revenue per customer and Amazon’s e-commerce generated revenue per quarter.

[Netflix estimates](https://dl.acm.org/doi/10.1145/2843948) its personalized recommendation engine is worth $1 billion per year. According to the Netflix team, “consumer research suggests that a typical Netflix member loses interest after perhaps 60 to 90 seconds of choosing, having reviewed ten to twenty titles on one or two screens. After that, the user either finds something of interest or the risk of the user abandoning our service increases substantially.” So Netflix executives believe they could lose at least $1 billion annually if their subscribers aren’t offered a proper recommendation.

LATAM Airlines is the largest South American airline carrier. Its business was struck hard by COVID-19. They lost 80% of their revenue and they went into chapter 11. The CEO decided to double down on AI to reduce costs and increase profitability. While cutting costs left and right, they significantly grew their data science and MLOps teams and automated almost all parts of their business. Now they are in a much better financial situation. In one of the use cases, the goal was to improve the precision of flight fuel calculation to avoid carrying extra fuel. The project saved them tens of millions of dollars annually and significantly reduced CO2 emissions (which is also an important environmental benefit). In another use case, they used customer data to deliver custom packages and upsell options and the project resulted in tens of millions of dollars in extra revenue.

Another typical example of the significant cost savings AI can bring is in the use of chatbots. A [report from Juniper Research](https://www.juniperresearch.com/press/chatbots-to-deliver-11bn-cost-savings-2023) has found that adopting chatbots across the retail, banking and healthcare sectors will realize business cost savings of $11 billion annually by 2023, up from an estimated $6 billion in 2018. When implemented correctly, chatbots address customer service staff scalability needs, boost customer service quality and collect valuable consumer data.

Governments and non-profit organizations also use AI. They address universal needs such as national security, improved healthcare, environmental protection, child safety, education and more, which are not tied to measurable business goals but benefit the entire population. The following information is taken from a McKinsey report: [*Applying AI for Social Good*](https://www.mckinsey.com/featured-insights/artificial-intelligence/applying-artificial-intelligence-for-social-good). ([pdf](https://ec.europa.eu/futurium/en/system/files/ged/mgi-applying-ai-for-social-good-discussion-paper-dec-2018.pdf)) and lists the different use cases.

Table 3-1. AI for good use-cases

  
| Category || Application |
| --- | --- |
| Crisis response || Disease outbreak, Migration crises, Natural and man-made disasters, Search and rescue |
| --- | --- |
| Economic empowerment || Agricultural quality and yield, Financial inclusion, Initiatives for economic growth, Labor supply and demand matching |
| Education || Access and completion of education, Maximizing students achievement, Teacher and administration productivity |
| Environment || Animal and plant conservation, Climate change and adaptation, Energy efficiency and sustainability, Land, air, and water conservation |
| Equality and inclusion || Accessibility and disabilities, Exploitation, Marginalized communities |
| Health and Hunger || Treatment delivery, Prediction and prevention, Treatment and long-term care, Mental Wellness, Hunger |
| Information verification and validation || False news, Polarization |
| Infrastructure || Energy, Real estate, Transportation, Urban planning, Water and waste management |
| Public and social sector || Effective management of public sector, Effective management of social sector, Fundraising, Public finance management, Services to citizens |
| Security and justice || Harm prevention, Fair prosecution, Policing |

AI is also making a difference in the world by tackling significant sustainability challenges. One example I was involved in addressed flash flooding and freshwater availability in a new and innovative way. The Hydroinformatics Institute of Singapore began using thousands of CCTV cameras dispersed throughout a large Asian city as real-time sensors to analyze and measure rainfall. They use this to generate highly spatially-distributed ground-level rainfall data. Then, the data is fed into complex deep learning algorithms they built and deployed with an automated MLOps pipeline to create accurate, real-time rainfall predictions. They used rainfall predictions to manage floods by moving floodgates ahead and routing excess rainfall to reservoirs that can store and convert it into drinking water for the population.

## Finding the AI Use Case

When defining an AI project, the goal or hypothesis can’t remain at the abstract level of wanting to increase top-line revenue or cutting costs. Rather, it should address a specific use case or business problem and have measurable outcomes and ROI. For example, an application can provide purchasing recommendations based on products likely to be purchased together and increase average customer order size by X%, which will positively impact top-level goals like increasing average revenue per customer and top-line revenue.

Use cases will generally fall under one or more of the following categories (in order of complexity and value):

*   Intelligent forecasting and data analysis to support various decisions.
    
*   Innovative process or service automation to reduce costs and increase productivity.
    
*   New products and services which generate incremental value.
    
*   Simpler or better user experience, and autonomous systems (bots, robots, cars and so on).
    

The same project may address cost reduction and, at the same time, increase revenue or improve user experience. An example would be setting a goal around building an AI model to predict demand for a specific product. The prediction can help retailers ensure they do not run out of stock, which could result in lost revenue. An added benefit is an improved customer experience, which results in happier and more loyal customers who purchase the products they were looking for.

You can read the [McKinsey’s state of AI in 2021 report](https://www.mckinsey.com/capabilities/quantumblack/our-insights/global-survey-the-state-of-ai-in-2021), which describes the adoption of the most common AI use cases.

The more detailed article is at [10 Ways Artificial Intelligence Helps Business: Uses & Examples](https://www.intellspot.com/artificial-intelligence-business/). Here are the main points it describes:

**10 Ways Artificial Intelligence Can Help Your Business:**

*   *Boosts Revenue With Product Recommendations:* AI powered recommendation systems suggest products to users based on factors such as a customer’s purchase history or the behavior of similar users. Thus, the AI systems help customers find products that meet their needs.
    
*   *Chatbots For Enhancing Customer Service:* A chatbot is a software that imitates human conversation (or a chat) with users over websites, mobile apps, phones, etc. Today, people want fast, easy, and personalized customer service anytime and anywhere. Chatbots can provide it.
    
*   *Helps You Create The Best Content Marketing Strategy:* Artificial intelligence uses algorithms to analyze existing data and create the best content marketing strategies. AI helps you decide what to write about, which keywords to use, how often to publish, which channels to use for content distribution, etc.
    
*   *Sentiment Analysis To Gauge Customer’s Emotions:* Using sentiment analysis, businesses detect the opinions and feelings expressed by customers and measure feedback placed in millions of web pages, reviews, and forums.
    
*   *Powers your Competitive Intelligence:* By leveraging artificial intelligence in your competitive intelligence efforts, you can effortlessly spy, track, and understand what your competitors are doing and what makes them successful.
    
*   *Sales Forecasting To Grow Your Business:* AI allows for accurate sales forecasting. And the accurate sales forecasts enable business to calculate the probability of customer purchase, predict short-term and long-term performance, and allocate resources accordingly.
    
*   *Helps You Optimize Your Price:* Many factors influence your price: price history, brand reputation, competition, quality, season, operating costs, demand, etc. Artificial Intelligence can successfully take all of these factors into account when determining optimal prices.
    
*   *Smart Cybersecurity:* AI-powered software can deal with cybersecurity in many ways: vulnerability, managemen, phishing, detection, Network Security, behavioral analytics, prevention control, etc.
    
*   *Help You Creates Delightful In-store Experiences:* Artificial intelligence has the power to entirely transform the traditional in-store experience and take it to the next level with a personalization that delights customers.
    
*   *Saves Time And Reduce Costs:* Artificial intelligence technology allows businesses to automate a variety of processes, free up employees’ time, and help improve productivity.
    

The curated list of the [top 100 artificial intelligence use-cases](https://www.marketsandmarkets.com/top-100-artificial-intelligence-use-cases.asp) by vertical and importance can give you more ideas. The following diagram (from the article [How AI-powered content marketing can fuel your business growth](https://www.smartinsights.com/content-management/content-management-systems/how-ai-powered-content-marketing-can-fuel-your-business-growth/)) illustrates how AI can be used in different marketing use cases.

![](assets/marketing-use-cases.png)

The best way to start is to organize a brainstorming session with all the different business and technology stakeholders to get use case ideas and validate their feasibility.

## Defining Goals and Evaluating the ROI

Although there is an apparent increase in the success ratio of AI projects, many projects result in minimal or no value from their AI investments. One of the reasons is that relatively few projects are deployed into production, mainly due to cultural and organizational challenges; in many cases, they were treated as a bunch of small science projects that failed to realize an ROI. Moreover, production deployments are complex since they usually require integration with existing systems, processes, online data assets and applications, in a scalable and robust fashion.

One of the ways to increase the success rate is to define achievable and measurable goals. Identifying, prioritizing and setting goals is a multi-functional team effort that should include business owners, domain experts, data science and engineering teams and more. This helps ensure alignment with company goals while having the necessary business and domain expertise. AI initiatives may also require effective governance, compliance, ethics, cost and risk considerations.

To evaluate the ROI of the project, you need to consider the investments and returns, both direct (hard) and soft (indirect).

Investments:

*   People (data scientists, data engineers, MLOps, and so on)
    
*   Compute and data infrastructure
    
*   Software licenses and services
    
*   Consultants and training
    

Returns:

*   Cost savings
    
*   Increased revenue
    
*   Time-saving or increased productivity
    
*   Increased competitiveness or user base
    

It is essential to factor in the uncertainty of the benefits. AI models are likely to have errors and their accuracy is lower than 100%. So it would help if you estimated both the error rate and the cost of making mistakes. Furthermore, the fact that you made the correct prediction does not mean your action yields the expected user behavior. For example, you predict that a user would like the suggested product, but the recommendation was not delivered on time or wasn’t visible to the user.

[Figure 3-2](#ch03.xhtml#ai-roi) illustrates how to calculate the ROI for an ML project.

![](assets/roi.png)

Another challenge is that while historical data may be accessible, real-world data may behave differently or not be accessible, leading to different results in production deployments. As a result, machine learning-based AI models may deteriorate in performance over time. ROIs and KPIs should be monitored constantly so the value does not decay. Budgeting for MLOps solutions and continuous development and deployment models is also essential.

AI applications also bring possible risks and expose the organization to liability, security vulnerabilities, compliance or legal challenges and more.

On the upside, many of the investments in AI can be shared across multiple projects. Building common AI platforms, practices and knowledge sharing (an *AI Factory*) in the organization can significantly impact the ROI. According to [McKinsey’s state of AI in 2021 report](https://www.mckinsey.com/capabilities/quantumblack/our-insights/global-survey-the-state-of-ai-in-2021), “The companies seeing the biggest bottom-line impact from AI adoption are more likely to follow core and advanced AI best practices, including MLOps; move their AI work to the cloud; and spend on AI more efficiently and effectively than their peers.”

A significant impact on the ROI of a project is to find the right balance of buy versus build. Many products and services in the market today already incorporate AI, which can help reduce long development cycles and risks. Pre-trained or partially trained ML models can save time, resources and data. MLOps platforms can save significant development overhead and technical debt and allow you to focus on business problems.

# How to Build a Successful ML Project

Various surveys indicate that the major impediments to the success of AI in the organization are cultural challenges, such as slow adaptability to change, reengineering of business processes, staff education, data literacy requirements, organizational alignment and elimination of silos to support business objectives. Many organizations report that direct involvement from C-level executives is essential to the success of AI projects. The Harvard Business Review dedicated an article to the vital role of [CEOs in leading a data-driven culture](https://hbr.org/2020/03/how-ceos-can-lead-a-data-driven-culture), McKinsey also writes about [the role of the CEO and MLOps](https://www.mckinsey.com/capabilities/quantumblack/our-insights/scaling-ai-like-a-tech-native-the-ceos-role).

But addressing the cultural challenges is not enough. To achieve a successful AI strategy, you need to redesign all your business processes and tasks around data and AI:

*   Build systems and processes for continuously collecting, curating, analyzing, labeling and maintaining of high-quality data. The most significant impediment to effective algorithms is insufficient or poor data.
    
*   Develop effective and reliable algorithms that can be explained, are not biased against particular groups or individuals, are correctly fit, continuously monitored and regularly updated using fresh data.
    
*   Integrate a business application’s data assets, AI algorithms, software and user interface into a single project with clear ownership and milestones. Avoid organizational silos.
    
*   Build robust engineering and MLOps practices to continuously develop, test, deploy and [monitor](https://www.iguazio.com/glossary/model-monitoring/) end-to-end ML applications.
    

## Approving and Prototyping the Project

Before committing to a project, you need to build and approve your plan. To do so, answer the following questions:

1.  *Objective*: What are the objectives of this AI use case and are they aligned with the strategic business goals?
    
2.  *KPIs*: What will qualify as success and how will it be measured?
    
3.  *Data*: Do you have enough data (and labels) to train the models? Can you obtain the same data in production and inference? Does the data contain bias? Can you get fresh labeled data for retraining?
    
4.  *People*: Who will be responsible (the owner) for the project? Which resources and skills are needed? Are they available or do you need to hire them?
    
5.  *Algorithms*: Which AI approach and algorithms are you planning to use? Can you find an existing model?
    
6.  *Ethics and risks*: Are there any ethical or legal issues regarding this use case (privacy, GDPR, bias)? Are any security risks introduced? Can you protect the model from malicious attacks?
    
7.  *Infrastructure*: What are the technology and infrastructure challenges and requirements? What are the implementation challenges? What are the expected infrastructure costs?
    
8.  *Continuity*: Can you continuously monitor and maintain the application? Can you update the data and model frequently enough? How do you verify the KPIs and ROI once the application is deployed?
    

After answering these questions and getting approval for the project, the next step is to validate the hypothesis and prototype the application by using rapid prototyping and simulation tools.

*   Manually gather data from different sources. Make sure the data you use can, later on, be ingested and prepared continuously at scale.
    
*   Explore the data and look for patterns and signals. Verify which datasets and features are required and which don’t add value. Next, try out derived features (date extractions, aggregates, indirect values such as turning zip code numbers to demographics or geolocation data and so on.)
    
*   Prepare the data, train a model using a relevant subset of the data, validate that it performs as expected and try out different frameworks, algorithms and existing models.
    
*   Build a prototype application that simulates the end-to-end flow: receives a request, prepares the data, infers using the model, drives actions and integrates with external APIs/systems, logs vital metrics for performance and KPI monitoring and so on.
    

Building a prototype can save time, reduce risks and improve the results. An excellent way to save time and energy is to have project templates with an application skeleton and best practices. Moving from the prototype to a production ML application can be done in multiple iterations, adding more data, logic and robustness in every iteration. It is essential to break the project into functional modules from day one (for example, data preparation, [training](https://www.iguazio.com/glossary/model-training/), testing, serving and so on) and define interfaces between the modules. This allows independent development of each module and better collaboration between team members.

You must define the initial prototype’s scope, milestones and objectives. Once it is implemented and the goals can be evaluated, the executive team needs to approve the productization of the project and allocate the extended required resources for its success.

## Scaling and Productizing Projects

ML projects that are designed for production and scale consist of the following three pipelines:

*   *Data pipeline*: Tap into the full-scale historical, operational and [real-time data sources](https://www.iguazio.com/glossary/real-time-ml/) and transform the raw data into features for use in the training and inference stages. [Feature stores](https://www.iguazio.com/glossary/feature-store/) can accelerate the development of a data pipeline and enable the reuse of existing data features from other projects.
    
*   *Model development (CI/CD) pipeline*: Automate the process of getting relevant data, data validation, training with different parameters and frameworks, evaluating and testing the model, deploying the inference pipeline and so on.
    
*   *Application pipeline*: Intercept requests or events, enrich and process data, use the model for inference, apply relevant actions and monitor various resources, data, model, and KPI metrics.
    

The pipelines must be designed for continuous development and operations. New versions can be deployed without disrupting the overall application. It is recommended to work in sprints (weekly, monthly). At the end of each sprint, look at the complete application in action. Each sprint provides more functionality or robustness until you reach a deployable and production-quality application. After deploying the application, keep iterating with feature improvements or bug fixes.

The [Figure 3-3](#ch03.xhtml#mlops-engineering-flow) illustrates the project engineering flow.

![](assets/ml-engineering.jpg)

In most cases prevalent today, each pipeline uses a different framework and is maintained by different teams with different skills. For example, [Spark](https://spark.apache.org/), [Flink](https://flink.apache.org/) and [Airflow](https://airflow.apache.org/) for data pipelines, [Kubeflow](https://www.kubeflow.org/) for model development and plain containers or serverless functions for the application pipelines. Having such a large variety of tools and frameworks creates operational challenges. Furthermore, each framework works with different metadata layouts and APIs, forcing additional glue logic and conversions. Therefore, working with standard metadata and abstractions across frameworks is essential for simplifying and streamlining deployments.

ML projects are developed continuously and collaboratively by different team members. Therefore, a versioned source control system like [Git](https://git-scm.com/), an agile development process and CI/CD automation are mandatory requirements for a successful outcome.

## Project structure and lifecycle

ML Project is a container for all your work on a particular ML application. Projects host functions, workflows, artifacts, notebooks, features and configurations. Projects have owners and members with role-based access control, which should define who can access what and how.

![](assets/ml-project-elements.png)

Project components (see: [Figure 3-4](#ch03.xhtml#ml-project-elements)):

*   *Functions*: Code elements along with their package requirements, configuration, metadata, resource definitions and more.
    
*   *Workflows*: Pipeline (DAG) definitions, like which step comes after which and how parameters are passed between steps.
    
*   *Artifacts*: Metadata and pointers to various data artifacts (files, datasets, models and more.) used in the project.
    
*   *Notebooks*: [Jupyter](https://jupyter.org/) notebooks used for interactive development, data exploration and visualization. It is recommended to only store production code in notebooks since it’s harder to test, automate and track changes in notebooks.
    
*   *Features*: Definitions of feature store features and the data pipelines that generate or retrieve those features (usually called feature sets and feature vectors).
    
*   *Configurations*: Parameters, secrets, build and installation instructions and more.
    

Projects should be stored and versioned in a source control system (Git) or archived. Then, they can be opened and edited as a project in the different IDEs (in Jupyter, [PyCharm](https://www.jetbrains.com/pycharm/), [VSCode](https://code.visualstudio.com/) and others). This approach enables versioning, collaboration and CI/CD.

You can define best practices and project templates for your team. This can simplify creating new projects and helps focus and prioritize in existing projects. [Example 3-1](#ch03.xhtml#project-dir-struct) demonstrates how you can define the project directory structure:

##### Example 3-1. Project directory layout example

```
my-project           # Parent directory of the project (context)
├── data             # Project data for local tests or outputs (not tracked by version control)
├── docs             # Project documentation
├── src / pkg-name   # Project source code (functions, libs, workflows)
├── tests            # Unit tests (pytest) for the different functions
├── notebooks        # directory for storing notebooks
├── README.md        # Project README
├── requirements.txt # Default Python requirements file (may have per function requirements files)
├── setup.py         # Python package setup file
├── LICENSE          # License file
└── ...
```

MLOps frameworks like [MLflow](https://mlflow.org/) and [MLRun](https://www.mlrun.org/) store additional metadata and configuration files (MLproject, project.yaml) in the project directory. This allows loading a project from Git, reconstructing all its objects and configurations automatically and versioning the configurations and metadata (a.k.a Git-ops).

In a continuous development and integration flow (illustrated in [Figure 3-5](#ch03.xhtml#ml-project-lifecycle)), developers create an ML project and write and test their code and models. Once it is ready, they push the changes into the source control system (Git). Next, the project is loaded into a development cluster to run an additional set of automated tests on larger datasets and a system that resembles the production setup. Then, bug fixes are applied to the code until the project becomes stable and the release is due. Finally, a version tag is assigned to the project when the release is ready and that version is deployed on the production cluster in a rolling upgrade process.

![](assets/ml-project-lifecycle.png)

Dividing the project into functional building blocks (functions, workflows, features and so on) and using Git enables continuous and collaborative development. Furthermore, placing code, ML objects, metadata and configurations in the same versioned project repository simplifies testing, deployment into production and rollback to older versions in case of problems.

# ML Project Example From A to Z

This section demonstrates a complete ML project and the development flow from initial exploration to continuous deployment at scale.

The development and productization flow consists of the following steps (illustrated in [Figure 3-6](#ch03.xhtml#project-dev-flow)):

*   Initial data gathering (for exploration).
    
*   Exploratory data analysis (EDA) and modeling.
    
*   Data and model pipeline development (data preparation, training, evaluation and so on).
    
*   Application pipeline development (intercept requests, process data, inference and so on).
    
*   Scaling and productizing the project (adding tests, scale, hyper-parameter tuning, experiment tracking, monitoring, pipeline automation and so on).
    
*   Continuous operations (CI/CD integration, upgrades, retraining, live ops and so on).
    

![](assets/project-dev-flow.png)

## Exploratory Data Analysis (EDA)

*Exploratory data analysis* (EDA) enables an in-depth understanding of your datasets, their quality and how they influence the target variable. EDA is vital for determining which raw and derived features should be used in the model and for examining your hypotheses. In many cases, EDA requires domain-specific knowledge (or intuition) to determine which variables can be used and how they can impact the model. EDA is usually a manual and interactive process but can use various tools to automate and better visualize the information gathering and analysis.

The EDA process consists of the following steps:

1.  Importing relevant datasets (extraction from the overall data).
    
2.  Understanding the data structure and statistics.
    
3.  Cleaning and sanitizing.
    
4.  Transforming - generating derived features.
    
5.  Feature analysis.
    
6.  Cross-feature relationships and correlation analysis.
    
7.  Prototyping a model and evaluating feature importance.
    

The process is iterative. You may need to obtain more data from other sources or you may find that some of the data is useless and doesn’t contribute to your model or that various transformations yield better results.

The first step is understanding the data shape, types, statistical distribution, categories, missing values and so on. Next comes data cleansing, removal of useless columns or rows, handling missing values, removing duplicates and identifying and fixing recording errors.

In many cases, the raw features are not a good indicator and you will need to create derived features that correlate better with the target results. Some examples:

*   Extracting date/time components (hour of the day, day in the week, is\_weekend and so on.) from a date/time field.
    
*   Value mappings (log scale, binning, encoding, grouping and so on).
    
*   Aggregation over time or entity (number of clicks in the last hour, total purchases by customer).
    
*   Important features obtained by joining data from a secondary dataset (map zip code to geolocation or social economic information, map product ID to its price or category and so on).
    

Once you have all the features, it’s time to analyze and visualize their behavior, histograms, outliers and categorization. Potentially you can apply transformations to improve the data quality and impact.

The next step is to find interesting relationships that show the influence of one variable on another, preferably on the target. Some features may not have any impact and can be removed while some may need to be transformed to increase their influence. At this stage, you can also evaluate the data for potential bias.

When the features are sanitized and prepared, you can build a basic model or use AutoML tools for prototyping a model. Once you have a model, examine your hypothesis to see if you can predict the target variable and evaluate the importance and necessity of the features you used.

## Data and Model Pipeline Development

In the EDA phase, the process was exploratory and interactive. Now it’s time to build the data preparation, modeling and testing and turn them into high-quality, robust and reusable code. As discussed in chapter 2, a preferred approach is to create individual python functions for each stage, give them parameters, record their outputs and create unit tests. Then, notebooks can execute those functions interactively and visualize their results.

Once the individual functions work, you can create a workflow (DAG), run the different tasks in an automated pipeline over scalable local or cloud resources and integrate that workflow into an automated CI/CD process.

A minimal pipeline includes the following steps:

1.  Data preparation: Prepare the training and testing datasets.
    
2.  Model training: train the model with the dataset and some parameters.
    
3.  Evaluation: evaluate the model against the test dataset and generate various reports and metrics.
    

Real-world pipelines will have more test and deployment steps and run the training step with different parameter combinations (hyper-parameter tuning).

You can see code examples for the three functions below, [Example 2-1](#ch02.xhtml#data-prep-func) demonstrates accepting a dataframe and required test size, processing the data, and splitting it to train and test datasets.

##### Example 3-2. Data preparation function code example (partial)

```
def data_preparation(dataset: pd.DataFrame, test_size=0.2):
    """A function which preparation training dataset

    :param dataset: input dataset dataframe
    :param test_size: the amount (%) of data to use for test

    :return train_dataset, test_dataset, label_column
    """
    dataset = clean_df(dataset).dropna(how="any", axis="rows")

    ... additional processing

    train, test = train_test_split(dataset, test_size=test_size)
    return train, test, label_column
...
```

###### Note

Using the feature store is a more powerful and automated way to process data. This will be discussed in the next chapter.

The next step is to train the model with the newly prepared dataset. For example, the following function (see: [Example 3-3](#ch03.xhtml#trainer-func)) accepts the training dataset and various parameters (which will be used for hyper-parameter tuning in the following sections), trains the model and returns the ML model.

##### Example 3-3. Model trainer function code

```
def train(
    dataset: pd.DataFrame,
    label_column: str = "label",
    n_estimators: int = 100,
    learning_rate: float = 0.1,
    max_depth: int = 3,
    model_name: str = "cancer_classifier",
):
    # Initialize the x & y data
    x = dataset.drop(label_column, axis=1)
    y = dataset[label_column]

    # Initialize the ML model
    model = ensemble.GradientBoostingClassifier(
        n_estimators=n_estimators, learning_rate=learning_rate, max_depth=max_depth
    )

    # Train the model
    model.fit(x, y)
    return model
```

The final step is to evaluate the model using the test set. The evaluate function (<<>>) accepts the trained model and training set as inputs and generates various reports and charts (such as ROC curves, feature importance, etc.). Since the evaluate function is generic, it can be implemented once, stored in a shared repository like MLRun’s functions hub, and used in multiple projects. A the complete implementation of the training and evaluation functions can be viewed in [MLRun train and evaluate hub function](https://www.mlrun.org/hub/functions/master/auto_trainer/latest/source/).

##### Example 3-4. Evaluation function code example (partial)

```
def evaluate(
    model,
    dataset: pd.DataFrame,
    label_columns: Optional[Union[str, List[str]]] = None,
):
    """
    Evaluating a model and generate reports and artifacts.

    :param model:             The model path or object.
    :param dataset:           The dataset to evaluate the model on.
    :param label_columns:     The target label(s) of the column(s) in the dataset.
                              for Regression or Classification tasks.
    """

    # load the model and dataset
    # run prediction using the test dataset
    # generate plots and reports and log them to the artifacts store
    # update the model, the result metrics and metadata in the model registry
```

Once you have implemented the three functions, you can define an execution pipeline (DAG) to run a function and feed its results to the next step and so on.

A big part of MLOps is to be able to record all the inputs, metadata, data, and results per experiment (a.k.a experiment tracking) to make it possible to understand and explain how specific model results were obtained. For example, MLFlow and MLRun MLOps frameworks track the execution of the functions and log the data and results. This will be covered in more detail in the following chapters.

## Application Pipeline Development

Models only bring value if deployed and integrated into an actual application. For example, an ML application receives relevant data or requests from users or other services, processes it and uses it with the model to make predictions and generate some actions. In addition, production applications require monitoring, logging, lifecycle management and so on. The flow of application, data, model and monitoring activities is called an Application Pipeline.

There are two types of Application pipelines. Real-time (or online) pipelines, which constantly accept events or requests and respond immediately, and batch pipelines, which are triggered through an API or at a given schedule. Batch pipelines usually read and process larger datasets on every run.

**Real-time application pipelines**

[Figure 3-7](#ch03.xhtml#rt-app-pipeline) describes a typical real-time application pipeline and its different steps. An application pipeline may receive a request and respond in real-time (synchronous) or it may process the request and write the results to another service or a storage/database system (asynchronous or streaming pipeline).

![](assets/rt-app-pipeline.png)

Real-time pipelines can be implemented manually by chaining individual containerized functions or can be automated by using a real-time pipelines framework such as MLRun Serving Graphs, Apache Beam, or AWS Step Functions. Chapter 6 covers the different options in detail.

###### Note

Some of the data processing logic implemented for the training pipeline is now re-implemented for an event-driven architecture (processing in real-time, event by event vs. working with large data frames). This engineering overhead can be eliminated when using a feature store. The feature transformation logic is generated automatically for batch and real-time from the same abstract definition (called a feature set).

Once the application pipeline and models are deployed, they can be automatically tracked and monitored to identify resource usage, model drift, model performance and more.

**Batch application pipelines**

[Figure 3-8](#ch03.xhtml#batch-app-pipeline) describes a typical batch (offline) application pipeline and its different steps. For example, an API call or a scheduled event may trigger the batch pipeline. It loads and processes one or more datasets, conducts batch inference and post-processing and writes the results to the target storage.

![](assets/batch-app-pipeline.png)

The batch application pipeline can be executed with the same pipeline engine used for training.

## Scaling and Productizing the Project

After building the model development components (data prep, training, evaluation) and the application pipeline, it’s time to add tests and automate and scale the project.

**Adding Tests**

The first step for increasing your ML application’s quality is adding unit tests for each component. The common practice is to use [pytest](https://docs.pytest.org/) and place the test files under the `/test` directory. For example, the following code demonstrates an implementation of unit tests for the data_prep function.

```
def test_data_preparation_pipline():
    df = get_data()
    train, test, label = data_preparation(df, 0.2)

    assert label == 'fare_amount'

    # check for expected types
    assert isinstance(train, pd.DataFrame)
    assert isinstance(test, pd.DataFrame)
```

You can build additional unit tests for each function or module and verify you’ve properly covered the usage patterns. To run the tests, simply execute pytest and point to the tests directory. Repeat these steps each time you push changes to the source repository.

```
python -m pytest tests
```

Numerous python packages test your code for formatting, conformance, quality, coverage and more (such as black, isort, flake8, and interrogate). You can add them to your project and execute them before committing the code or as part of the CI process.

For example, formatting the code in \\src and \\tests using black:

```
python -m black src tests
```

Additional tests (validating the data, model, and APIs) should be implemented as part of the ML pipeline.

**ML Pipelines and Hyperparameter Optimization**

To find the best model, you should run the same training code with different parameter combinations (hyperparameter search). However, doing it on your laptop can take time and resources. Instead, define the search options and let it run in parallel in the cloud or over a cluster. Many MLOps frameworks support hyperparameter jobs. If you provide the hyperparameter options (strategy, selection criteria, parallelism level, resources, early stop and so on), they will execute all the permutations for you and automatically select the best results.

Model development is a multi-stage process. It requires data ingestion, preparation, validation, training one or more models and testing and evaluating the models. You can also deploy and test the application pipeline with the newly generated model and data items.

Most MLOps platforms have a way to describe and run a complete workflow (DAG) of steps. Some of the well-known open-source workflow execution tools are Airflow and Kubeflow pipelines. There are some CI frameworks like [GitHub Actions](https://github.com/features/actions), [Gitlab CI](https://docs.gitlab.com/ee/ci/) and [Jenkins](https://www.jenkins.io/), which can run simple workflows. But the CI workflow tools lack MLOps capabilities such as handling large datasets, tracking execution and artifacts, running distributed workloads and others.

Frameworks such as MLRun add the missing MLOps features to the CI/CD system and simplify the way ML pipelines are built and executed. After you have implemented, executed and tested each function, you only need to place them in a DAG and run it. MLRun works with different underline workflow engines, such as Kubeflow pipelines, and can work with all the CI frameworks.

A CI/CD pipeline for an ML application will likely implement the following steps:

1.  Data preparation
    
2.  Model training using hyperparameters and grid search
    
3.  Model evaluation
    
4.  Application pipeline deployment (with the best model)
    

When the pipeline is executed with MLRun, MLRun tracks the progress and results, and you can view them in the client (IDE, Jupyter, or others) or in the MLRun UI. [Figure 3-9](#ch03.xhtml#mlrun-workflow-screen) shows an example of the workflow tracking UI in MLRun.

![](assets/mlrun-workflow-screen.png)

## CI/CD and Continuous Operations

You now have all the ingredients: data pipelines, model development pipelines and application pipelines. However, those components will continuously develop and become enhanced. They require an agile process for monitoring results, pushing updates, testing and deployment.

**Continuously Monitoring Data and Models**

In traditional services, we monitor application performance, resource usage, errors and more. However, it is critical in ML applications to also monitor the data and models (see REFTO: Continuous Model and Data Monitoring in Chapter 2).

The data and model monitoring layers take metadata collected at the data preparation and model training phase (data types, statistics and others) and compare it with production data and metrics. MLRun automates this process. The metadata is automatically recorded with the model and the features at development time and compared in real-time or periodically with metadata and behavior of the production data (which is generated automatically by the model serving classes).

You probably want to avoid constantly staring at dashboards for model or data performance problems. Instead, you can define triggering policies and actions. For example, when a certain threshold is reached, a notification can alert the administrator or initiate an automated process for retraining a model or mitigating potential errors.

**Integrating with a CI/CD Service**

CI/CD (continuous integration and deployment) is the standard approach for building and maintaining modern services in an agile process. Chapter 2 covered REFTO: CI/CD for ML and the differences from traditional CI/CD. The reference project uses MLRun to extend the GitHub Actions CI service to ML and data applications.

As a first step, you need to create scripts that will execute the different tests and verifications. The standard approach is to use a Makefile. In the Makefile, add commands to build, test and so on. Here are some examples for make commands (see the complete Makefile in the project directory):

```
.PHONY: lint
lint: fmt-check flake8 ## Run lint on the code

.PHONY: fmt-check
fmt-check: ## Format and check the code (using black and isort)
	@echo "Running black+isort fmt check..."
	$(PYTHON_INTERPRETER) -m black --check --diff src tests
	$(PYTHON_INTERPRETER) -m isort --check --diff src tests

.PHONY: flake8
flake8: ## Run flake8 lint
	@echo "Running flake8 lint..."
	$(PYTHON_INTERPRETER) -m flake8 src tests

.PHONY: test
test: clean ## Run tests
	$(PYTHON_INTERPRETER) -m pytest -v --capture=no --disable-warnings tests
```

With this make file, typing `make lint` or `make test`, will run the lint and pytest tests.

###### Note

The CI/CD system (such as Jenkins or GitHub Actions) examines your project and searches for CI scripts in a reserved directory and executes them when the code is changed or merged.

In addition to static tests, you should automatically run the ML pipeline. However, since ML pipelines can consume significant computation, you may want the user to explicitly request running the ML pipeline. This can be done by typing a command in the Git pull request (for example /run), which will trigger the execution of the ML pipeline on cloud resources and automate the execution, data movement and tracking.

You can use the same approach to automate deployment, run exhaustive testing, apply governance and more, while adding more CI scripts and ML pipelines to match them and restricting who can execute which workflow and at what stage (development, staging, production).

# Critical Thinking Discussion Questions

*   TBD
    

# Exercises

*   TBD
    

# Chapter 4. Working with Data and Feature Stores

Machine learning takes data and turns it into predictive logic. Data is essential to the process, can come from many sources, and must be processed to make it usable. Therefore, data management and processing are the most critical components of machine learning.

Data can originate from different sources:

*   *Files*: Data stored in local or cloud files.
    
*   *Data warehouses*: Databases hosting historical data transactions.
    
*   *Online databases*: SQL, NoSQL, graph, or time-series databases hosting up to data transactional or application data.
    
*   *Data Streams*: Intermediate storage hosting real-time events and messages (for passing data reliably between services).
    
*   *Online services*: Any cloud service that can provide valuable data (this can include news sites, social, financial, government, and news services).
    
*   *Incoming messages*: Asynchronous messages and notifications which can arrive through emails or any other messaging services (Slack, WhatsApp, Teams, and so on).
    

Source data is processed and stored as features for use in the [model training](https://www.iguazio.com/glossary/model-training/) and model flows. In many cases, features are stored in two storage systems. One for batch access (training, batch prediction, and so on) and one for online retrieval (for real-time or online serving). As a result, there may be two separate data processing pipelines, one using batch processing and the other using real-time (stream) processing.

The data sources and processing logic will likely change over time, resulting in changes to the processed features and the model produced from that data. Therefore, applying versioning to the data, processing logic, and tracking data lineage are critical elements in any MLOps solution.

Delivery of accurate and high-quality production models requires high volumes of data and significant processing power. Processing of production data can be scaled using distributed analytics engines ([Apache Spark](https://spark.apache.org/), [Dask](https://www.dask.org/), [Google BigQuery](https://cloud.google.com/bigquery), and more), stream processing technologies (like [Apache Flink](https://flink.apache.org/)), or multi-stage data pipelines.

One of the mechanisms to automate integration with data sources, scalable batch and real-time data processing, data versioning, and feature management is to use a feature store. A feature store is a central hub for producing, sharing and monitoring features. Feature stores are essential in modern MLOps implementations and will be described in further detail in this chapter.

# Data Versioning and Lineage

Models and data products are derived from data. Therefore, collecting metadata and tracing the origin of the data allows better control and governance for data products. Furthermore, if you want to examine a specific version of a data product, you must understand the original data used to produce that product or model.

Data versioning, lineage, and metadata management are a set of essential MLOps practices that address the following:

1.  *Data quality*: Tracing data through an organization’s systems and collecting metadata and lineage information can help identify errors and inconsistencies. This makes it possible to take corrective action and improve data quality.
    
2.  *Model reproducibility and traceability*: Access to historical data versions allows us to reproduce model results and can be used for model debugging, troubleshooting, and trying out different parameter sets.
    
3.  *Data governance and auditability*: By understanding the origin and history of data, organizations can ensure that data follows expected policies and regulations, track sources of errors, and perform audits or investigations.
    
4.  *Compliance*: Data lineage can help organizations demonstrate compliance with regulations such as GDPR and HIPAA.
    
5.  *Simpler data management*: Metadata and lineage information enables better data discovery, mappings, profiling, integration, and migrations.
    
6.  *Better collaboration*: Data versioning and lineage can facilitate cooperation between data scientists and ML engineers by providing a clear and consistent view of the data used in ML models and when handling upgrades.
    
7.  *Dependency tracking*: Understanding how each data, parameters, or code change contributes to the results and providing insights into which data or model objects need to change due to data source modification.
    

## How It Works

As shown in [Figure 4-1](#ch04.xhtml#data-lineage), the data generation flow can be abstracted as having a set of data sources and parameters that are used as inputs to a data processing (computation) task that produces a collection of data products or artifacts. The output artifacts can be of different types, files, tables, machine learning models, charts, and so on.

![](assets/data-lineage.png)

The data tracking system records the information about the inputs (data sources and versions, parameters) and computation tasks (code, packages, resources, executing user, and more). Then, it adds it as metadata in the output artifacts. The metadata may include additional information like user-provided labels or tags, information about the data structure, schema, statistics, and so on. The metadata is usually not copied to each output artifact but is instead referenced (by a link) to eliminate data duplication.

As shown in [Figure 4-2](#ch04.xhtml#multi-stage-lineage), output artifacts from the first task (for example, data preparation) can be used as data inputs to the following tasks (for example, model training, testing, and more).

![](assets/multi-stage-lineage.png)

When accessing a data product through a user interface or an SDK, the metadata lets us see the exact data sources, parameters, and the full details of the computation task. We can also trace back the progress of the data generated in a multi-stage flow and examine all the additional metadata.

Every time the data processing task runs, a new version of the output artifacts is created (see [Figure 4-3](#ch04.xhtml#data-lineage-vers)). Each version is marked with a unique version identifier (commit id) and can also be tagged with a meaningful version name, such as *master*, *development*, *staging*, *production*, and so on. This is similar to the GIT flow when versioning source code.

![](assets/data-lineage-vers.png)

Let’s assume you are repeatedly running a specific task every hour. It has the same inputs and parameters or you might make small changes that do not change the output data results. This can lead to vast piles of redundant data and multiple versions will store the same content. Many data versioning solutions implement *content deduplication* to address this challenge.

When an artifact is produced, a cryptographic hash value of the content is calculated (for example, using the MD5 or SHA1 algorithms), which represents the uniqueness of the content. Finally, the hash value is compared with older versions or is used as an index in the storage system. This way, the content is stored only once.

Since the nature of data versioning solutions is to track various attributes in addition to the source data (code, parameters, users, resources, and more), it must be well integrated with the source control system (GIT) and the job or pipeline execution framework. Otherwise, the user must manually glue the frameworks together and provide the reference metadata for recording it along with the data.

Many frameworks ([MLflow](https://mlflow.org/), [MLRun](https://www.mlrun.org/), and more) provide a logging API, where the user calls a `log_artifact()` method, which automatically records and versions the new data along with the code and execution metadata. Many might offer an `auto logging` solution that does not require code instrumentation. Instead, it will automatically figure out which data and metadata need to be saved and versioned by understanding the user code and the ML framework’s capabilities.

## Common ML Data Versioning Tools

A set of open-source and commercial frameworks for data versioning exists. This book focuses on explaining and comparing the open-source options [DVC](https://dvc.org/), [Pachyderm](https://www.pachyderm.com/), MLflow, and MLRun.

**DVC (Data Version Control)**

DVC started as a data versioning tool for ML and was extended to support basic [ML workflow automation](https://www.iguazio.com/glossary/automated-machine-learning/) and experiment management. It takes advantage of the existing software engineering toolset you’re already familiar with (Git, your IDE, [CI/CD](https://www.iguazio.com/glossary/ci-cd-for-machine-learning/), and so on).

DVC works just like GIT (with similar commands) but for large file-based datasets and model artifacts. This is its main advantage, but also its weakness. DVC stores the data content in files or an object storage (AWS S3, GCS, Azure blob, and so on) and keeps a reference to those objects in a file (*.dvc*), which is stored in the GIT repository.

The following commands will add a local model file (*model.pkl*) to the data versioning system.

```
dvc add model.pkl
```

DVC will copy the content of the *model.pkl* file into a new file with a new name (derived from the content *md5* hash value) and place it under the *.dvc/* directory. It also creates a file named `model.pkl.dvc`, which points to that content file. Next, the new metadata file needs to be tracked by GIT, the content should be ignored, and the changes should be committed. This is done by typing the following commands:

```
git add model.pkl.dvc .gitignore
git commit -m "Add raw data"
```

When you want to upload the data to your remote storage, you will need to set up a remote object repository (not shown here) and use the DVC push command:

```
dvc push
```

The data flow is illustrated in [Figure 4-4](#ch04.xhtml#dvc-flow)

![](assets/dvc-flow.png)

As you can see from the example, DVC provides reliable synchronization between code and file data objects, but requires manual configuration, and does not store extended metadata about the execution, workflow, parameters, and so on. Instead, DVC handles parameters and results metrics using JSON or YAML files stored and versioned alongside the code.

Users can define workflow stages that wrap an executable (for example, a Python program) and specify which parameters (`-p`) are the file inputs or dependencies (`-d`) and outputs (`-o`) to that executable. See example 4-1:

##### Example 4-1. Adding a workflow step in DVC

```
dvc stage add -n featurize \
              -p featurize.max_features,featurize.ngrams \
              -d src/featurization.py -d data/prepared \
              -o data/features \
              python src/featurization.py data/prepared data/features
```

When you run the `dvc repro` command, it will evaluate if the dependencies have changed, execute the required steps, and register the outputs.

DVC does not use an experiment database. It uses GIT as the database, and every execution or parameter combination is mapped to a unique GIT commit. Furthermore, DVC is focused on local development. Therefore, using it at scale or in a containerized or distributed workflow environment can be challenging and require scripting and manual integrations.

In summary, DVC is an excellent tool for versioning large data artifacts and mapping them to GIT commits in a local development environment. In addition, DVC implements data deduplication to reduce the actual storage footprint. On the other hand, DVC is command-line oriented (GIT flow), has limited capabilities for running in production, executing pipelines, and tracking extended attributes and structured data. It also comes with a minimal UI (studio).

**Pachyderm**

Pachyderm is a data pipeline and versioning tool built on a containerized infrastructure. It provides a versioned file system and allows users to construct multi-stage pipelines, where each stage runs on a container, accepts input data (as files), and generates output data files.

Pachyderm provides a versioned data repository that can be implemented over object storage (AWS S3, Minio, GCS, and so on) and accessed through a file API or the SDK/CLI. Every data commit or change is logged similarly to GIT. Data is deduplicated to preserve space.

The Pachyderm data pipeline executes containers and mounts a slice of the repository into the container (under the */pfs/* directory). The container reads files, processes them, and writes the outputs back into the Pachyderm repository.

[Example 4-2](#ch04.xhtml#pachyderm-pipeline) shows a simple pipeline definition that takes all the data from the `data` repository on the `master` branch, runs the word count logic (using the specified container image), and writes the output to the `out` repository.

##### Example 4-2. Pachyderm pipeline example

```
pipeline:
    name: 'count'
description: 'Count the number of lines in a csv file'
input:
    pfs:
        repo: 'data'
        branch: 'master'
        glob: '/'
transform:
    image: alpine:3.14.0
    cmd: ['/bin/sh']
    stdin: ['wc -l /pfs/data/iris.csv > /pfs/out/line_count.txt']
```

Pipelines can be triggered every time the input data changes, and data can be processed incrementally (only new files will be passed into the container process). This can save time and resources.

Pachyderm has a nice user interface for managing pipelines and exploring the data. See [Figure 4-5](#ch04.xhtml#pachyderm-overview).

![](assets/pachyderm-overview.png)

Pachyderm can work with large or continuous structured data sources by breaking the data into smaller CSV or JSON files.

In summary, Pachyderm is an excellent tool for building versioned data pipelines, where the code is simple enough to read and write files. It handles deduplication and incremental processing. However, it requires separate tracking of the source code (runs pre-built images), execution or experiment parameters, metadata, metrics, and more.

**MLflow Tracking**

MLflow is an open-source platform for managing the end-to-end machine learning lifecycle. One of its core components is MLflow Tracking. MLflow Tracking provides an API and UI for logging machine learning runs, their inputs and outputs, and visualizing the results.

MLflow Tracking runs are executions of some data science code. Each run records the following information:

*   *Code version*: Git commit hash used for the run.
    
*   *Start and end time*: The start and end time of the run.
    
*   *Source*: The name of the file to launch the run, or the project name and entry point for the run if running from an MLflow Project.
    
*   *Parameters*: `Key-value` input parameters of your choice. Both keys and values are strings.
    
*   *Metrics*: `Key-value` metrics, where the value is numeric. MLflow records and lets you visualize the metric’s full history.
    
*   *Artifacts*: Output files in any format. For example, you can record images (for example, PNGs), models (for example, a pickled `scikit-learn` model), and data files (for example, a Parquet file) as artifacts.
    

MLflow Tracking is not a complete data versioning solution since it doesn’t support features such as data lineage (recording data inputs and which data was used to create a new data item), deduplication, and more. However, it enables logging and indexing the data outputs of every run along with the source code, parameters, and some execution details. MLflow can be manually integrated with other tools like DVC to track data and experiments.

MLflow’s advantage is tracking the data outputs with additional metadata about the code and parameters and visualizing or comparing them in a graphical UI. However, this does not come for free. The user code needs to be instrumented with MLflow tracking code.

[Example 4-3](#ch04.xhtml#mlflow-tracking) demonstrates a partial code snippet that tracks a run using the MLflow API. First, the command line arguments are parsed manually and the input data is passed as a string URL, just like any other parameter. Then, the loading and transformation of the data are done manually.

After the logic (data preparation, training, and so on) is executed, the user logs the tags, input parameters, output metrics, and data artifacts (dataset and model) using the MLflow log commands.

##### Example 4-3. MLflow tracking code example

```
if __name__ == "__main__":
    # parse the input parameters
    parser = argparse.ArgumentParser()
    parser.add_argument("--data", help="input data path", type=str)
    parser.add_argument('--dropout',  type=float, default=0.0, help='dropout ratio')
    parser.add_argument("--lr", type=float, default=0.001, help='learning rate')
    args = parser.parse_args()

    # Read the csv file
    try:
        data = pd.read_csv(args.data)
    except Exception as e:
        raise ValueError(f"Unable to read the training CSV, {e}")

    # additional initialization code ...

    with mlflow.start_run():

        # training code ...

        # log experiment tags, parameters and result metrics
        mlflow.set_tag("framework", "sklearn")
        mlflow.log_param("dropout", args.dropout)
        mlflow.log_param("lr", args.lr)
        mlflow.log_metric("rmse", rmse)
        mlflow.log_metric("r2", r2)
        mlflow.log_metric("mae", mae)

        # log data and model artifacts
        mlflow.log_artifacts(out_data_path, "output_data")
        mlflow.sklearn.log_model(model, "model",
                                 registered_model_name="ElasticnetWineModel")
```

MLflow sends the run information to the tracking server and stores the data elements in local files or remote objects (for example, in S3). The run information can be viewed or compared in the MLflow user interface (see [Figure 4-6](#ch04.xhtml#mlflow-run-ui)).

![](assets/mlflow-run-ui.png)

MLflow does not manage or version data objects. Run is the primary element and you cannot directly access or search data objects and artifacts. In addition, there is no lineage tracking, which means there is no tracking of which data objects were used to produce a new data object or artifact. When you run a pipeline, you cannot see the artifacts from the different steps in one place or chain output from one stage to the input of the next step.

With MLflow, the storage capacity can become significant since every run saves the outputs in a new file directory, even when nothing has changed. There is no data deduplication like in the other frameworks.

In summary, MLflow tracking is an excellent tool for tracking and comparing ML experiment results in a development environment. In addition, MLflow is easy to install and use. However, it is not a data tracking or versioning system and may require significant storage capacity. Furthermore, MLflow requires developers to add custom code and MLOps teams to add glue logic to fit into production [deployments](https://www.iguazio.com/glossary/model-deployment/) and CI/CD workflows.

**MLRun**

MLRun is an open-source MLOps orchestration framework with multiple sub-components to handle the complete ML lifecycle. Data objects are first-class citizens in MLRun and are well-integrated with the other components to provide seamless experience and automation.

Whereas most frameworks manage file data objects, MLRun supports a variety of data objects (data stores, items/files, datasets, streams, models, feature sets, feature vectors, charts, and more), each with unique metadata, actions, and viewers.

Every object in MLRun has a type, unique version ID, tags (named versions like development, production, and so on.), user-defined labels (for grouping and searching across objects), relations to other objects, and is a project member. For example, a run object has links to the source and output data objects and to function (code) objects, forming a graph of relations.

[Figure 4-7](#ch04.xhtml#mlrun-job-screen) demonstrates the run screen with information tabs for general and code attributes, data input objects, data/artifact output objects, result metrics, auto-collected logs, and so on. Users can view the information from different perspectives. For example, look at all the datasets in the project (regardless of which run generated them).

![](assets/mlrun-describe-job.png)

MLRun data objects and artifacts carry detailed metadata, including information on how they were produced (by who, when, code, framework, and so on), which data sources were used to create them, and type-specific attributes such as schema, statistics, preview, and more. The metadata is auto-generated, which provides better observability and eliminates the need for additional glue logic.

MLRun provides an extensive API/SDK for tracking and searching across data and experiments. However, the real power is that it can deliver most of the features and automation without requiring additional coding.

Take [Example 4-4](#ch04.xhtml#mlrun-tracking). It accepts input data and parameters and generates output data and results. Note that, unlike the previous examples, the code doesn’t include argument parsing, data loading, conversion, logging, and so on.

##### Example 4-4. MLRun code example

```
def data_preparation(dataset: pd.DataFrame, test_size=0.2):
    # preform processing on the dataset
    dataset = clean_df(dataset).dropna(how="any", axis="rows")
    dataset = dataset.drop(columns=["key", "pickup_datetime"])
    train, test = train_test_split(dataset, test_size=test_size)
    return train, test, "fare_amount"
```

When executing the function and specifying the input data object URL or path (a file, a remote object, or a complex type), it is automatically loaded into the function. For example, using AWS *boto* drivers to access S3 objects or *BigQuery* drivers to access a BigQuery table. Then, the data is converted to the accepting format (DataFrame) and injected into the user code.

MLRun can auto-detect the returned value type (for example, `train` and `test` are of type DataFrame) and store it in the best form, along with auto-generated metadata, links to the job details and data input objects, and versioning information. If the data repeats itself, it is deduplicated and stored only once to preserve storage space.

Data objects have type-specific visualized in the UI and client SDK regardless of how and where they are stored, For example, tabular formats with table metadata (schema, stats, and more) for datasets or interactive graphics for chart objects. See [Figure 4-8](#ch04.xhtml#mlrun-dataset-artifact) and [Figure 4-9](#ch04.xhtml#mlrun-artifact-sdk).

![](assets/mlrun-dataset-artifact.png)![](assets/mlrun-artifact-sdk.png)

In summary, MLRun is a complete MLOps orchestration framework with a significant focus on data management, movement, versioning, and automation. In addition, MLRun has a rich object model that covers different types of data and execution objects (functions, runs, workflows, and more), how they are related, and how they are used. MLRun focuses on abstraction and automation to reduce development and deployment efforts. However, MLRun is not a general data management and versioning solution, and its value is maximized when used in the context of MLOps.

**Other Frameworks**

Some tools, such as [Delta Lake](https://delta.io/) and [LakeFS](https://github.com/treeverse/lakeFS), handle data lake versioning. However, those tools are not focused on the ML lifecycle and may require integration to make them useful for MLOps.

Cloud vendors provide solutions that are usually tightly bound to their internal services. For example, see [Amazon SageMaker ML Lineage Tracking](https://docs.aws.amazon.com/sagemaker/latest/dg/lineage-tracking.xhtml) and [Azure ML Datasets](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-data-assets).

# Data Preparation and Analysis at Scale

Data processing is used extensively across the different data, ML, and application pipelines. When working with production data, there is a need to support more extensive scale and performance and, in some cases, handle data as it arrives in real time.

Practices that work during interactive development, for example, storing the data in a CSV file and reading it into the notebook, don’t work with gigabytes or terabytes of data. They require distributed or parallel data processing approaches.

The general architecture for distributed data processing is the same, with differences in how data is distributed and collected and which APIs they use. For example, the data is partitioned across multiple computer nodes, the processing requests or queries arrive at one or more nodes for local processing, and the results are collected and merged for a single answer. In addition, complex queries may shuffle data between nodes or execute multiple processing and movement steps.

[Figure 4-10](#ch04.xhtml#map-reduce-example) demonstrates how distributed data processing works using the map-reduce approach for counting words in a document.

![](assets/map-reduce-example.png)

**Structured and Unstructured Data Transformations**

Data can be *structured*, meaning it conforms to a specific format or structure and often has a predefined schema or data model. Structured data can be a database table or files with a structured layout (for example, csv, excel, json, ml, parquet). However, most of the world’s data is *unstructured*, is usually more complex, and is more difficult to process than structured data. This includes free text, logs, web pages, images, video, and audio.

Here are some examples of analytic transformations that can be performed on structured data:

1.  *Filtering*: Selecting a subset of the data based on certain criteria, such as a specific date range or specific values in a column.
    
2.  *Sorting*: Ordering the data based on one or more columns, such as sorting by date or by a specific value.
    
3.  *Grouping*: Organizing the data into groups based on one or more columns, such as grouping by product category or by city.
    
4.  *Aggregation*: Calculating summary statistics, such as count, sum, average, maximum, and standard deviation, for one or more columns.
    
5.  *Joining*: Combining data from multiple tables or datasets based on common columns, such as joining a table of sales data with a table of customer data.
    
6.  *Mapping*: Mapping values from one or more columns to new column values using user-defined operations or code. Stateful mapping can calculate new values based on original values and accumulated states from older entries (for example, time passed from the last login).
    
7.  *Time series analysis*: Analyzing or aggregating data over time, such as identifying trends, patterns, or anomalies.
    

The following technique examples can be used to process unstructured data or convert it to structured data:

1.  *Text mining*: Using natural language processing (NLP) techniques to extract meaning and insights from text data. Text mining can extract information such as sentiment, entities, and topics from text data.
    
2.  *Computer vision*: Using image and video processing techniques to extract information from visual data. Computer vision can extract information such as object recognition, facial recognition, and image classification.
    
3.  *Audio and speech recognition*: Using speech-to-text and audio processing techniques to extract meaning and insights from audio data. Audio and speech recognition can extract information such as speech-to-text, sentiment, and speaker identification.
    
4.  *Data extraction*: Using techniques such as web scraping and data extraction to extract structured data from unstructured data sources.
    

Various machine-learning methods can be used to transform raw data into more meaningful data, for example:

1.  *Clustering*: Grouping similar data points based on certain characteristics, such as customers with similar purchasing habits.
    
2.  *Dimensionality reduction*: Reducing the number of features in a dataset to make it easier to analyze or visualize.
    
3.  *Regression and classification*: Predicting a class or a value based on other data features.
    
4.  *Imputing*: Determine the expected value based on other data points in case of missing data.
    

**Distributed Data Processing Architectures**

Data-processing architectures can be broken into three main categories:

1.  *Interactive*: A request or an update arrives, is processed, and a response is returned immediately. For example, SQL and NoSQL databases, data warehouses, key/value stores, graph databases, time-series databases, cloud services, and more.
    
2.  *Batch*: A job is started on a request or a scheduled time, data is fetched and processed, and the results are written to the target storage after completion. Batch jobs usually take longer to process. Example frameworks for batch data processing include [Hadoop](https://hadoop.apache.org/), [Spark](https://spark.apache.org/), [Dask](https://www.dask.org/), and more.
    
3.  *Streaming*: Continuous processing of incoming requests or chunks of data and writing the results in real time to a target storage or message queue.
    

Batch processing is usually more efficient for processing large data quantities. However, interactive and stream data processing deliver faster responses with shorter delays. In addition, building data stream processing pipelines is usually more complex than batch jobs.

Some frameworks like Spark may support different processing methods (interactive, batch, streaming), but they will usually be more optimal only in one of the processing methods.

## Interactive Data Processing

Interactive systems are expected to respond immediately, so the requesting client or interactive dashboard will not need to wait. Furthermore, production services may depend on the reliability and robustness of the results. This is why interactive systems have simple APIs with limited data operations. In some cases, interactive systems provide mechanisms to define custom logic through stored procedures and UDFs (user-defined functions).

The main difference between the different types of interactive data systems is how they index and store data to minimize response retrieval time. For example, NoSQL, in-memory, and key/value stores are optimized for retrieval by an index key (such as a *user id*, *product id*, and so on). The data is divided by the key (or a crypto hash or the key) and stored in different nodes. Then, when a request arrives, it is passed to the specific node, which manages the data for that key (user, product, and so on) and can quickly calculate and retrieve the answer. On the other hand, complex or cross-key calculations require coordination between all the nodes and take much longer.

On the other hand, analytical databases and data warehouses are designed to traverse many records with different index key values. They organize the data in columns (by field) and use various columnar compression technologies and filtering and hinting tricks (like bloom filtering) to skip data blocks.

Other systems like time-series or graph databases have more advanced data layouts and search strategies that combine multi-dimensional indexes and columnar compression. For example, accessing the time-series metric object by the metric key (name) and using columnar compression technologies to scan or aggregate the individual values (by time).

Many interactive systems use the SQL language or SQL-like semantics to process data.

Some notable data systems examples by sub-category are listed in [Table 4-1](#ch04.xhtml#data-systems-examples):

Table 4-1. Data systems examples by sub-category

  
| Category || Description |
| --- | --- |
| Relational || Store structured data, access through SQL command. Examples include [MySQL](https://www.mysql.com/), [PostgreSQL](https://www.postgresql.org/), [Oracle](https://www.oracle.com/), and [Microsoft SQL Server](https://www.microsoft.com/en-us/sql-server). |
| --- | --- |
| NoSQL || Examples include [MongoDB](https://www.mongodb.com/), [Cassandra](https://cassandra.apache.org/_/index.xhtml), [Redis](https://redis.io/), [Elasticsearch](https://www.elastic.co/), [AWS DynamoDB](https://aws.amazon.com/dynamodb/), and [Google BigTable](https://cloud.google.com/bigtable). |
| Time-series || Store and query time-series data. Examples include [InfluxDB](https://www.influxdata.com/), [Prometheus](https://prometheus.io/), and [TimescaleDB](https://www.timescale.com/). |
| Graph || Store and query data in a graph format. Examples include [Neo4j](https://neo4j.com/) and [Titan](https://titan.thinkaurelius.com/). |

Analytical systems usually traverse and process larger datasets. As a result, they support more extensive transformations (filtering, grouping, joining, aggregating, mapping, and so on) and user-defined functions. In addition, some can process and aggregate data from other databases or data stored in files. For example, solutions like Spark SQL or Presto DB have connectors to many data sources and can process queries that span many datasets and are stored in different systems.

One of the most popular distributed SQL-based analytical engines is Presto DB and its follow-on project, [Trino](https://trino.io/). Presto was initially developed by Facebook and contributed to open-source. Later, it was forked into projects like the Trino and commercial products like the [Amazon Athena](https://aws.amazon.com/athena/) cloud service. Trino has a long list of [data connectors](https://trino.io/docs/current/connector.xhtml)

![](assets/presto-db-arch.png)

[Figure 4-11](#ch04.xhtml#presto-db-arch) illustrates Presto and Trino architectures. Queries arrive through HTTP requests, are parsed, and are broken by the planner and the scheduler into smaller tasks that are processed and merged by the individual workers.

## Batch Data Processing

Batch data processing is used when there is a need to process large amounts of data and run a sequence of data transformations, and the processing time is less of a concern. In batch processing, the data is read and broken into chunks passed to multiple workers for processing. Once the result is ready, it is written to the target system.

Batch processing is often used to process large amounts of historical data and generate the dataset for training ML models.

One of the best known batch data processing frameworks was Apache Hadoop, an open-source software framework for distributed storage and large-scale processing of data-intensive tasks. Hadoop was initially developed by Yahoo! engineers and was based on the *MapReduce* programming model. The MapReduce model consists of two main functions: `Map` and `Reduce`. The `Map` function takes an input dataset and processes it into a set of intermediate key-value pairs, which are then grouped by key and processed by the `Reduce` function to produce the final output.

Hadoop has since been replaced with more modern and cloud-native architectures based on cloud object storage, containerized infrastructure, and computation frameworks such as [Spark](https://spark.apache.org/), [Flink](https://flink.apache.org/), [Beam](https://beam.apache.org/), [Dask](https://www.dask.org/), and others.

An everyday use for batch processing is found in ETL (Extract, Transform, Load) tasks. ETL refers to extracting data from multiple sources, transforming it, and loading it into a target database, data warehouse, or data lake. ETL is a crucial step in the data integration process, as it allows organizations to extract, clean, and transform data from multiple sources into a single, centralized repository.

Batch-processing pipelines may be complex and have multiple steps and dependencies. [Apache Airflow](https://airflow.apache.org/) is one of the most popular open-source frameworks for authoring, scheduling, and monitoring batch data pipelines.

Airflow was initially developed by Airbnb and is now maintained by the Apache Software Foundation. It provides a simple and easy-to-use interface for defining workflows as directed acyclic graphs (DAGs) of tasks, where each task represents an individual processing step. The tasks can be written in Python and run in various environments, including locally, over Kubernetes, or in the cloud.

Airflow also provides a web-based user interface (see [Figure 4-12](#ch04.xhtml#airflow-graph)) for managing and [monitoring](https://www.iguazio.com/glossary/model-monitoring/) workflows, including the ability to see the status of each task, retry failed tasks, and manually trigger or schedule tasks. It also includes features for managing and organizing workflows, such as defining dependencies between tasks and setting up task retry logic.

![](assets/airflow-graph.png)

[Example 4-5](#ch04.xhtml#airflow-pipeline-code) is an example of a Python code that can be used to create a DAG in Apache Airflow that reads data from a CSV file, processes it, and writes it to a destination.

##### Example 4-5. Airflow data pipeline code example

```
import csv
from airflow import DAG
from airflow.operators.python_operator import PythonOperator
from datetime import datetime, timedelta

def process_data(**kwargs):
    ti = kwargs['ti']
    input_file = ti.xcom_pull(task_ids='read_file')
    processed_data = do_data_processing(input_file)
    return processed_data

def do_data_processing(input_file):
    # Placeholder function that performs data processing
    processed_data = input_file
    return processed_data

def read_csv_file(file_path):
    with open(file_path, 'r') as file:
        reader = csv.reader(file)
        return list(reader)

def write_csv_file(file_path, data):
    with open(file_path, 'w') as file:
        writer = csv.writer(file)
        writer.writerows(data)

default_args = {
    'owner': 'airflow',
    'depends_on_past': False,
    'start_date': datetime(2021, 1, 1),
    'email_on_failure': False,
    'email_on_retry': False,
    'retries': 1,
    'retry_delay': timedelta(minutes=5),
}

dag = DAG(
    'data_processing_dag',
    default_args=default_args,
    description='A DAG that reads data from a CSV file, processes it'
                ', and writes it to a destination',
    schedule_interval=timedelta(hours=1),
)

read_file = PythonOperator(
    task_id='read_file',
    python_callable=lambda: read_csv_file('/path/to/input_file.csv'),
    xcom_push=True,
    dag=dag,
)

process_data = PythonOperator(
    task_id='process_data',
    python_callable=process_data,
    provide_context=True,
    dag=dag,
)

write_file = PythonOperator(
    task_id='write_file',
    python_callable=lambda: write_csv_file('/path/to/output_file.csv',
                                           ti.xcom_pull(task_ids='process_data')),
    provide_context=True,
    dag=dag,
)

read_file >> process_data >> write_file
```

There are several cloud-based batch data pipeline services such as [AWS Glue](https://aws.amazon.com/glue/), [Google Cloud Composer](https://cloud.google.com/composer) (based on Airflow), and [Azure Data Factory](https://azure.microsoft.com/en-us/products/data-factory).

One of the disadvantages of Hadoop or other batch pipelines is the need to read data from disk, process it, and write it again to disk at every step. However, frameworks such as [Spark](https://spark.apache.org/) and [Dask](https://www.dask.org/) know how to compile the processing pipeline into an optimal graph where tasks are done in memory where possible, and this way minimize the IO to disk and maximize performance.

[Example 4-6](#ch04.xhtml#spark-pipeline-code) demonstrates a Spark code that reads a CSV file, processes the data, and writes the result into a target file.

##### Example 4-6. PySpark data pipeline code example

```
from pyspark.sql import SparkSession

# Create a Spark session
spark = SparkSession.builder.appName("SimpleBatchProcessing").getOrCreate()

# Load a CSV file into a Spark DataFrame
df = spark.read.csv("/path/to/input_file.csv", header=True, inferSchema=True)

# Perform some data processing on the DataFrame
processed_df = df.groupBy("column_name").agg({"column_name": "mean"})

# Write the processed DataFrame to a new CSV file
processed_df.write.csv("/path/to/output_file.csv", header=True)

# Stop the Spark session
spark.stop()
```

The same example is shown in [Example 4-7](#ch04.xhtml#dask-pipeline-code) implemented using Dask. The advantage of Dask is that the operations are very similar to Python pandas, which is a tremendous advantage for data scientists. However, Spark is usually more scalable and robust.

##### Example 4-7. Dask data pipeline code example

```
import dask.dataframe as dd

# Load a CSV file into a Dask DataFrame
df = dd.read_csv('/path/to/input_file.csv')

# Perform some data processing on the DataFrame
processed_df = df.groupby('column_name').column_name.mean().compute()

# Write the processed DataFrame to a new CSV file
processed_df.to_csv('/path/to/output_file.csv', index=False)
```

You can see that the Spark and Dask examples are much simpler compared to the Airflow ones. However, Airflow can be more suitable for managing and tracing long and complex jobs.

## Stream Processing

*Stream processing* enables scalable, fault-tolerant, and real-time data processing. It is often used in applications that process large amounts of data in real-time, such as real-time analytics, fraud detection, or recommendations.

In stream processing, data and incoming events are pushed into a stream (queue) and read by one or more workers. The workers process the data sequentially, make transformations, aggregate results, and write the results into a database or an output stream. Unlike traditional message queues, stream processing occurs in order. For example, assume the stream contains two events, one for customer login and another for customer logout. Not processing them in order can lead to a broken state. Another example is a money deposit operation occurrence, followed by a withdrawal. The withdrawal operation may be declined if operations are processed in the wrong order.

Streams are designed to scale. They are broken into partitions, and each partition handles a specific set of data objects, so it will not violate the order. For example, a user activity stream is partitioned by the user ID so that a specific user’s activities will always be stored in the same partition and processed by the same worker.

Streams ([Kafka](https://kafka.apache.org/), [AWS Kinesis](https://aws.amazon.com/kinesis/), and others) are different than message queues ([RabbitMQ](https://www.rabbitmq.com/), [AMQP](https://www.amqp.org/), [AWS SQS](https://aws.amazon.com/sqs/), [Google PubSub](https://cloud.google.com/pubsub), and so on). Message queues do not guarantee message ordering. However, they guarantee reliable delivery of messages, while the client manages the reliability in the case of streams. Furthermore, they are much faster due to the more straightforward logic and parallelism offered with streams.

[Figure 4-13](#ch04.xhtml#streaming-application) illustrates a streaming application in which clients publish data that is distributed between the individual partitions (based on a hash of the partition key). One worker is reading from each partition and processing the data. The worker can use a database to store the state on known intervals (checkpoints), so the state can be recovered in case of a failure, or the worker can free unused memory. Finally, the results can be written into a target database or an output stream.

![](assets/streaming-application.png)

Streams provide “at-least-once semantics”. Therefore, the same message may appear multiple times. A way to provide “exactly once” semantics (the same message is processed only once) is with the help of checkpoints. Streams are processed in order, and the state can be persisted after every micro-batch. In the case of a failure, the worker can restore the last checkpoint data (state), process the events from that point forward, and ignore older events.

**Stream Processing Frameworks**

Doing real-time analytics on real-time streams differs from doing it in batch or SQL. With streams, the workers can only go over the data once, in sequential order, and see a portion of the data (in the same partition). This is why real-time analytics frameworks such as [Spark Streaming](https://spark.apache.org/docs/latest/streaming-programming-guide.xhtml), [Apache Flink](https://flink.apache.org/), Apache Beam, [Apache Nifi](https://nifi.apache.org/), and others, focus on stream processing and implement the standard analytic and statistic methods in a stream-optimized way.

A typical scenario in stream processing is to aggregate values over time. For example, examining the total value of customer transactions in the last hour to detect fraud. It is not feasible to calculate the total for every new event with stream processing. It will take a considerable amount of time and memory. Instead, the values are grouped into windowed buckets. For example, six buckets or more, each holding the total per 10 minutes. The process only sums the values of the last six buckets and drops the oldest bucket every 10 minutes. [Figure 4-14](#ch04.xhtml#sliding-time-windows) illustrates overlapping sliding windows with a one-minute window duration and 30-second window periods.

![](assets/sliding-time-windows.png)

[Example 4-8](#ch04.xhtml#apache-beam-code) shows the Apache Beam code for defining such a window.

##### Example 4-8. Defining the sliding window using Apache Beam

```
from apache_beam import window
sliding_windowed_items = (
    items | 'window' >> beam.WindowInto(window.SlidingWindows(60, 30)))
```

Coding with stream processing frameworks requires advanced data engineering knowledge. This is why many users avoid real-time data, even though it can provide much better business value and more accurate model scoring results. Feature Stores come to the rescue, as they can automatically generate the batch and the streaming pipeline from the same higher-level data processing logic.

# Introducing Feature Stores

*Feature stores* are a factory and central repository for machine learning features. Feature stores handle the collection of raw data from various sources, the transformation pipeline, storage, cataloging, versioning, security, [serving](https://www.iguazio.com/glossary/model-serving-pipeline/), and monitoring. They automate many processes described in this chapter, while accelerating production time and reducing engineering efforts. Feature stores form a shared catalog of production-ready features, enable collaboration and sharing between teams, and accelerate the innovation and delivery of new AI applications.

The first feature store implementations came from large service providers like Uber, Twitter, and Spotify. In those providers, AI is core to the business, and feature stores helped them accelerate the development and deployment of new AI applications and improve collaboration and reuse. Today there are multiple commercial and open-source implementations to choose from.

Advanced feature stores provide the following capabilities:

*   *Data connectivity*: Glueless integration with multiple offline (data lakes, data warehouses, databases, and so one) and online (streams, message queues, APIs, managed services, and so on) sources.
    
*   *Offline and online transformation*: Some feature stores offer capabilities to automatically build and manage the batch and streaming pipelines from higher-level logic.
    
*   *Storage*: Storing the generated features in an offline store (such as an object store) and an online store (usually a key/value database).
    
*   *Metadata management*: Auto-generating, storing, and managing all feature metadata, including lineage, schemas, statistics, labels, and more.
    
*   *Versioning*: Managing multiple versions of each feature and the process of promoting features from development to production and integrating with CI/CD.
    
*   *Generating and managing feature vectors*: Correctly joining multiple features into a single dataset for use in training or serving applications.
    
*   *Central cataloging*: Providing centralized access to generate, label, or search features.
    
*   *Security and governance*: Controlling the access to features and raw data and to logging feature access.
    
*   *Easy-to-use UI and SDK*: Simple access through APIs and a user interface to abstract the underline complexity, visualize features, and make it usable by data scientists.
    
*   *Monitoring and high availability*: Monitoring the assets and data processing tasks automatically while reliably recovering from failures.
    
*   *Feature validation and analysis*: Executing various data processing tasks automatically or as initiated by the user, to validate feature correctness or to generate a deep analysis of features, correlation, and so on.
    

You should conduct a thorough comparison before choosing a feature store. For example, many features have very partial functionality, may focus on cataloging features, and lack automated transformations, data management at scale, or real-time functionality. These capabilities provide the most significant value in accelerating time to production.

**Feature Store Architecture and Usage**

[Figure 4-15](#ch04.xhtml#feature-store-usage) illustrates a feature store’s general architecture and usage. Raw data is ingested and transformed into features and features are cataloged and served to different applications (training, serving, monitoring). APIs and UI allow data scientists, data engineers, and ML engineers to update, search, monitor, and use features.

![](assets/feature-store-usage.png)

The core components of a feature store are:

*   *Transformation layer*: Converts raw offline or online data into features and stores them in both an online (key/value) and offline (object) store.
    
*   *Storage layer*: Stores multiple versions of a feature in feature tables (feature sets) and manages the data lifecycle (create, append, delete, monitor, and secure the data). The data layer stores each feature in two forms: offline for training and analysis and online for serving and monitoring.
    
*   *Feature retrieval*: Accepts requests for multiple features (feature vectors) and other properties (such as time ranges, event data, and so on) and produces an offline data snapshot for training or a real-time vector for serving.
    
*   *Metadata management and cataloging*: Stores the feature definition, metadata, labels, and relations.
    

## Ingestion and Transformation Service

This chapter so far has discussed the complexities of implementing large-scale processing for batch and real-time data, data versioning, and metadata management. Feature stores aim at reducing that complexity through abstraction and automation. With modern feature stores, data pipelines are described using high-level transformation logic. This logic is converted to the underline processing engine semantics and deployed as a continuous and production-grade service, saving significant engineering efforts.

Pipeline implementation is different for local development (using packages like Pandas), large-scale offline data (using batch processing), and real-time data (using stream processing). The advantage of a feature store that supports automated transformations is that it uses one definition for all three deployment modes and eliminates the reengineering involved in porting data pipelines from one method to another. In some feature stores, the data pipeline technology will be determined by the data sources, whether offline (data lakes, data warehouses, databases, and so on) or online (streams, message queues, APIs, managed services, and others).

Feature stores implement the data ingestion and transformation on groups of features (called feature sets or feature groups) that originate from the same source. For example, all the features extracted from the credit card transaction log. Feature sets take data from offline or online sources, build a list of features through a set of transformations, and store the resulting features along with the associated metadata and statistics.

[Figure 4-16](#ch04.xhtml#feature-set-graph) illustrates the transformation service (feature set). Once the data is ingested from the source, it passes through a graph (DAG) of transformations, and the resulting features are written into the offline and online stores.

![](assets/feature-set-graph.png)

Examples of transformation (by data type):

*   *Structured*: Filter, group, join, aggregate, OneHot encoding, map, extract, and classify.
    
*   *Textual*: Extract, parse, disassemble, detect entities, sentiments, and embeddings.
    
*   *Visual (images and videos)*: Frame, resize, detect objects, crop, recolor, rotate, map, and classify.
    

The generated transformation service should be production-grade and support auto-scaling, high availability, live upgrades, and more. In addition, it should support continuous data ingestion and processing. For example, new data may arrive continuously (for real-time) or in scheduled intervals (for offline). Therefore, serverless function technologies are an excellent fit.

## Feature Storage

The features are usually stored in two forms, offline storage for training and analytics applications and online storage for real-time serving and monitoring applications. See [Figure 4-17](#ch04.xhtml#feature-storage).

![](assets/feature-storage.png)

The offline store holds all the historical data and often uses data lakes, object storage, or data warehouse technologies. For example, a common choice is to use compressed parquet files stored in object storage like AWS S3.

The online store holds the most recent data and often uses NoSQL or key/value stores like Redis, AWS DynamoDB, Google BigTable, and others. The online store needs to support reading features in milliseconds.

## Feature Retrieval (for Training and Serving)

Training, serving, and analysis applications require multiple features from multiple datasets and sources. In contrast, feature stores organize features in groups (called *feature sets*) based on their origin and entity (primary key such as a *user id*, *product id*, and so on).

Retrieving multiple features from different sources, times, and with different indexes can be a complex analytics task. Feature stores automatically determine the parameters required for the `JOIN` query based on the features metadata, entity names, and user request data. In addition, when the datasets are transactional (records are marked with a timestamp), the join operation needs to take into account time correctness and *time traveling* to only return the values known at the time of the event (also referred to as *as of join* analytics operation).

Offline feature sets can be generated through SQL queries generated by the feature store. However, with real-time serving applications that need to respond in milliseconds, this creates considerable overhead, and other real-time methods are used. In addition, time-based features (such as the number of requests in the last hour) cannot be pre-calculated and require special handling to generate an accurate result (for example, by combining pre-calculated time windowed data and ad-hoc last-mile calculations).

[Figure 4-18](#ch04.xhtml#feature-serving) illustrates the feature retrieval flow with two separate engines, one for offline retrieval and the other for real-time retrieval. Note that in the case of offline, the dataset is snapshotted or preserved in a new dataset to allow data lineage tracking and explainability.

![](assets/feature-serving.png)

The `get_offline_features` request can accept event data to base the query on, a valid time range (for example, if we want to train the model based on data from the last month), and which features and columns should return (for example, whether to include the index, time, or label columns). Then, it initiates a local or serverless analytics job that computes the results and returns the features vector dataset.

In real-time retrieval, the system initializes the retrieval service (configuring a local or remote real-time analytics function once to save time on requests). Then, user requests are pushed with the entity keys (taken from the event data) and accept a result vector. In addition, some feature stores allow real-time imputing (replacing missing or NaN data with statistical feature values taken from the feature metadata).

# Feature Stores Solutions and Usage Example

Feature stores started as internal platforms in leading cloud services providers (such as Uber, Spotify, Twitter, and others). But now, many open-source and commercial feature store solutions are in the market. However, as in every important new technology space, there are many functionality differences between those solutions; you need to be aware before choosing the right one.

The most notable and essential difference is if the feature store platform manages the data (transformation) pipeline for you and whether it supports both offline and real-time (streaming) pipelines. As you’ve read in this chapter, building and managing a scalable data pipeline is the major challenge. If you are forced to do it manually, it significantly undermines the value of a feature store.

[Table 4-2](#ch04.xhtml#feature-store-compare) compares the leading feature store solutions:

TBD verify the table content !!!

Table 4-2. Data systems examples by sub-category

  
| Category || [Feast](https://feast.dev/) || [Tecton](https://www.tecton.ai/) || [MLRun](https://docs.mlrun.org/en/stable/feature-store/feature-store.xhtml) || [SageMaker](https://aws.amazon.com/sagemaker/feature-store/) || [Vertex AI](https://cloud.google.com/vertex-ai/docs/featurestore) || [DataBricks](https://docs.databricks.com/machine-learning/feature-store/index.xhtml) || [HopsWorks](https://www.hopsworks.ai/) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Open-source || Yes || No || Yes || No || No || No || Yes |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Managed option || No || major clouds || cloud + on-prem || on AWS || on GCP || major clouds || cloud + on-prem |
| Offline pipelines || No || Yes || Yes || No || No || No || Yes |
| Real-time pipelines || No || Yes || Yes || No || No || No || No |
| Feature retrieval || Yes || Yes || Yes || Yes || Yes || Yes || Yes |
| Engines || Spark || Spark || Python, Dask, Spark, Nuclio || None || Spark || Spark || Spark, Flink |
| Feature analytics || No || Partial || Yes || No || No || No || Yes |
| Versioning & Lineage || No || Yes || Yes || No || No || No || Yes |
| Features Security || No || Yes || Yes || Yes || No || No || No |
| Monitoring || No || Yes || Yes || No || No || No || Yes |
| Glueless Training & Serving || No || No || Yes || No || No || No || Yes |

The following sections will demonstrate how feature stores are used with the two leading open-source frameworks: Feast and MLRun. Note that MLRun is more fully featured and provides offline and online transformation services (based on MLRun’s serverless engines) along with many other unique features.

## Using Feast Feature Store

Feast does not provide a transformation service. Data should be prepared upfront and stored in a supported source (like S3, GCS, BigQuery, or other). Feast registers the source dataset and its metadata (schema, entity, and so on) in a FeatureView object. See [Example 4-9](#ch04.xhtml#feast-feature-view).

##### Example 4-9. Defining Feast FeatureView (source: Feast)

```
# Read data from parquet files. Parquet is convenient for local development mode. For
# production, you can use your favorite DWH, such as BigQuery. See Feast documentation
# for more info.
driver_hourly_stats = FileSource(
    name="driver_hourly_stats_source",
    path="/content/feature_repo/data/driver_stats.parquet",
    timestamp_field="event_timestamp",
    created_timestamp_column="created",
)

# Define an entity for the driver. You can think of entity as a primary key used to
# fetch features.
driver = Entity(name="driver", join_keys=["driver_id"])

# Our parquet files contain sample data that includes a driver_id column, timestamps and
# three feature column. Here we define a Feature View that will allow us to serve this
# data to our model online.
driver_hourly_stats_view = FeatureView(
    name="driver_hourly_stats",
    entities=[driver],
    ttl=timedelta(days=1),
    schema=[
        Field(name="conv_rate", dtype=Float32),
        Field(name="acc_rate", dtype=Float32),
        Field(name="avg_daily_trips", dtype=Int64),
    ],
    online=True,
    source=driver_hourly_stats,
    tags={},
)
```

Feast does not provide an online transformation or ingestion service. Instead, the user needs to run a *materialization* task to copy the offline features into the real-time store (database). Unfortunately, this also means that the data stored in the online store is inaccurate between materializations, and running materialization too frequently can result in significant computation overhead.

Running the materialization task via the SDK:

```
store = FeatureStore(repo_path=".")
store.materialize_incremental(datetime.now())
```

The project may contain one or more feature views, and each is defined and materialized independently. Features can be retrieved from one or more feature views (will initiate a JOIN operation).

To retrieve offline features (directly from the offline source), use the `get_historical_features()` API call as shown in Example 4-11:

##### Example 4-10. Retrieve offline features with Feast (source: Feast)

```
# The entity dataframe is the dataframe we want to enrich with feature values
# see https://docs.feast.dev/getting-started/concepts/feature-retrieval for details
# for all entities in the offline store instead
entity_df = pd.DataFrame.from_dict(
    {
        # entity's join key -> entity values
        "driver_id": [1001, 1002, 1003],
        # "event_timestamp" (reserved key) -> timestamps
        "event_timestamp": [
            datetime(2021, 4, 12, 10, 59, 42),
            datetime(2021, 4, 12, 8, 12, 10),
            datetime(2021, 4, 12, 16, 40, 26),
        ],
        # (optional) label name -> label values. Feast does not process these
        "label_driver_reported_satisfaction": [1, 5, 3],
        # values we're using for an on-demand transformation
        "val_to_add": [1, 2, 3],
        "val_to_add_2": [10, 20, 30],
    }
)

store = FeatureStore(repo_path=".")

# retrieve offline features, feature names are specified with <view>:<feature-name>
training_df = store.get_historical_features(
    entity_df=entity_df,
    features=[
        "driver_hourly_stats:conv_rate",
        "driver_hourly_stats:acc_rate",
        "driver_hourly_stats:avg_daily_trips",
        "transformed_conv_rate:conv_rate_plus_val1",
        "transformed_conv_rate:conv_rate_plus_val2",
    ],
).to_df()

print("----- Example features -----\n")
print(training_df.head())
```

To retrieve online features (from the online store), we use the `get_online_features()` API call in the following way:

##### Example 4-11. Retrieve online features with Feast (source: Feast)

```
from pprint import pprint
from feast import FeatureStore

store = FeatureStore(repo_path=".")

feature_vector = store.get_online_features(
    features=[
        "driver_hourly_stats:acc_rate",
        "driver_hourly_stats:avg_daily_trips",
        "transformed_conv_rate:conv_rate_plus_val1",
        "transformed_conv_rate:conv_rate_plus_val2",
    ],
    entity_rows=[
        # {join_key: entity_value}
        {
            "driver_id": 1001,
            "val_to_add": 1000,
            "val_to_add_2": 2000,
        },
        {
            "driver_id": 1002,
            "val_to_add": 1001,
            "val_to_add_2": 2002,
        },
    ],
).to_dict()

pprint(feature_vector)

# results:
{'acc_rate': [0.86463862657547, 0.6959823369979858],
 'avg_daily_trips': [359, 311],
 'conv_rate_plus_val1': [1000.6638441681862, 1001.1511893719435],
 'conv_rate_plus_val2': [2000.6638441681862, 2002.1511893719435],
 'driver_id': [1001, 1002]}
```

## Using MLRun Feature Store

MLRun supports the registration of existing sources (like Feast) or the definition of a data pipeline for transforming source data into features. When defining the data pipeline (called a *graph*), MLRun provisions the selected data processing engine based on the abstract user definitions. MLRun supports a few processing engines, including local Python, Dask, Spark, and Nuclio (a real-time serverless engine).

In MLRun, by default, the pipeline writes into online and offline stores, so there is no need for separate materialization jobs, and the online and offline features are always in sync. In addition, MLRun can auto-detect the data schema, making it more straightforward and robust.

MLRun separates the definition of the feature set (a collection of features generated by the same pipeline) from the data source definitions. This way, you can use the same feature set in interactive development and in production. Just swap the source from a local file in development to a database or real-time Kafka stream in the production deployment.

[Example 4-12](#ch04.xhtml#mlrun-feature-set) shows an example of defining a feature set for processing credit card transactions to detect credit card fraud. The definition includes the entity, timestamp, and transformation graph using built-in operators and aggregations. Note that a user can also add their custom Python operators. See the [full example](https://docs.mlrun.org/en/stable/feature-store/end-to-end-demo/index.xhtml).

The data pipeline consists of the following:

*   *Extracting* the data components (hour, day of week).
    
*   *Mapping* the age values.
    
*   *One hot encoding* for the transaction category and the gender.
    
*   *Aggregating* the amount (avg, sum, count, max over 2/12/24 hour time windows).
    
*   *Aggregating* the transactions per category (over 14 days time windows).
    
*   *Writing* the results to offline (Parquet) and online (NoSQL) targets.
    

##### Example 4-12. Defining MLRun FeatureSet (source: MLRun)

```
import mlrun.feature_store as fs

# Define the credit transactions FeatureSet
transaction_set = fs.FeatureSet("transactions",
                                entities=[fs.Entity("source")],
                                timestamp_key='timestamp',
                                description="transactions feature set")

# Define and add value mapping
main_categories = ["es_transportation", "es_health", "es_otherservices",
       "es_food", "es_hotelservices", "es_barsandrestaurants",
       "es_tech", "es_sportsandtoys", "es_wellnessandbeauty",
       "es_hyper", "es_fashion", "es_home", "es_contents",
       "es_travel", "es_leisure"]

# One Hot Encode the newly defined mappings
one_hot_encoder_mapping = {'category': main_categories,
                           'gender': list(transactions_data.gender.unique())}

# Define the data pipeline (graph) steps
transaction_set.graph\
    .to(DateExtractor(parts = ['hour', 'day_of_week'], timestamp_col = 'timestamp'))\
    .to(MapValues(mapping={'age': {'U': '0'}}, with_original_features=True))\
    .to(OneHotEncoder(mapping=one_hot_encoder_mapping))


# Add aggregations for 2, 12, and 24 hour time windows
transaction_set.add_aggregation(name='amount',
                                column='amount',
                                operations=['avg','sum', 'count','max'],
                                windows=['2h', '12h', '24h'],
                                period='1h')


# Add the category aggregations over a 14 day window
for category in main_categories:
    transaction_set.add_aggregation(name=category,column=f'category_{category}',
                                    operations=['count'], windows=['14d'], period='1d')
```

The data pipeline can be visualized using `transaction_set.plot(rankdir="LR", with_targets=True)`, as seen in [Figure 4-19](#ch04.xhtml#mlrun-feature-pipeline).

![](assets/mlrun-feature-pipeline.png)

Once you have the feature set definition, you can test and debug it with the `preview()` method that runs the data pipeline locally and lets you view the results.

```
df = fs.preview(transaction_set, transactions_data)
df.head()
```

When the feature set definition is done, you can deploy it as a production job that runs on demand, on a given schedule, or as a real-time pipeline.

For running batch ingestion, use the `ingest()` method. For real-time ingestion, from HTTP or streams, use `deploy_ingestion_service()`, which starts a real-time Nuclio serverless pipeline. See [Example 4-13](#ch04.xhtml#mlrun-feature-ingest).

##### Example 4-13. Ingest data into MLRun FeatureSet (source: MLRun)

```
# Batch ingest the transactions dataset (from CSV file) through the defined pipeline
source = CSVSource("mycsv", path="measurements.csv")
fs.ingest(transaction_set, source=source)

# Deploy a real-time pipeline with HTTP API endpoint as the source
# MLRun support other real-time sources like Kafka, Kinesis, etc.
source = HTTPSource()
fs.deploy_ingestion_service(transaction_set, source)
```

You can watch the feature sets, their metadata, and statistics in the MLRun feature store UI. See [Figure 4-20](#ch04.xhtml#mlrun-ui-feature-set).

![](assets/mlrun-ui-feature-set.png)

The feature retrieval in MLRun is done using the *Feature Vector* object. Feature vectors hold the definitions of the requested features and additional parameters. In addition, it also stores calculated values such as the features metadata, statistics, and so on, which can be helpful in the training, serving, or monitoring tasks. For example, feature statistics are used for automated value imputing in the case of missing or NaN feature values and for model drift monitoring in the serving application.

Feature vectors can be created, updated, and viewed in MLRun’s UI.

The user first defines the feature vector, then she or he can use it to obtain offline or online features. See how to retrieve offline features and use the `get_offline_features()` method in [Example 4-14](#ch04.xhtml#mlrun-get-offline).

##### Example 4-14. Get offline features from MLRun (source: MLRun)

```
# Define the list of features you will be using (<feature-set>.<feature>)
features = ['transactions.amount_max_2h',
            'transactions.amount_sum_2h',
            'transactions.amount_count_2h',
            'transactions.amount_avg_2h',
            'transactions.amount_max_12h']

# Import MLRun's Feature Store
import mlrun.feature_store as fstore

# Define the feature vector name for future reference
fv_name = 'transactions-fraud'

# Define the feature vector using our Feature Store
transactions_fv = fs.FeatureVector(fv_name, features,
                                   label_feature="labels.label",
                                   description='Predicting a fraudulent transaction')

# Save the feature vector definition in the Feature Store
transactions_fv.save()

# Get offline feature vector as dataframe and save the dataset to a parquet file
train_dataset = fstore.get_offline_features(transactions_fv, target=ParquetTarget())

# Preview the dataset
train_dataset.to_dataframe().tail(5)
```

To get real-time features, you first need to define a service (which initializes the real-time retrieval pipeline), followed by `.get()` methods to request feature values in real-time. The separation between the service creation (one-time initialization) and individual requests ensures lower request latencies. In addition, MLRun supports auto value imputing based on the feature’s metadata and statistics. This can save significant development and computation overhead. See [Example 4-15](#ch04.xhtml#mlrun-get-online).

##### Example 4-15. Get online features from MLRun (source: MLRun)

```
# Create the online feature service, substitute NaN values with the feature mean value
svc = fs.get_online_feature_service('transactions-fraud:latest',
                                    impute_policy={"*": "$mean"})

# Get sample feature vector
sample_fv = svc.get([{'source': 'C76780537'}])

# sample_fv Result
[{'amount_max_2h': 14.68,
  'amount_max_12h': 70.81,
  'amount_sum_2h': 14.68,
  'amount_count_2h': 1.0,
  'amount_avg_2h': 14.68}]
```

###### Note

MLRun’s feature stores provide accurate real-time aggregations and low latency by combining pre-calculated values during the ingestion process with real-time calculation at feature request time.

MLRun framework provides a model development and training pipeline, real-time serving pipelines, and integrated model monitoring. MLRun’s feature store is natively integrated with the other components, eliminating redundant glue logic, metadata translation, and so on. Thus accelerating time to production.

# Conclusion

With data management and processing being the most critical components of ML, it’s important to understand how to optimally perform data-related tasks. This chapter explores the recommended tools and practices for the various stages of working with your data. We started the chapter by discussing data versioning and lineage, which are essential for tracing the data origin. Then we explored data preparation and analysis at scale, which is how the data is handled so it can be used in production. In this section, we also discussed the architecture of interactive data processing solutions and the differences between batch data processing and real-time processing.

After reviewing the challenges of implementing these practices at scale, we moved on to present the concept of feature stores, which are a central repository for ML features. We covered the various capabilities of a feature store, like data connectivity and offline and online transformation. We also showed where the feature store fits in the MLOps pipeline, from ingesting raw data to supporting the use of that data in training, serving, monitoring, and more. Finally, we reviewed different feature store solutions and how to use them.

# Critical Thinking Discussion Questions

*   Which details does metadata provide us with? As data professionals, why do we need this information?
    
*   Which open-source data versioning tools are there? Which one could be a good fit for your organization?
    
*   What’s the difference between batch processing and stream processing? When is each one used?
    
*   How does a feature store simplify the data management and processing practices? Which capabilities enable this?
    
*   What are the differences between the Feast and the MLRun feature stores? Which one could be a good fit for your organization?
    

# Exercises

*   Choose an open-source solution (DVC, Pachyderm, MLFlow or MLRun) and create a data versioning script or workflow that will record and version data and metadata.
    
*   Create a prototype of a batch processing pipeline with the tool of your choice.
    
*   Connect a Trino data connector to a data source.
    
*   Train a demo model (you can use Hugging Face if you need a sample model) with a feature store.
    
*   Create a feature set and ingestion pipeline in MLRun. You can use [this project](https://docs.mlrun.org/en/stable/feature-store/end-to-end-demo/01-ingest-datasources.xhtml) as a reference.
    

# Chapter 5. Developing Models for Production

This chapter is about “developing models for production.” Creating the appropriate environment to build models is at the heart of this. A woodworker that creates tables and chairs for a living needs to have a proper workshop. The workshop needs adequate space, ideally large enough to store raw materials, like wood and paint, and an organized set of tools. This workshop context is what ultimately enables the creation of high-quality furniture. Without this workspace, the quality and output of the furniture suffer.

Similarly, with MLOps, the context matters. In [Figure 5-1](#ch05.xhtml#Figure-5.1-environment), the tools and environment a model builds in are critical. A critical first step is an SDK coupled with proper editors, like Visual Studio Code, a cloud-based development environment, and CI/CD from the beginning.

![](assets/5.1-environment.png)

Another way to explain the context is to look at a picture of the beach I walked on while writing this book in [Figure 5-2](#ch05.xhtml#Figure-5.5-open-beach). The sounds of the ocean waves, the sun sparkling in the sky, and bare feet walking in warm water are excellent contexts for writing a book. The reason is that the beach gives a sense of relaxation, which makes a person naturally reflective.

![](assets/5.5-open-beach.jpeg)

Many developers have a workflow each day where they work for four hours, take a walk for an hour, then finish another four hours of work. They do this because it creates the context of first engaging with a problem, then getting your mind off the coding challenge, letting it work in the background, and then working on the rest of the solution that popped up during the mid-day walk.

Similarly, this chapter covers many concepts in building machine learning models within the correct context. Next, let’s get into the details surrounding the model building.

# Running, Tracking, and Comparing ML Jobs

In many ways, there is nothing new about the concept of running, tracking, and comparing ML jobs to a traditional software engineer. Software engineers follow CPU, Memory, Disk I/O, and microservice response times when deploying services. Machine Learning operations fits nicely into the conventional scope of monitoring for software engineering.

Let’s start by looking at Azure Databricks as a simplified toy example of running, tracking, and comparing ML jobs [Figure 5-3](#ch05.xhtml#Figure-5.8-databricks-mlops). At a high level, this is an end-to-end MLOps solution that allows you to use AutoML to create an experiment, serve a model and then serve out the model in many different environments, including Databricks itself via a Databricks endpoint or a containerized deployment in Azure, AWS, GitHub Codespace or more.

![](assets/5.8-databricks-mlops.png)

Let’s break it down further and talk about each step along the way. Notice that a Kaggle dataset on [classifying Fake News](https://www.kaggle.com/datasets/ruchi798/source-based-news-classification) begins the journey. Next, that dataset uploads into the DBFS (Databricks File System). The DBFS is a distributed file system mounted into a Databricks workspace and available on Databricks clusters. You can experiment with this workflow with many simple Kaggle datasets. In [Figure 5-4](#ch05.xhtml#Figure-5.9-databricks-ui), the data bricks UI essentially maps out many of the sequential steps in this pipeline, from data to compute to models to serving, etc.

![](assets/5.9-databricks-ui.png)

One of the dependencies on this end-to-end MLOps pipeline is that a compute cluster is necessary for hosting the DBFS and doing the AutoML. In [Figure 5-5](#ch05.xhtml#Figure-5.10-databricks-cluster), a default cluster in Azure Databricks launches with an exemplary configuration of a minimum of two workers and a max of eight workers and terminates after 120 minutes.

![](assets/5.10-databricks-cluster.png)

Once this is up and running, uploading data to the DBFS and running experiments unlocks the ability to run AutoML jobs in [Figure 5-6](#ch05.xhtml#Figure-5.11-databricks-automl). The critical inputs to the AutoML experiment are the cluster, the ML problem type, in this case, “classification”, the input training, which lives on the DBFS, and finally, the “Prediction target,” which is the column to predict. The Databricks AutoML system does the rest.

![](assets/5.11-databricks-automl.png)

# AutoML

You can see a great example of this using the default diamonds dataset with the DBFS. Notice in [Figure 5-7](#ch05.xhtml#Figure-5.12-databricks-automl) that multiple training runs work towards optimizing the accuracy metrics creating notebooks and models as artifacts.

![](assets/5.12-databricks-automl.png)

Further, if you drill down into the model further, you can see in [Figure 5-8](#ch05.xhtml#Figure-5.13-model-inference) that there are three modes “Real-time,” which sets up an endpoint running on virtual machines, streaming “Delta Live Tables,” and “Batch Inference ."In a nutshell, the AutoML workflow enables a build once and produces many styles in that you can serve the model in many ways.

![](assets/5.13-model-inference.png)

## Saving Essential Metadata with the Model Artifacts

As shown in the [following example](https://github.com/noahgift/mlflow-project-best-practices/blob/main/download-model.py), there is yet another way to work with the model created using AutoML inside Databricks. Downloading the model to a cloud-based environment opens up more ways to leverage inference. In this example [Example 5-1](#ch05.xhtml#download_model), the model downloads locally using the `mlflow` API.

##### Example 5-1. Download model (download-model.py)

```
from mlflow.store.artifact.models_artifact_repo import ModelsArtifactRepository
from mlflow.tracking import MlflowClient

client = MlflowClient()
my_model = client.download_artifacts("68baff0203344dfebe89a6c73c6d6cfe", path="model")
print(f"Placed model in: {my_model}")
```

You can list models as shown in the following API call [Example 5-2](#ch05.xhtml#list_model), which allows you to search for models that meet the criteria you are looking for in terms of type, accuracy, etc.

##### Example 5-2. List models (list-models.py)

```
from pprint import pprint
from mlflow.tracking import MlflowClient

client = MlflowClient()
for rm in client.list_registered_models():
    pprint(dict(rm), indent=4)
```

Yet another way to work with the results is to curl the API of Databricks to invoke the model from a bash script, as shown in the following example [Example 5-3](#ch05.xhtml#invoke_endpoint).

##### Example 5-3. Invoke Endpoint

```
curl \
  -u token:$DATABRICKS_TOKEN \
  -X POST \
  -H "Content-Type: application/json; format=pandas-records" \
  -d@data.json \
  https://adb-2951765055089996.16.azuredatabricks.net/model/Fake-News/1/invocations
```

One of the other ways to invoke the model is via Python, as shown in the [following example](https://github.com/noahgift/mlflow-project-best-practices/blob/main/predict-fake-news.py). The general flow is the creation of a Pandas DataFrame with the input data to classify, in this case, the text of a dubious new report [Example 5-4](#ch05.xhtml#predict_fake_news_via_Python).

##### Example 5-4. List models (predict-fake-news.py)

```
#!/usr/bin/env python

import os
import requests
import pandas as pd

my_data = {
    "text": {"aliens are coming to invade earth"},
}
df = pd.DataFrame(data=my_data)

def create_tf_serving_json(data):
    return {
        "inputs": {name: data[name].tolist() for name in data.keys()}
        if isinstance(data, dict)
        else data.tolist()
    }

def score_model(dataset):
    url = "https://adb-2951765055089996.16.azuredatabricks.net/model/Fake-News/1/invocations"
    headers = {"Authorization": f'Bearer {os.environ.get("DATABRICKS_TOKEN")}'}
    data_json = (
        dataset.to_dict(orient="split")
        if isinstance(dataset, pd.DataFrame)
        else create_tf_serving_json(dataset)
    )
    response = requests.request(method="POST", headers=headers, url=url, json=data_json)
    if response.status_code != 200:
        raise Exception(
            f"Request failed with status {response.status_code}, {response.text}"
        )
    return response.json()


if __name__ == "__main__":
    print(score_model(df))
```

You can see this API invocation in the Azure Cloud shell in [Figure 5-9](#ch05.xhtml#Figure-5.15-azure-cloud-shell).

![](assets/5.15-azure-cloud-shell.png)

You can see this API invocation in the GitHub Codespace in [Figure 5-10](#ch05.xhtml#Figure-5.16-github-codespaces).

![](assets/5.16-github-codespaces.png)

###### Note

Here is a walkthrough of this entire process.

*   [YouTube walkthrough](https://studio.youtube.com/video/PUXhWZQW8BI/edit?c=UCNDfiL0D1LUeKWAkRE1xO5Q)
    
*   [MLOps Platforms from Development to Production](https://learning.oreilly.com/videos/mlops-platforms-from/032232022VIDEOPAIML)
    

You can also take the downloaded model from earlier and put in into a FastAPI microservice as shown in the [following example](https://github.com/noahgift/mlflow-project-best-practices/blob/main/main.py) in [Example 5-5](#ch05.xhtml#predict_fake_news_via_FastAPI).

##### Example 5-5. predict via FastAPI (main.py)

```
# pylint: disable=no-name-in-module
# pylint: disable=no-self-argument

from fastapi import FastAPI
import uvicorn
import mlflow
import pandas as pd
from pydantic import BaseModel
from fastapi.responses import JSONResponse
from fastapi.encoders import jsonable_encoder

class Story(BaseModel):
    text: str

def predict(text):
    print(f"Accepted payload: {text}")
    my_data = {
        "text": {0: text},
    }
    data = pd.DataFrame(data=my_data)
    result = loaded_model.predict(pd.DataFrame(data))
    return result


# Load model as a PyFuncModel.
loaded_model = mlflow.pyfunc.load_model('model')
app = FastAPI()

@app.post("/predict")
async def predict_story(story: Story):
    print(f"predict_story accepted json payload: {story}")
    result = predict(story.text)
    print(f"The result is the following payload: {result}")
    payload = {"FakeNewsTrueFalse": result.tolist()[0]}
    json_compatible_item_data = jsonable_encoder(payload)
    return JSONResponse(content=json_compatible_item_data)

@app.get("/")
async def root():
    return {"message": "Hello Model"}

@app.get("/add/{num1}/{num2}")
async def add(num1: int, num2: int):
    """Add two numbers together"""

    total = num1 + num2
    return {"total": total}


if __name__ == '__main__':
    uvicorn.run(app, port=8080, host='0.0.0.0')
```

To take the workflow to the next level, containerize it, as shown in the following [Example 5-6](#ch05.xhtml#containerize_inference_application) [Dockerfile](https://github.com/noahgift/mlflow-project-best-practices/blob/main/Dockerfile).

##### Example 5-6. Dockerfile of FastAPI application

```
FROM public.ecr.aws/lambda/python:3.8

RUN mkdir -p /app
COPY ./main.py /app/
COPY model/ /app/model/
COPY ./requirements.txt /app/requirements.txt
RUN pip install --no-cache-dir --upgrade -r /app/requirements.txt
WORKDIR /app
EXPOSE 8080
CMD [ "main.py" ]
ENTRYPOINT [ "python" ]
```

The reason for containerizing the ml application is that it allows you to build once and deploy many. For example, you could deploy to AWS App Runner or Azure App Services, or Google Cloud run.

# Auto-Logging and MLOps Automation

One of the valuable aspects of MLFlow is that it can automatically log what is going on [Example 5-7](#ch05.xhtml#mlflow_inference_application) [in the project](https://mlflow.org/docs/latest/tracking.xhtml#automatic-logging). It accomplishes this by using `mlflow.autolog()`

##### Example 5-7. mlflow inference logging

```
import mlflow

from sklearn.model_selection import train_test_split
from sklearn.datasets import load_diabetes
from sklearn.ensemble import RandomForestRegressor

mlflow.autolog()

db = load_diabetes()
X_train, X_test, y_train, y_test = train_test_split(db.data, db.target)

# Create and train models.
rf = RandomForestRegressor(n_estimators = 100, max_depth = 6, max_features = 3)
rf.fit(X_train, y_train)

# Use the model to make predictions on the test dataset.
predictions = rf.predict(X_test)
autolog_run = mlflow.last_active_run()
```

Next, let’s move to a different topic: handling training at scale.

# Handling Training at Scale

Training at scale is a fascinating problem, and it is worth starting at a theoretical scale first. In [Figure 5-11](#ch05.xhtml#Figure-5.3-unique-characteristics), notice the idea of building an ML model once and deploying many.

![](assets/5.3-unique-characteristics.png)

What makes this process possible is first building for the [ONNX runtime](https://github.com/microsoft/onnxruntime). Next a developer decides which of the potential targets to deploy toward. In the case of web applications using specialized hardware and commercial MLOps inference services, there is a well-established story for deployment.

An emerging use case, though as well, is the concept of binary-based workflows that embed the Onnx runtime and then use their high-performance and low-memory footprint to unlock the ability to make inferences on serverless platforms like AWS Lambda, build CLI tools, and also deploy to edge hardware like a [Coral.ai device](https://coral.ai/). In this scenario, the officially supported runtimes of both C# and Rust are potential solutions.

## Building and Running Multi-Stage Workflows

An excellent example of why you want to use a platform for MLOPs is tying together the multiple stages of a lifecycle. These include data collection and preparation, feature engineering, model training, model selection and tuning, and deployment and monitoring. Orchestrating all of this ad-hoc is not scalable in the real world. Let’s dive into the details of this next.

## Scaling and Automating Deep Learning Tasks

The notebook in a modern MLOps workflow has many purposes, which adds to the challenge of it remaining at the center of MLOps in the future as shown in [Figure 5-12](#ch05.xhtml#Figure-5.15-mlops-notebooks).

![](assets/5.15-mlops-notebooks.jpg)

Notice how the challenge lies in merging a science focus with an engineering focus. Let’s dive into this in more detail next.

## Managing Computation Resources Efficiently

The challenge in MLOps at scale is orchestrating the continuum between stops ingesting data, doing exploratory data analysis, modeling, and then building a conclusion in [Figure 5-13](#ch05.xhtml#Figure-5.17-sagemaker)

![](assets/5.17-sagemaker.jpg)

Each step in building and deploying models requires extensive, scalable resources, for example, S3, to store the raw data and the model placeholder. The training jobs also need elastic resources, as do the inference jobs. MLOps is a distributed computing problem, and dealing with a platform is one of the most reasonable ways to solve this difficult problem.

Another excellent example of how vital scalability is Apple’s [CreateML tool](https://developer.apple.com/machine-learning/create-ml/). In [Figure 5-14](#ch05.xhtml#Figure-5.4.2-createml) the [dogs and cats dataset](https://www.kaggle.com/c/dogs-vs-cats) from Kaggle gets dropped onto the UI to setup a training job.

![](assets/5.4.2-create-ml.png)

Next, a Mac Pro M2 Max with 38 GPU cores and 10 CPU cores can fully utilize these GPU cores[Figure 5-15](#ch05.xhtml#Figure-5.4-create-ml-saturate). The tooling here is the key, in that fast merging of software and hardware allows a developer to prototype models quickly and then later export them from [CoreML](https://developer.apple.com/documentation/coreml) format to ONNX.

![](assets/5.4-create-ml-saturate.jpg)

Yet another example of a high-level framework is [ML.NET framework](https://dotnet.microsoft.com/en-us/apps/machinelearning-ai/ml-dotnet) shown in [Figure 5-16](#ch05.xhtml#Figure-5.18-mlnet) saturating the CPU cores as it does AutoML.

![](assets/5.18-mlnet.png)

In the end, not only is there a model created that efficiently works with ONNX, but a console application in C# is part of the framework’s build process enabling tight integration with C#, Visual Studio Code, the model format and ultimately a build once, deploy tool.

There are many emerging examples of tools that couple hardware, software, model format, and the ultimate deliverable in the MLOps space and it is worth having these frameworks evaluated for your organization’s goals.

## AWS Silicon Innovation

Another piece of the puzzle in MLOps is [custom silicoln](https://aws.amazon.com/silicon-innovation/). On the training side, there is [Tranium](https://aws.amazon.com/machine-learning/trainium/), a high-performance machine-learning training accelerator designed to accelerate deep learning workloads. The first version uses about 50% less energy, over 2x faster training and over 70% lower cost to train and the second version has up to 4x higher throughput and 10x lower latency as well as support for ultra-large Generative AI models.

In terms of inference, there is also [AWS Inferentia](https://aws.amazon.com/machine-learning/inferentia/) This performance means that an OPT-30B model (30 billion parameter large language model) is over 3x faster than GPU-based inference, and an OPT-66B is out of memory on a GPU, yet does 248 tokens a second on Inferentia2.

You can explore this workflow using the [AWS Neuron SDK](https://github.com/aws-neuron/aws-neuron-sdk). The main takeaway is that a critical aspect of operationalizing models is looking for software and hardware solutions that improve cost and energy efficiency as in [Figure 5-17](#ch05.xhtml#Figure-5-30-aws-ml-stack). Ultimately AWS has the deepest synergy in MLOps. Many smaller players need help to keep this synergy with hardware, software, and platforms, as evidenced by low GitHub stars on competing solutions, common market share, or even negative exits of startups in the MLOps space.

![](assets/5-30-aws-ml-stack.png)

# CI/CD and Automation for the Development Flow

An essential consideration in MLOps is setting an intention for a project. What is the goal we are working towards when doing machine learning? The toolchain will be different depending on how the goals get defined. If the goal is to automate from the beginning, which is a central goal of MLOPs, then the toolchain needs to support automation, i.e., an SDK vs. a notebook, as shown in [Figure 5-18](#ch05.xhtml#Figure-5.2-problem-first).

![](assets/5.2-problem-first.png)

A notebook-first approach to MLOps is problematic because it fails the context test, i.e., if the context is software automation, then that goal requires a continuous integration pipeline. Since notebooks are essentially “write-only” and impossible to test, the goal cannot be the same as automation. Instead, notebooks aim to explore an idea involving data science in a write-only, untestable fashion.

The three critical aspects of CI/CD (Continuous Integration)/ (Continuous Delivery) are automation, continuous feedback and testing, and collaboration. Let’s discuss each briefly:

*   Automation: CI/CD relies exclusively on automation to ensure a fast, efficient, and error-free software delivery pipeline. This capability means that every step of the software development pipeline requires automation, including the development environment, building software, testing, packaging, and deployment through IAC (Infrastructure As Code). Automation also reduces the chances of human error and ensures consistent results.
    
*   Continuous feedback and testing: The key takeaway here is that software quality control tools, including linting, formatting, compiling, and testing, happen each time changes are made in the software. The code is consistently improving each time it changes since these quality tools report back the status of the changes.
    
*   Collaboration: The continuous improvement of the codebase and the ability to spin up a new environment enables deep collaboration between team members.
    

Another twist to this workflow is newer techniques shown in [Figure 5-19](#ch05.xhtml#Figure-5-7-ai-in-the-ci-cd-loop). What is relatively new are the items on the right in the red, i.e., IAC, AI Coding suggestions, and a reproducible development environment. The entire point of CI/CD is to facilitate automation, feedback, testing, and collaboration. The more inputs that assist with this, the better. If you don’t have a compiler, it hurts you; if you don’t have a linter or even a fast linter, it hurts you, etc (see rust appendix for more information on this).

![](assets/5-7-ai-in-the-ci-cd-loop.png)

A takeaway here is that using high-performance tools is something to be aware of as a way to improve a CI/CD pipeline. Also, note that a notebook doesn’t work in a CI/CD pipeline. How could it work? A notebook has a different context than automation. While a useful tool, it is orthogonal to MLOps, since a notebook is a form of authoring media, like a new book form.

What could the CI/CD pipeline look like then for MLOps? A good starting point is the [GitHub MLOps template repo](https://github.com/nogibjj/mlops-template). The essential items include a configurable cloud-based development environment, like GitHub, a `Makefile` that orchestrates the work locally and in the build system, and finally, a cloud-based build system.

But this is only the base, similar to the software engineering base. The unique CI/CD components of MLOps include, at the very least, a model. Let’s dive into an end-to-end pre-trained model example next.

## End to End PyTorch to AWS App Runner

A great way to eliminate some of the complexity in understanding CI/CD for MLOps is to build a [simple example microservice](https://github.com/nogibjj/pytorch-fastapi-aws-apprunner) that uses a pre-trained PyTorch model. To do this will involve modifying the [official tutorial](https://pytorch.org/tutorials/intermediate/flask_rest_api_tutorial.xhtml) of using Flask with PyTorch.

First, there is an [app.py](https://raw.githubusercontent.com/nogibjj/pytorch-fastapi-aws-apprunner/main/app.py) that builds a PyTorch model that used for prediction served out via FastAPI. The first function transforms the image, then the `get_prediction` gets the inference and finally, the `async def predict` puts it all together.

\[\[Pre-Trained PyTorch Microservice\]\] .FastAPI application PyTorch

```
"""
Build a PyTorch model that used for prediction served out via FastAPI
"""

import io
import json

from torchvision import models
import torchvision.transforms as transforms
from PIL import Image
import fastapi
from fastapi import File, UploadFile
import uvicorn

app = fastapi.FastAPI()

model = models.densenet121(weights="DenseNet121_Weights.IMAGENET1K_V1")
model.eval()
imagenet_class_index = json.load(open("imagenet_class_index.json", encoding="utf-8"))


def transform_image(image_bytes):
    my_transforms = transforms.Compose(
        [
            transforms.Resize(255),
            transforms.CenterCrop(224),
            transforms.ToTensor(),
            transforms.Normalize([0.485, 0.456, 0.406], [0.229, 0.224, 0.225]),
        ]
    )
    image = Image.open(io.BytesIO(image_bytes))
    return my_transforms(image).unsqueeze(0)


def get_prediction(image_bytes):
    tensor = transform_image(image_bytes=image_bytes)
    outputs = model.forward(tensor)
    _, y_hat = outputs.max(1)
    predicted_idx = str(y_hat.item())
    return imagenet_class_index[predicted_idx]


@app.get("/")
def index():
    return {"message": "Hello World"}


@app.post("/files/")
async def create_file(file: bytes = File()):
    return {"file_size": len(file)}


@app.post("/uploadfile/")
async def create_upload_file(file: UploadFile):
    return {"filename": file.filename}


@app.post("/predict")
async def predict(file: UploadFile = File(...)):
    image_bytes = await file.read()
    print(len(image_bytes))
    class_id, class_name = get_prediction(image_bytes=image_bytes)
    return {"class_id": class_id, "class_name": class_name}


if __name__ == "__main__":
    uvicorn.run(app, host="0.0.0.0", port=8080)
```

The `Dockerfile` is very simple as shown. It serves out traffic to port 8080 on the external interface of “0.0.0.0”.

```
FROM tiangolo/uvicorn-gunicorn-fastapi:python3.9

WORKDIR /code

COPY . /code

RUN pip install --upgrade pip &&\
    pip install --no-cache-dir --upgrade -r /code/requirements.txt

CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8080"]
```

The container image gets pushed to Amazon ECR, or you can point to the source code repo, as shown in [Figure 5-20](#ch05.xhtml#Figure-5-19-container-registery) and then you setup [AWS App Runner](https://aws.amazon.com/apprunner/).

![](assets/5-19-container-registery.png)

Next, you configure the build process in [Figure 5-21](#ch05.xhtml#Figure-5-20-configure-build).

![](assets/5-20-configure-build.png)

Next, you configure the service in [Figure 5-22](#ch05.xhtml#Figure-5-21-configure-service).

![](assets/5-21-configure-service.png)

Next, you configure the deployment in [Figure 5-23](#ch05.xhtml#Figure-5-22-configure-deploy).

![](assets/5-22-deployed-service.png)

At the end of this process, you get to the deployed endpoint and connect to `/docs`; from there, it is straightforward to upload images to predict. You can view [entire walkthrough on YouTube here](https://www.youtube.com/watch?v=-CoOotWG30E).

## Guest Section on GitHub Actions with Azure Machine Learning Studio

In this section, I contacted [O’Reilly Alfredo Deza](https://www.oreilly.com/pub/au/7872), a Developer Advocate for Azure. He worked extensively with DevOps and MLOps workflows in education and shared this technique with the readers of this book.

One of the things I tend to use when working with Azure Machine Learning Studio is to add GitHub Actions, making it a powerful combination. GitHub Actions work with YAML files called workflow files. These files are easy to read and, therefore, easy to maintain - an essential mix when working with enterprise-level software.

There are two common patterns I use, and a base to build on further:

1.- Registering models or datasets from GitHub to Azure ML Studio

2.- Retrieving specific versions of models for packaging

## Authenticating to Azure

Any time you are interacting with Azure to create or retrieve data from you account will require authentication. There are different ways that you can authenticate with GitHub Actions but one that I tend to use the most is using an Azure Service Principal. A Service Principal is a way to create accounts that have limited scope to resources and it only needs one simple command.

Using the [Azure Cloud Shell](https://shell.azure.com/?WT.mc_id=academic-0000-alfredodeza) run the following command to find your subscription id:

```
az account show --query id -o tsv
```

Capture the resulting ID and use it in the next step, replace `$AZURE_SUBSCRIPTION_ID` with the result from the previous command:

```
az ad sp create-for-rbac  --sdk-auth --name "github-actions" \
   --role contributor --scopes /subscriptions/$AZURE_SUBSCRIPTION_ID
```

That command will generate a JSON output that you must use it in a GitHub repository secret. A repository secret is a way to securely store sensitive information like the one provided by the Azure CLI command. In the GitHub repository where you want to use GitHub Actions with Azure, go to Settings, then Actions, and select New. For the name, use `AZURE_CREDENTIALS` and for the value, paste the JSON output from the last command.

Although you will need to configure the YAML workflow with more components, this is the step you would use to authenticate to Azure:

```
  - uses: azure/login@v1
    with:
     creds: ${{ secrets.AZURE_CREDENTIALS }}
```

The fact that it only requires three lines to authenticate properly with Azure with an account that has enough permissions allows you to concentrate in the other aspects of your Machine Learning project, rather than spending time in trying to make authentication work. This is one of the main reasons I like GitHub Actions.

## Use your Azure ML Workspace

This section will assume that you already have created an Azure Machine Learning workspace and you have access to its portal at [*https://ml.azure.com/?WT.mc\_id=academic-0000-alfredodeza*](https://ml.azure.com/?WT.mc_id=academic-0000-alfredodeza). One common use for the Azure ML workspace is to create and store models in Azure. You can use these registered models with GitHub Actions after authenticating to perform different workflows including packaging.

The following example shows you how to make sure that the Azure CLI will have everything it needs to work with the Azure ML workspace and retrieve the model:

```
- name: set auto-install of extensions
  run: az config set extension.use_dynamic_install=yes_without_prompt
```

Next, replace `workspace-name` and `workspace-group` with your Azure ML workspace and resource group respectively so that GitHub Actions can attach the workspace to the job:

```
- name: attach workspace
  run: az ml folder attach -w "workspace-name" -g "workspace-group"
```

Finally, you can retrieve a model with a specific version. The version comes after the colon in the value to `--model-id`. In the following example, we use a GPT-2 model in the ONNX format that was previously registered in Azure ML:

```
- name: retrieve the ONNX model
  run: az ml model download -t "." --model-id "GPT-2-onnx:1"
```

Although I haven’t gone into more details to build an end-to-end example, hopefully, I’ve demonstrated some powerful building blocks you can use to create more complex jobs using Azure ML and GitHub Actions. In the past, I’ve used these examples to package Machine Learning models and deploy them to a container registry. The increased readability of GitHub Actions, with the ease of the Azure CLI, makes this an outstanding combination worth experimenting with.

Let’s move on to wrap up the chapter next.

# Conclusion

This chapter discusses the context of building machine learning models, particularly about building once and deploying many. In these scenarios, tight feedback exists between hardware, software, model formats, and the ultimate deliverable. The context matters in building models, and there are many unique contexts to consider ranging from binary deployment to embedded to planet-scale inference and more.

We also discussed two different end-to-end examples. The first involved using Databricks as a build once and deploying many solutions for MLOps. Examples included invoking via their ecosystem, pulling out the model, and putting it into a containerized workflow. We wrapped up a pre-trained FastAPI PyTorch example. Go through the exercises and critical thinking questions to get the most out of this chapter.

# Exercises

*   Deploy a simple image classification model: Using a pre-trained convolutional neural network (CNN), build a FastAPI web application that takes an image file from the user, processes it using the model, and displays the predicted class. The app should handle errors and unexpected inputs and be ready for deployment in production via a [locust](https://locust.io/) based load-test
    
*   Building a scalable text classification pipeline: Train a deep learning model for text classification on a large dataset and develop a scalable pipeline for inference using [Ludwig](https://ludwig.ai/latest/).
    
*   Integrating model monitoring and alerting: Develop a system that continuously monitors a deployed model’s performance using an MLOps platform. Send alerts when anomalies are detected. Use data drift analysis to link performance anomalies with data changes.
    
*   Implementing a model versioning system: Build a versioning system for models that allow the team to track the evolution of models over time from scratch. The system should follow the model’s code, data, and hyperparameters and allow for easy rollback to previous versions. Use Git for version control and tools like DVC for managing data and models.
    
*   Creating a model retraining pipeline: Develop a pipeline that automates the retraining models as new data becomes available. The project should monitor data sources for changes, retrain the model on the latest data, and deploy the new model to production. Consider using technologies like Apache Airflow for workflow management, Kubernetes for deployment, AWS Sagemaker, or Azure ML Studio.
    

# Critical Thinking Discussion Questions

*   What factors influence a model’s performance in a production deployment? How could you stack the deck in favor of your organization to achieve a successful outcome?
    
*   What is your organization’s data management and model-building governance policy? Could using an [enterprise data catalog](https://learning.oreilly.com/library/view/the-enterprise-data/9781492098706/) improve governance?
    
*   What are the critical bottlenecks in deploying models in terms of performance? How do languages, techniques, or designs help build solutions that scale?
    
*   What is the advantage of building once and deploying to many targets, i.e. binary command-line tools, edge-based devices, and mobile devices using [Onnx](https://github.com/microsoft/onnxruntime)?
    
*   How could you protect the privacy of the users of your ML systems? Are there approaches that will comply with all near-term government regulation?
    

# Chapter 6. Deployment of Models and AI Applications

Processing data, training, and validating models are just precursors to the real thing: building and deploying an application that uses the data you generated and the model you have built to drive decisions and actions.

To deliver machine learning applications, start by building and registering the model(s) for use in the production application. Then, create an application [pipeline](https://www.iguazio.com/glossary/machine-learning-pipeline/) that generally accepts events or data, prepares the required model features, infers results using one or more models, and drives actions. Finally, monitor the data, models, and applications to guarantee their availability and performance. In cases of problems or degraded model performance, drive corrective actions.

Many organizations still think of “serving a model” or creating a model endpoint. However, they need to pay more attention to the bigger picture of delivering an ML application as a whole, instead of dividing the application delivery responsibility between data science and engineering teams. Ignoring the bigger picture will lead to significant functionality gaps, failures, unnecessary risks, and long delays.

# Model Registry and Management

A *model registry* is a central repository for storing ML models and their metadata and managing the model lifecycle and versions. Once a [model training](https://www.iguazio.com/glossary/model-training/) process completes, it saves the model and its metadata in the registry. Then, different functions (such as [evaluation](https://www.iguazio.com/glossary/model-evaluation/), testing, optimization, and so on) extend the model metadata or update the model files. Finally, the serving functions or application pipelines load the model and use it for making predictions.

Model registries provide the following functionality:

*   Storing models along with their metadata and labels (tags).
    
*   Managing model access, versions, and lifecycle.
    
*   Enabling the finding, grouping, and comparing of models based on metadata attributes or labels.
    
*   Storing information required for the [model deployment](https://www.iguazio.com/glossary/model-deployment/) and monitoring.
    
*   Tracking the model status and approval process.
    
*   Providing a simple or automated mechanism to deploy models into production.
    

Usually, the model registry is integrated with the experiment tracking system. This way, the essential metadata from the training or experiment is automatically recorded in the registry without manual intervention. However, most model registry solutions provide APIs to register models trained on other systems.

A model consists of the following data and metadata elements:

*   *Base metadata*: Unique model name, identifier, description, project, owner, version information, and so on.
    
*   *Labels*: A set of key/value tags used to label, filter, group, and search the model.
    
*   *Model files*: The saved model (for example, in pkl, json, or hd5 formats) and auxiliary files used by the model serving process.
    
*   *Tracking information*: References to how the model was trained, parameters, data sources, code version, training framework, and so on.
    
*   *Model metrics*: Performance metrics collected during the training, evaluation, and testing processes. For example, [model accuracy](https://www.iguazio.com/glossary/model-accuracy-in-ml/), loss, f1 score, ROC curves, feature importance, and so on.
    
*   *Dataset schema*: The schema of the model inputs (X) and outputs (Y), including field names, order, and types.
    
*   *Deployment data*: Information and parameters required for the model deployment such as package dependencies, container image, runtime parameters, and so on.
    
*   *Monitoring metadata*: Information required for monitoring the model performance or [drift](https://www.iguazio.com/glossary/drift-monitoring/). For example, statistical information and histograms per feature to determine if there is a drift between the training data and serving data.
    
*   *Status and state*: Information about the current state, usage, and approvals of the model.
    

In many cases, the training pipelines generate multiple models. For example, when trying different algorithms or parameter combinations. In such cases, we will use different names or labels per model and can compare the models to select the most suitable option. In addition, the same model pipeline may produce multiple models, one for every subset of the data (for example, a model per user, per device, per country, and so on).

Model registries provide APIs and a user interface to create, update, retrieve, list, compare, and deploy models. Model registries are a component of an MLOps or data science platform. For example, open-source solutions include [MLflow](https://mlflow.org/docs/latest/model-registry.xhtml) and [MLRun](https://docs.mlrun.org/en/stable/store/models.xhtml). In addition, there are commercial solutions from [AWS SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/model-registry.xhtml), [Google Vertex AI](https://cloud.google.com/vertex-ai/docs/model-registry/introduction), and [DataRobot](https://docs.datarobot.com/en/docs/mlops/deployment/registry/reg-create.xhtml). Although registries can import or export models, the best approach is to use the built-in registry once you choose the MLOps platform.

## Solution Examples

Some solutions (for example, in SageMaker and Vertex AI) require you to package the model in a container and provide minimal visibility into the model origin and metadata. This approach may lead to additional work, functional limitations (cannot serve multiple models in the same container), and limited observability.

**SageMaker Example:**

[Example 6-1](#ch06.xhtml#sagemaker-registry) shows a code example for registering a model in AWS SageMaker. It covers the following steps that are required to register a model:

1.  Save the model and the code in a *tar.gz* package and upload it to S3.
    
2.  Build a container image or use a pre-built Docker image.
    
3.  Create a model package group.
    
4.  Create a model package and specify the information about the model package, image, runtime preference, metadata, and so on.
    

##### Example 6-1. Registering a model in AWS SageMaker

```
import boto3
from sagemaker import image_uris

region = boto3.Session().region_name
client = boto3.client('sagemaker', region)

# Require you to first package the model in tar.gz and upload to S3

# Specify the S3 location of the model package
model_package_location = 's3://my-bucket/my-model-package.tar.gz'

# Find the image url for a SageMaker built-in inference image
inference_image = image_uris.retrieve(
    framework="sklearn",
    region=region,
    version="1.0-1",
    py_version="py3",
    instance_type="ml.m5.large",
)


# Define the model package metadata
model_package_name = 'my-model-package'
model_package_group_name = model_package_name + "-group"
model_package_description = 'A sample model package'
model_package_framework = 'scikit-learn'
model_package_runtime = 'Python 3.8'

print(model_package_group_name)
group_response = client.create_model_package_group(
    ModelPackageGroupName=model_package_group_name,
    ModelPackageGroupDescription="My group description",
)

model_package_version_response = client.create_model_package(
    ModelPackageGroupName=model_package_group_name,
    ModelPackageDescription="scikit-learn demo",
    ModelPackageVersion='1.0',
    MetadataProperties={
        'GeneratedBy': 'my-username'
    },
    InferenceSpecification={
        "Containers": [
            {
                "ContainerHostname": "scikit-learn",
                "Image": inference_image,
                "ModelDataUrl": model_package_location,
                "Framework": "SAGEMAKER-SCIKIT-LEARN",
                "Environment": {
                    "SAGEMAKER_CONTAINER_LOG_LEVEL": "20",
                    "SAGEMAKER_PROGRAM": "inference.py",
                    "SAGEMAKER_REGION": region,
                },
            },
        ],
        "SupportedRealtimeInferenceInstanceTypes": [
            "ml.c5.xlarge",
            "ml.m5.xlarge",
        ],
        "SupportedContentTypes": ["text/csv"],
        "SupportedResponseMIMETypes": ["application/json"],
    },
)
```

**MLflow Example:**

In MLflow, the experiment tracking service can save model artifacts (with the experiment metadata), and the model registry can register artifacts as models. See [Example 6-2](#ch06.xhtml#mlflow-registry-code), which demonstrates how a training job logs a model artifact and registers it.

##### Example 6-2. Registering a model in MLflow

```
from sklearn import ensemble, metrics
from sklearn.model_selection import train_test_split

import mlflow
import mlflow.sklearn
import pandas as pd

dataset = pd.read_cvs("data.csv")

with mlflow.start_run(run_name="YOUR_RUN_NAME") as run:
    params = {"n_estimators": 5, "learning_rate": 0.1}
    model = ensemble.GradientBoostingClassifier(**params)

    # Initialize the x & y data and split to train and test sets
    x = dataset.drop("label", axis=1)
    y = dataset["label"]
    x_train, x_test, y_train, y_test = train_test_split(x, y)

    # Log parameters and metrics using the MLflow APIs
    mlflow.log_params(params)


    # Train the model and log the metrics
    model.fit(x_train, y_train)
    predicted_probs = model.predict_proba(x_test)
    roc_auc = metrics.roc_auc_score(y_test, predicted_probs[:,1])
    mlflow.log_metric("test_auc", roc_auc)

    # Log the sklearn model and register as version 1
    mlflow.sklearn.log_model(
        sk_model=model,
        artifact_path="sklearn-model",
        registered_model_name="sk-learn-reg-model"
    )
```

Once the model is registered it can be viewed in the MLflow UI, as shown in [Figure 6-1](#ch06.xhtml#mlflow-registry-ui).

![](assets/mlflow-registry.png)

**MLRun Example:**

In MLRun, The training function can use the framework-specific `apply_mlrun()` method to automatically grab all the model details, metadata, data schema, and statistics and save the model in the registry (see [Example 6-3](#ch06.xhtml#mlrun-registry-code)). Notice that MLRun automates data movement and the collection of experiment metadata, parameters, and metrics.

##### Example 6-3. Registering a model in MLRun

```
import pandas as pd
from sklearn import ensemble
from sklearn.model_selection import train_test_split
from mlrun.frameworks.sklearn import apply_mlrun

def train(
    dataset: pd.DataFrame,
    label_column: str = "label",
    n_estimators: int = 100,
    learning_rate: float = 0.1,
    model_name: str = "cancer_classifier",
):
    # Initialize the x & y data and split to train and test sets
    x = dataset.drop(label_column, axis=1)
    y = dataset[label_column]
    x_train, x_test, y_train, y_test = train_test_split(x, y)

    # Pick an ideal ML model
    model = ensemble.GradientBoostingClassifier(
        n_estimators=n_estimators, learning_rate=learning_rate
    )

    # Generate and register model artifact along with all its metrics and metadata
    # MLRun auto extracts the model schema and drift metadata from the test set
    apply_mlrun(model=model, model_name=model_name, x_test=x_test, y_test=y_test)

    # Train the model
    model.fit(x_train, y_train)
```

Once the model is registered it can be viewed in the MLRun UI along with all the automatically gathered metadata. See [Figure 6-2](#ch06.xhtml#mlrun-registry-ui).

![](assets/mlrun-registry.png)

If you have an existing code function that returns a model object, you don’t have to add the `auto_mlrun()` method. Instead, MLRun will automatically detect the model object and save it. However, it will not include all the metadata and statistics. You can add those later using the `update_model()` method.

You can register models you trained on other systems with the project `log_model()` method.

```
model_object = project.log_model('my-model', model_file=model_path, ..)
```

MLRun also provides a simple way to export models and all their metadata into a *.zip* file and load it back into another system. See [Example 6-4](#ch06.xhtml#mlrun-registry-export).

##### Example 6-4. Export and import MLRun models

```
# In the source platfrom export the model artifact into a .zip object
model_object.export("s3://my-bucket/model.zip")

# In the destination system import the model files + metadata from zip into the project
model_object = project.import_artifact("my-model", "s3://my-bucket/model.zip")
```

# Model Serving

Models are a form of equation. They accept numeric values (X) and respond with results or predictions (Y). Models have unique dependencies and development lifecycles. Therefore, it is better to package and deploy them as microservices (containers) and access them through an API. In addition, using an API allows independent scaling of the model (add/remove containers), high availability, granular security, and rolling upgrades.

The most basic approach is manually wrapping the model prediction code with a protocol. For example, using Python [Flask](https://flask.palletsprojects.com/) or [FastAPI](https://fastapi.tiangolo.com/) packages to add HTTP REST API on top of the model. However, this simplistic approach means you must write and maintain a lot of code to handle the different API calls, exceptions, scaling, security, upgrades, and more.

Serving frameworks handle the model deployment, protocol, lifecycle, and monitoring for you. Many of the frameworks require you to build the container package, and they add the deployment, scaling, and so on. Some frameworks (like MLRun) use serverless functions architecture to automatically create the container package and inject advanced functionality and observability into the serving microservice. In addition, there are managed model-serving solutions in the cloud in which you upload the model and don’t need to control the infrastructure.

You can deploy and serve models through an online endpoint (using HTTP/REST or gRPC protocols), which accepts the input dataset and responds with the prediction immediately, or through a streaming or messaging protocol (such as [Kafka](https://kafka.apache.org/), [Kinesis](https://aws.amazon.com/kinesis/), [PubSub](https://cloud.google.com/pubsub), and others) which receives the input events, makes a prediction, and writes the results to a database or an upstream stream/queue.

You can deploy models as part of a batch pipeline. For example, the first step is to prepare the dataset. Then, the model prediction step generates predictions from the incoming dataset and writes the results to the next step or a storage system. The batch pipeline can run on demand or be scheduled at regular intervals.

[Figure 6-3](#ch06.xhtml#serving-modes) illustrates different model-serving deployment options, online (synchronous), stream (asynchronous), and batch.

![](assets/serving-modes.png)

Online serving protocols support multiple operations to handle the entire model lifecycle, for example:

*   *Predict*: Send an input dataset and return the predicted results.
    
*   *Get model metadata*: Get information about the model and its schema.
    
*   *Get Health*: Get the health and readiness of the model.
    
*   *List*: List the models and the versions served by the endpoint.
    
*   *Explain*: Send the input data and return a description (explanation) of the prediction response.
    

[NVIDIA Triton](https://developer.nvidia.com/nvidia-triton-inference-server) (TensorRT), [KServe](https://github.com/kserve/kserve) (KFServing), [Seldon Core](https://github.com/SeldonIO/seldon-core), and MLRun support a standard [model serving protocol](https://docs.mlrun.org/en/stable/serving/model-api.xhtml).

In the advanced solutions, you can control how models are loaded and evicted from memory and a model endpoint can serve multiple models to preserve memory space and computation resources. In addition, they can handle data pre/post-processing and advanced functionality such as ensembles, canaries, monitoring, and more.

[Table 4-2](#ch04.xhtml#feature-store-compare) lists the leading [model serving](https://www.iguazio.com/glossary/model-serving/) solutions.

Table 6-1. Data systems examples by sub-category

  
| Category || [Sagemaker](https://docs.aws.amazon.com/sagemaker/latest/dg/deploy-model.xhtml) || [Vertex AI](https://cloud.google.com/vertex-ai/docs/tabular-data/classification-regression/get-online-predictions) || [MLRun](https://docs.mlrun.org/en/latest/serving/serving-graph.xhtml) || [Seldon](https://docs.seldon.io/projects/seldon-core/en/v2/index.xhtml) || [KServe](https://www.kubeflow.org/docs/external-add-ons/kserve/) || [Triton](https://developer.nvidia.com/nvidia-triton-inference-server) |
| --- | --- | --- | --- | --- | --- | --- |
| Open-source || No || No || Yes || Yes || Yes || Yes |
| --- | --- | --- | --- | --- | --- | --- |
| Managed option || AWS || GCP || cloud + on-prem || cloud + on-prem || No || No |
| Serverless || Yes || Yes || Yes || No || No || No |
| Protocol || Proprietary || Proprietary || Standard || Standard || Standard || Standard |
| Multi-stage pipelines || No || No || Yes || Yes || No || No |
| Streaming || No || No || Yes || Basic || No || No |
| Model Monitoring || Basic || Basic || Yes || Yes || No || No |

## AWS SageMaker

In SageMaker, you can retrieve a model from the registry, deploy it to an endpoint, and call it to generate predictions. For example, see the code in [Example 6-5](#ch06.xhtml#sagemaker-serving1).

##### Example 6-5. Deploy registered model in SageMaker

```
import sagemaker

sagemaker_session = sagemaker.Session()
role = sagemaker.get_execution_role()

# Get the model package from the registry
model = sagemaker.ModelPackage(
    role=role,
    model_package_arn=model_package_arn,
    sagemaker_session=sagemaker_session)

# Deploy the model as an endpoint
predictor = model.deploy(
    initial_instance_count=1,
    instance_type='ml.m5.xlarge',
    endpoint_name="some-name")

# Test the model by sending a request to the endpoint
test_data = {"input": [1, 2, 3, 4, 5]}
response = predictor.predict(test_data)
print(response)
```

If you train the model with SageMaker’s built-in frameworks, you can skip the part of registering the model and immediately deploy it to an endpoint (see [Example 6-6](#ch06.xhtml#sagemaker-serving2)).

##### Example 6-6. Deploy built-in trained model in SageMaker

```
from sagemaker.pytorch import PyTorch

# Train the model using an estimator
pytorch_estimator = PyTorch(entry_point='train_and_deploy.py',
                            instance_type='ml.p3.2xlarge',
                            instance_count=1,
                            framework_version='1.8.0',
                            py_version='py3')
pytorch_estimator.fit('s3://my_bucket/my_training_data/')

# Deploy my estimator to a SageMaker Endpoint and get a Predictor
predictor = pytorch_estimator.deploy(instance_type='ml.m4.xlarge',
                                     initial_instance_count=1)

# `data` is a NumPy array or a Python list.
# `response` is a NumPy array.
response = predictor.predict(data)
```

###### Tip

SageMaker model serving is a great choice when you build your models inside SageMaker. However, it requires more work when using external or standard open-source frameworks. In addition, data processing or application logic is not handled by the serving endpoint and will require external services or serverless functions.

## Seldon Core

Seldon Core is an open-source model serving solution that can deploy over [Docker](https://www.docker.com/) or [Kubernetes](https://kubernetes.io/). Seldon can deploy a single model or a multi-stage pipeline with multiple models and processing steps. In addition, it supports [model monitoring](https://www.iguazio.com/glossary/model-monitoring/) and explainability.

Seldon Core supports two types of model servers (see [Figure 6-4](#ch06.xhtml#seldon-model-types)):

*   *Reusable*: Allows deploying a family of standard models using pre-built images. The models are often fetched from a central repository (like AWS S3 storage).
    
*   *Non-Reusable*: Using a custom model server that requires building a custom Docker image with the code and dependent packages.
    

![](assets/seldon-model-types.png)

Seldon models and pipelines are defined using a YAML file and deployed using the Kubernetes command line tool (Kubectl). See [Example 6-7](#ch06.xhtml#seldon-serving).

##### Example 6-7. Deploy a model using Seldon Core (source: Seldon Core)

Step 1: Create a YAML file describing a single (reusable) model server:

```
apiVersion: machinelearning.seldon.io/v1
kind: SeldonDeployment
metadata:
  name: sklearn
spec:
  name: iris
  protocol: v2
  predictors:
  - graph:
      children: []
      implementation: SKLEARN_SERVER
      modelUri: gs://seldon-models/sklearn/iris-0.23.2/lr_model
      name: classifier
    name: default
    replicas: 1
```

Step 2: Deploy the model to the Kubernetes cluster:

```
> kubectl apply -f resources/iris-sklearn-v2.yaml
seldondeployment.machinelearning.seldon.io/sklearn created
```

Step 3: Test the new endpoint:

```
import requests

inference_request = {
    "inputs": [
        {"name": "predict", "shape": [1, 4], "datatype": "FP32",
         "data": [[1, 2, 3, 4]]}
    ]
}

endpoint = "http://localhost:8003/seldon/seldon/sklearn/v2/models/infer"
response = requests.post(endpoint, json=inference_request)

print(response.json())
```

Seldon Core supports various model deployment options, multi-stage pipelines, and a standard (v2) protocol. However, it is more DevOps-oriented and requires manual configuration and an understanding of the Kubernetes API.

## MLRun Serving

The MLRun MLOps framework includes advanced model and application serving functionality. MLRun serving allows users to define multi-stage real-time pipelines and quickly deploy them to production with the help of [Nuclio](https://nuclio.io/), a real-time serverless engine. Nuclio is a high-performance, elastic, open-source “serverless” framework focused on data, I/O, and compute-intensive workloads. It supports advanced functionality and many triggering options (such as HTTP, cron, Kafka, Kinesis, and others). MLRun and Nuclio’s serverless architecture converts the code and high-level definitions into a hardened, high-performance, self-healing, and auto-scaling service with built-in monitoring and observability.

MLRun serving supports two topology options:

*   *Router*: A basic serving of one or more models (the default option).
    
*   *Flow*: A multi-stage pipeline (DAG - directed acyclic graph) with built-in or custom steps (for example, API integrations, data enrichment and processing, model serving, routing, storage, and so on).
    

MLRun contains built-in serving classes for the major ML/DL frameworks ([Scikit-Learn](https://scikit-learn.org/), [TensorFlow](https://www.tensorflow.org/), [ONNX](https://onnx.ai/), [XGBoost](https://xgboost.ai/), [LightGBM](https://github.com/microsoft/LightGBM), [PyTorch](https://pytorch.org/), and [Hugging Face](https://huggingface.co/)) and supports a standard serving protocol (like KServe, Seldon, and Triton). In addition, MLRun provides a few container images with the required ML/DL packages pre-installed, or you can choose a base image and additional package requirements, which will automatically build the desired image for you.

In MLRun, the first step is to define a function object that specifies the code, packages, resources, triggers, and so on. Then you define the serving topology (graph). Once the serving function is fully defined, you can simulate it locally or deploy it to a cluster using a single API call.

[Example 6-8](#ch06.xhtml#mlrun-serving-basic) demonstrates the usage of MLRun serving. First, define a function using a standard image, add a model, and then simulate and debug the serving pipeline locally with test data. Finally, deploy the function to the cluster and test the live endpoint (using `.invoke()`).

##### Example 6-8. Define and deploy a basic MLRun serving topology

```
serving_fn = mlrun.new_function("serving", image="mlrun/mlrun",
                                kind="serving", requirements=[])

# Add a model object or file (can be in S3, GCS, local file, etc.)
serving_fn.add_model(
    "my-model",
    model_path=model_uri,
    class_name="mlrun.frameworks.sklearn.SklearnModelServer")

# Create a mock server (simulator) and test/debug the endpoint
server = serving_fn.to_mock_server()
sample = {"inputs": [[5.1, 3.5, 1.4, 0.2], [7.7, 3.8, 6.7, 2.2]]}
server.test(path=f"/v2/models/my-model/infer", body=sample)

# Result:
{'id': '2b2e1703f98846b386965ce834a6c4ab',
 'model_name': 'my-model',
 'outputs': [0, 2]}

# Deploy the serving function to the cluster
project.deploy_function(serving_fn)
```

```
> 2023-02-14 12:29:12,008 [info] Starting remote function deploy
> 2023-02-14 12:29:12  (info) Deploying function
> 2023-02-14 12:29:12  (info) Building
> 2023-02-14 12:29:12  (info) Staging files and preparing base images
> 2023-02-14 12:29:12  (info) Building processor image
> 2023-02-14 12:29:57  (info) Build complete
> 2023-02-14 12:30:05  (info) Function deploy complete
> 2023-02-14 12:30:05,918 [info] successfully deployed function
```

```
# Send prediction request to the live endpoint
serving_fn.invoke(path=f"/v2/models/my-model/infer", body=sample)
```

The real power of MLRun serving graphs is the ability to develop and deploy complex distributed AI applications rapidly while ensuring maximum performance, scalability, availability, and security. [Example 6-9](#ch06.xhtml#mlrun-serving-nlp) shows an example of a multi-stage NLP application with data pre/post-processing. You can extend the serving graphs to include branching and parallelism. You can also add advanced data processing steps, model ensembles, exception handling, custom monitoring, and more.

##### Example 6-9. Define and deploy a multi-stage serving graph topology

```
# Create an MLRun serving function from custom code
serving_function = mlrun.code_to_function(
    filename="src/serving.py",
    kind="serving",
    image="mlrun/mlrun",
    requirements=[],
)

# Set the serving topology
graph = serving_function.set_topology("flow", engine="async")

# Define a 3 step graph (preprocess -> hugging face model -> postprocess)
# the custom preprocess and postprocess functions are in serving.py
# while the HuggingFaceModelServer is a built-in MLRun class
graph.to(handler="preprocess", name="preprocess")\
     .to(mlrun.frameworks.huggingface.HuggingFaceModelServer(
              name="sentiment-analysis",
              task="sentiment-analysis",
              model_name="distilbert-base-uncased",
              model_class="AutoModelForSequenceClassification",
              tokenizer_name="distilbert-base-uncased",
              tokenizer_class="AutoTokenizer"))\
     .to(handler="postprocess", name="postprocess").respond()

# Plot to graph:
serving_function.plot(rankdir='LR')
```

![](assets/mlrun-huggingface-graph.png)

```
# Deploy the pipeline
project.deploy_function(serving_function)

# Send a text request and get the sentiment results
response = serving_function.invoke(path='/predict', body="good morning")
print(response)

# Result:
['The sentiment is POSITIVE', 'The prediction score is 0.7876932144165039']
```

MLRun serving provides a rich user interface that natively integrates with the platform’s other elements (see [Figure 6-5](#ch06.xhtml#mlrun-model-endpoints-ui)).

![](assets/mlrun-model-endpoints-ui.png)

In summary, MLRun serving extends the notion of model serving to rapid delivery of application pipelines and accelerating the deployment of AI applications. In addition, its serverless architecture reduces infrastructure costs and engineering overhead and enables continuous operations.

# Advanced Serving and Application Pipelines

The previous sections explained the need to transition from looking at the model endpoint as the production end goal to thinking about AI applications. When you build applications, you must address API integrations, data enrichment, validations, processing, and storage. In addition, the same application often requires routing, cascading, or merging results from multiple models and issuing one or more actions. Finally, you must monitor every aspect, including resource usage, data, model performance, and application KPIs. Therefore, you have to define the deployment goals around application pipeline design, implementation, and maintenance.

In the model development flow, the job execution time or frequency may not be critical. However, in production, applications may need to scale to serve thousands of requests and terabytes of data. Sometimes, the client is waiting for an immediate answer, which requires more focus on performance and latency. Therefore, enabling parallelism and considering technologies that optimize the data pipeline and model performance are necessary.

You will likely need to upgrade the model or enhance the application pipeline at a certain point. However, upgrades are not trivial when the application serves online clients or critical business services. In addition, new models may behave differently in production. As a result, you should first test them in isolation or expose only a fraction of the clients to the latest version before making the change available to everyone. Production deployment should include a strategy and implementation for live upgrades, A/B testing, failure recovery, and rollbacks.

## Implementing Scalable Application Pipelines

Serving application pipelines executes a set of activities. For example, intercepting an event, enriching and processing the data, using one or more models for prediction, and returning a response or issuing an action. The activities can run sequentially (one after the other), in parallel, or combine sequential and parallel activities. Pipelines can be synchronous (the client waits for the response) or asynchronous (the client does not wait).

A simple sequential implementation uses a single process that calls the different activities one after the other. For example, [Example 6-10](#ch06.xhtml#fastapi-pipeline) demonstrates a sequential application pipeline using FastAPI with the following steps:

1.  Reading and enriching the incoming request.
    
2.  Data pre-processing to generate a feature vector.
    
3.  Model prediction.
    
4.  Processing the model results and returning a response to the client.
    

##### Example 6-10. Sequential application pipeline example using FastAPI

```
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel

app = FastAPI()

# Define the prediction request data (json) structure
class PredictRequest(BaseModel):
    user: str
    # ...


# API to get model endpoint status
@app.get("/")
async def get_status():
    return {"model": "my-model", "version": 1.0, "status": "ok"}


# API to process data and make a prediction
@app.post("/predict")
async def predict(req: PredictRequest):

    enriched_data = enrich_user(req)

    data = pre_process(enriched_data)

    prediction = model_predict(data)

    return post_process(prediction, req)


def enrich_user(req: PredictRequest):
    ...
```

Suppose you want to distribute the work to multiple microservices or avoid package dependencies. In that case, the primary process can call the activities (implemented through separate microservices) utilizing REST API calls. However, the distributed architecture will require you to handle additional complexities, such as partial failures, retries, service authentication, and rolling upgrades across multiple microservices.

###### Note

In both local and distributed architectures, the flow remains sequential and synchronous, which can lead to slower performance. Performance can be improved when adding parallelism. For example, the primary process can use threads or *async* to execute multiple activities in parallel. However, this adds more complexity to the code.

A way to achieve distributed processing, parallelism, and simplicity is to use asynchronous or streaming pipeline frameworks where you define a graph (DAG) of activities. Then, the framework executes, scales, and tracks the activities using distributed computing resources. In addition, the application pipeline is monitored, deployed, and upgraded as one managed service.

[Figure 6-6](#ch06.xhtml#application-pipelines-arch) illustrates a few application pipeline architecture options:

1.  Sequential activities in the same process.
    
2.  Combining sequential and parallel activities using a few processes.
    
3.  An asynchronous streaming pipeline.
    

![](assets/application-pipelines-arch.png)

There are multiple commercial and open-source distributed pipeline frameworks in the industry. Some examples covered here are [AWS Step Functions](https://aws.amazon.com/step-functions/), [Apache Beam](https://beam.apache.org/), and [MLRun Serving Graphs](https://docs.mlrun.org/en/stable/serving/serving-graph.xhtml).

### AWS Step Functions

AWS Step Functions is a workflow service that executes a state machine of individual steps (as shown in [Figure 6-7](#ch06.xhtml#step-functions-ui)). Steps can invoke a serverless Lambda function or call AWS services. The Step Functions service controls the execution of the workflows and its graphical console shows your application’s workflow as a series of event-driven steps.

Step Functions has two workflow types. Standard workflows are ideal for long-running, auditable workflows, as they show execution history and visual debugging. Express workflows suit high-event-rate workloads like streaming data processing and IoT data ingestion.

The Express workflows can be either synchronous (wait until the workflow completes and then return the result) or asynchronous (don’t wait for the workflow to complete).

![](assets/step-functions-ui.png)

While AWS Step Functions is feature-rich, can dynamically scale resources, and has an excellent user interface, it adds the complexity of creating a new Lambda function for every step as well as performance overhead of networking and data serialization between each step.

To build an ML application pipeline with data preparation and model serving steps, you must first create serverless Lambda functions for each step. For example, [Example 6-11](#ch06.xhtml#step-funcs-predict) demonstrates a TensorFlow model serving Lambda function.

##### Example 6-11. Serverless prediction function using AWS Lambda

```
import json
import boto3
import tensorflow.keras.models as models

s3 = boto3.client('s3')

# Load the Keras model from S3 during initialization
model_path = '<your-s3-bucket>/path/to/model.h5'
response = s3.get_object(Bucket='<your-s3-bucket>', Key=model_path)
model_bytes = response['Body'].read()
model = models.load_model(model_bytes)

def lambda_handler(event, context):
    # Load input data from event
    input_data = event['input_data']

    # Make predictions using the preloaded model
    predictions = model.predict(input_data)

    # Return predictions as JSON
    return json.dumps(predictions.tolist())
```

Once you have all the functions, you can define the multi-stage workflow. [Example 6-12](#ch06.xhtml#step-funcs-workflow) demonstrates a simple asynchronous pipeline with data preparation, model prediction, and post-processing.

##### Example 6-12. AWS Step Functions workflow example

```
from stepfunctions.steps import LambdaStep, PassStep, Chain
from stepfunctions.workflow import Workflow
from stepfunctions.inputs import ExecutionInput

def create_workflow(input_data):
    # Define the workflow using the stepfunctions library
    with Workflow('MyWorkflow') as workflow:
        # Define the Lambda function to preprocess the data
        preprocess_data_step = LambdaStep(
            'PreprocessData',
            parameters={
                'FunctionName': '<function-arn>',
                'Payload': ExecutionInput(input=input_data)
            }
        )

        # Define the Lambda function to load and run the model
        run_model_step = LambdaStep(
            'RunModel',
            parameters={
                'FunctionName': '<function-arn>',
                'Payload': ExecutionInput(
                    input=preprocess_data_step.output()['Payload'])
            }
        )

        # Data post processing
        post_process_step = LambdaStep(
            'PostProcess',
            parameters={
                'FunctionName': '<function-arn>',
                'Payload': ExecutionInput(input=run_model_step.output()['Payload'])
            }
        )

    # Start the execution of the workflow
    execution = workflow.execute(inputs={'input_data': input_data})
    return execution.execution_arn

# Example usage
input_data = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
execution_arn = create_workflow(input_data)
```

Step Functions does not support streaming protocols such as Kafka or Kinesis. However, you can create a front-end function that will read from a stream and invoke the workflow. See [Example 6-13](#ch06.xhtml#step-funcs-stream), an example Lambda function.

##### Example 6-13. AWS Lambda function that reads events from a stream and executes the workflow

```
import os, base64, boto3

sf = boto3.client("stepfunctions", os.environ['REGION'])

def lambda_handler(event, context):
    # Execute the workflow on every stream message
    for record in event['Records']:
        payload = base64.b64decode(record['kinesis']['data'])
        sf.start_execution(
            stateMachineArn=os.environ['WORKFLOW_ARN'],
            input=payload,
        )
```

### Apache Beam

Apache Beam is an open-source stream processing framework, focused on online structured data processing. ([Google Dataflow](https://cloud.google.com/dataflow) is a managed version of Apache Beam).

Beam lets you build an asynchronous pipeline (DAG) consisting of multiple steps. The steps can use built-in data, IO, and computing or user-defined functions. Beam pipelines can be executed locally using the [DirectRunner](https://beam.apache.org/documentation/runners/direct/) or deployed to distributed runners such as [Apache Spark](https://spark.apache.org/), [Flink](https://flink.apache.org/), and [Google Dataflow](https://cloud.google.com/dataflow).

Beam’s advantages are that it is open-source, scalable, and contains powerful data operators (such as calculating aggregations over a time window). However, it does not have the flexibility and control over the underline resources and packages per step as with AWS Step Functions.

[Example 6-14](#ch06.xhtml#apache-beam-pipe) defines a three-step pipeline: reading JSON data; pre-processing, prediction, and serializing the output; and writing it to a Kafka stream.

##### Example 6-14. Apache Beam pipeline with data processing and prediction

```
import apache_beam as beam
from tensorflow.keras.models import load_model
import json

# Define a custom DoFn that parses JSON strings
class ParseJsonFn(beam.DoFn):
    def process(self, element):
        yield json.loads(element)

# Define a custom DoFn that serializes Python dictionaries to JSON strings
class SerializeJsonFn(beam.DoFn):
    def process(self, element):
        yield json.dumps(element).encode('utf-8')

# Define a custom DoFn to make model predictions
class MakePredictions(beam.DoFn):
    def __init__(self, model_path):
        self.model_path = model_path
        self.model = None

    def setup(self):
        self.model = load_model(self.model_path)

    def process(self, element):
        # Make a prediction using the loaded model
        prediction = self.model.predict(element)

        # Return the prediction
        return [prediction]

# Define the pipeline options
options = beam.options.pipeline_options.PipelineOptions()
options.view_as(beam.options.pipeline_options.StandardOptions).runner = 'DirectRunner'

# Create the pipeline
with beam.Pipeline(options=options) as p:
    # Read data from a JSON file or message
    messages = p | "Read JSON" >> beam.io.ReadFromText("path/to/json_message.json")

    # Parse JSON strings into Python dictionaries
    parsed_messages = messages | "Parse JSON" >> beam.ParDo(ParseJsonFn())

    # Make model predictions using the custom DoFn
    predictions = parsed_messages | 'MakePredictions' \
        >> beam.ParDo(MakePredictions(model_path='my_model.h5'))

    # Print the predictions
    predictions | 'PrintPredictions' >> beam.Map(print)

    # Serialize the processed messages to JSON strings
    serialized_messages = predictions | "Serialize JSON" \
        >> beam.ParDo(SerializeJsonFn())

    # Write the output to a Kafka topic
    serialized_messages | "Write to Kafka" >> beam.io.WriteToKafka(
        producer_config={'bootstrap.servers': 'kafka-host:9092'},
        topic='my-topic'
    )
```

### MLRun Serving Graphs

Many solutions that are used to build application pipelines, started as batch or stream processing for structured data (Apache Beam, Flink, [Storm](https://storm.apache.org/), [Airflow](https://airflow.apache.org/), [Spark Streaming](https://spark.apache.org/docs/latest/streaming-programming-guide.xhtml), and so on). Therefore, it’s more complicated to expand it to the model serving and monitoring applications or to handle unstructured data like text, video, and other. Other solutions like AWS Step Functions have emerged as a generic way to chain microservices and require more customization and logic to handle data processing and model serving at scale.

MLRun Serving Graph combines the benefits of AWS Step Functions as a versatile serverless function-based pipeline (using Nuclio real-time serverless functions) with the parallel data processing capabilities of Apache Beam and an easy way to build and debug scalable pipelines. In addition, it adds machine learning and [deep learning](https://www.iguazio.com/glossary/deep-learning-pipelines/) functionality and built-in components (steps).

With MLRun Serving Graph, you build a DAG with sources, intermediate steps (tasks), routers, queues, and data targets:

*   *Sources*: Real-time (HTTP endpoint, Kafka, Kinesis, and so on) or offline (for example, read data periodically from a file/object/database) data or event inputs.
    
*   *Steps*: Run a function, class handler, or REST API call. MLRun has a list of pre-built steps, including data manipulation, readers, writers, and model serving. You can also write your steps using standard or custom Python functions/classes.
    
*   *Routers*: A special type of step with routing logic and multiple children. The routing logic defines how the data/events are passed to and collected from the child steps. For example, the basic router class passes the event to a specific child based on the event content or metadata. The Parallel router passes the event to *all* the children and merges the results. The Ensemble router is a derivative of the Parallel router, which can intelligently combine the results from multiple child models into one aggregate result. Users can create their own routers and use custom logic.
    
*   *Queue*: Represents a queue or stream that accepts data from one or more source steps and publishes to one or more output steps. Queues are best used to connect independent functions/containers. Queues can run in memory or be implemented using a stream, which allows them to span processes/containers.
    
*   *Targets*: Online or offline storage (streams, files, databases, and so on).
    

Like Apache Beam or AWS Step Functions, every step in the DAG accepts an event object, manipulates it, and passes event(s) downstream. In the case of MLRun, there is a long list of built-in flow control, parallel data processing, and ML/DL steps ([see documentation](https://docs.mlrun.org/en/latest/serving/available-steps.xhtml)), such as filtering, mapping, flattening, micro-batching events, aggregating, joining, encoding, imputing, model serving, model ensembles, and so on. The final result can be written out asynchronously to some destination (file, DB, stream, and others) or returned immediately to the caller (synchronously) by marking the result step (responder).

Once the user defines the serving graph (DAG), they can test and debug it using the built-in simulator (mock server) and deploy it to production over one or more serverless functions with a single command.

[Example 6-15](#ch06.xhtml#mlrun-app-pipeline-flow) demonstrates a multi-stage pipeline with data pre-processing, feature enrichment (using MLRun’s [feature store](https://www.iguazio.com/glossary/feature-store/)), an ensemble of three models (returning the average result between the three models), and a post-processing step. The function pipeline code contains two custom pre/post-processing steps (implemented in  `_func.py_`) and the built-in `_EnrichmentVotingEnsemble_` router class.

##### Example 6-15. MLRun Serving Graphs with pre/post-ing and a three leg ensemble

```
# Define a serverless serving graph function
function = mlrun.code_to_function("app-pipe", kind="serving",
                                  image="mlrun/mlrun",
                                  requirements='requirements.txt')

# Define the graph topology and start with the pre-process step
graph = function.set_topology("flow")

pre_process = graph.to(handler="pre_process", name="pre-process")

# add an EnrichmentVotingEnsemble router with 3 child models (routes)
# The input data will be enriched with feature store features
# nil values will be imputed (with stats from the feature vector metadata)
router = pre_process.to(mlrun.serving.routers.EnrichmentVotingEnsemble(
    name='VotingEnsemble',feature_vector_uri="my-vector", impute_policy={"*": "$mean"}
))
for i, model in enumerate(models):
    router.add_route(f"model{i}", model_path=model.uri)

# Add the post-process step (after the router step)
router.to(handler="post_process", name="post-process")

# plot the graph topology (using Graphviz)
graph.plot(rankdir='LR')
```

Once the function is defined, the pipeline can be simulated by creating and using the mock server. It can then be deployed into production microservices using the `_deploy_function()_` method. See [Example 6-16](#ch06.xhtml#mlrun-app-pipeline-deploy).

##### Example 6-16. Test and deploy the pipeline

```
# Create a mock server (simulator) and test the graph with the test data
server = function.to_mock_server()
resp = server.test("/v2/models/infer", body={"inputs": test_data})

# Deploy the graph as a real-time serverless function
project.deploy_function(function)

# Invoke the remote function using the test data
resp = function.invoke("/v2/models/infer", body={"inputs": test_data})
```

MLRun simplifies the migration to production. The observability and model monitoring functionalities are built-in. Therefore, there is no need to pile on additional code to collect and report metrics (only to turn on the tracking feature). In addition, users can also define and report custom real-time metrics, which will be collected and shown in the monitoring dashboards, or report errors, which will be centrally logged.

MLRun Serving Graphs provide flexible configuration of pipeline steps, breaking into the underlining auto-scaling and real-time Nuclio serverless functions, while gaining the best scalability with optimal costs. For example, as illustrated in [Figure 6-8](#ch06.xhtml#mlrun-serving-to-funcs) you can specify which steps run on the same microservice (thus eliminating network and serialization overhead) and which ones must spread across microservices (for allowing the use of different OSs/software packages or resources like [GPUs](https://www.iguazio.com/glossary/gpu-for-machine-learning/) per the steps).

![](assets/mlrun-serving-to-funcs.png)

## Model Routing and Ensembles

The basic model serving implementation loads a model into memory and makes a prediction every time a request arrives. However, when you serve multiple models, you may need more advanced topologies to optimize costs, to deliver better results by combining various models, or to dynamically shift traffic from one model to another.

Deploying one microservice per function can be expensive, especially if you don’t call the models frequently or of you are using costly compute instances with GPUs or a large memory. In such cases, you may prefer to implement a single microservice function that hosts multiple models in memory and routes the request to a specific model based on the URL or on elements in the request body. (For example, select a country-specific model based on the country code in the request). Furthermore, you can dynamically load and unload models into memory based on their usage (in other words, implement a caching mechanism) to reduce memory consumption.

While model caching solutions can reduce the costs and memory consumption, they can also add delay to the first (non-cached) request. You should only use caching when you are not sensitive to the first request latency.

Different models can come with different software package dependencies or resource requirements (for example, they need a GPU). In such cases, you should deploy models using separate container microservices, each with a specific package and resource requirements, or group models into containers based on those requirements and route the traffic to the particular microservice that hosts the relevant model.

Collocating data processing with models in the same microservice can help increase performance by keeping the different transformations and prediction activities in memory vs. writing and reading into storage between steps. However, this does not allow fine-grain scaling (scaling a specific task in the pipeline) and requires aggregating the resource and package requirements. Therefore, consider the tradeoff between the two approaches based on your needs. MLRun Serving Graphs provide a simple way to specify which steps are collocated in the same container and separated into individual containers.

A common mechanism to improve the accuracy of a model is to combine different models that were trained using different datasets, algorithms, or parameters. For example, train one model with seasonal data (transactions done in the same period of the year) and another with temporal data (recent transactions) and combine the results from both models to preserve the seasonal and temporal effects on the scoring result. Another example is combining machine learning and deep learning models and returning the average result.

Model Ensemble is a technique in machine learning where multiple models are trained and their predictions are combined to make a final prediction and, in this way, improve the overall model performance.

There are two common approaches to creating an ensemble:

*   *Bagging*: Training multiple models independently on different subsets of the training data and then combining their predictions using some aggregation technique, such as averaging or majority voting. Use Bagging to reduce the variance of the predictions.
    
*   *Boosting*: Training multiple models sequentially, where each model is trained to correct the errors of the previous model. Use Boosting to reduce the bias of the predictions.
    

###### Tip

It is essential to monitor the performance of each sub-model in an ensemble, and this way, understand which approach yields better accuracy. In some cases, you may want to control the weights between models in an ensemble based on the individual performance. (For example, favor the temporal model due to the higher impact of recent events).

The Multi-Armed Bandit (MAB) algorithms (such as epsilon-greedy, UCB, or Thompson sampling) can be used to adaptively select the best-performing model or a combination of models over time. MAB algorithms can assign a score to each model, which reflects its expected performance and uncertainty. The system can then direct more inference requests to the top-performing models while continuing to explore other models and updating their scores.

## Model Optimization and ONNX

Some models, especially deep learning models, can consume significant computation resources. Using optimization techniques, you can reduce resource consumption and improve performance. Some examples of optimizations include the following:

*   *Feature reduction*: Reducing the size of the feature vector by removing features that do not add significant value to the result.
    
*   *Code optimization*: Moving critical sections of the code to faster binary code implementations (in C, C++, Go, Rust, and so on).
    
*   *Hardware acceleration*: Using GPUs, TPUs, or FPGAs can significantly improve inference performance by offloading computations from the CPU to specialized hardware that is optimized for matrix multiplication.
    
*   *Quantization*: Reducing the precision of the model’s weights and activations, typically from 32-bit floating-point to 8-bit integer precision.
    
*   *Pruning*: Removing some of the model’s weights or neurons that have little impact on the model’s accuracy.
    
*   *Model compression*: Reducing the size of the model by compressing its weight. Use techniques like weight sharing, low-rank factorization, or Huffman coding.
    

Several frameworks can optimize models:

*   *ONNX Runtime*: A high-performance engine for executing ONNX models. It provides hardware acceleration support for CPUs, GPUs, and FPGAs, and supports model compression techniques, such as quantization and pruning.
    
*   *Intel OpenVINO*: A toolkit for optimizing and deploying machine learning models on Intel hardware, including CPUs, GPUs, and FPGAs. It supports model compression techniques, such as quantization and pruning, and provides optimized libraries for deep learning operations.
    
*   *NVIDIA TensorRT*: A high-performance inference engine for NVIDIA GPUs that supports model compression techniques, such as pruning and quantization. It provides hardware acceleration support for NVIDIA GPUs and includes optimized libraries for deep learning operations.
    

[ONNX](https://onnx.ai/) (Open Neural Network Exchange) is an open format built to represent machine learning models. ONNX defines a common set of operators - the building blocks of machine learning and deep learning models - and a standard file format to enable AI developers to use models with various frameworks, tools, runtimes, and compilers.

The ONNX runtimes run in machine native (binary) code and supports model compression techniques and hardware-specific optimizations, which deliver significantly faster inference performance.

# Data and Model Monitoring

Monitoring solutions can be broken into three main layers:

1.  *Resource monitoring*: Monitoring the resources (CPUs, GPUs, memory, storage) used by the the ML application, as well as their health and the service’s availability.
    
2.  *Model and data monitoring*: Monitoring the performance of the model and the data used by the model (accuracy, drift, bias, data quality, and so on).
    
3.  *Application monitoring*: Monitoring the overall application performance (throughput, latency, errors, and so on) across all pipeline steps and measuring the business KPIs defined for the application.
    

You can use the same solution to monitor all three layers or different services per layer. In any case, it is essential to correlate the information across layers (using tags and labels) since resource or model performance problems will usually impact the higher layers of the application.

Cluster monitoring solutions can monitor resources. For example, in [Kubernetes](https://www.iguazio.com/glossary/kubernetes-for-mlops/), the typical answer is to use [Prometheus](https://prometheus.io/) and [Grafana](https://grafana.com/) to track the microservice resources. But first, you need to determine which model or application is served by which container (and this can change dynamically). Therefore, when deploying the models as Kubernetes resources (containers, pods, and so on), you should label them with the model and application information.

If you use public clouds, you can use managed cloud services for resource monitoring, such as [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/), [Azure Monitor](https://learn.microsoft.com/en-us/azure/azure-monitor/), [Google Cloud Monitoring](https://cloud.google.com/monitoring), [Datadog](https://www.datadoghq.com/), [New Relic](https://newrelic.com/), and others.

Model, data, and application performance metrics can also be reported in the traditional resource and application monitoring solutions. However, the real challenge is to collect this information and reference metadata and to keep it cost-effective and scalable, given the enormous volumes of data collected.

Model and data monitoring solutions have unique challenges; they compare the data and model performance in production with reference data collected at the model development and training phases to calculate the accuracy, drift, and so on. In addition, data is collected for every model request and must be stored for real-time dashboards and offline access, while traditional resource and service monitoring stores only sampled metrics. Furthermore, there are no one-to-one relations between the microservice and the model, and the same container can host multiple models, ensembles, and so on. Finally, model and data monitoring solutions must also work for batch workloads, which are still a dominant place where models are used.

Most model monitoring solutions are limited to structured (tabular) data and do not support unstructured data (text, images, videos, and so on). However, you can address unstructured data by creating a transformation from unstructured data to tabular data and monitoring the results. For example, convert an image to numeric RGB values or detected object metrics.

Application monitoring spans the different stages of the ML application (data enrichment, preparation, model prediction, actions, and so on). It looks into application-level metrics, such as overall requests latency and throughput, application errors, application-level metrics, business metrics, KPIs, and others.

Multiple versions of the same application pipeline may run in parallel (for example, in the case of A/B testing). Therefore, you want to compare the application KPIs across versions, not just the model performance, since a better model does not necessarily reflect on a better KPI. For example, if the model does not respond at the right time or if the actions following the prediction do not generate the right impact.

Considering application-level monitoring ahead of the design and implementation would be best since it requires custom instrumentation in multiple application junctions and ways to collect and use reference data for KPI performance measurements.

Monitoring results are shown in dashboards, but they can bring more significant value when they trigger alerts and corrective actions. For example, model drift indication can start a retraining flow, change the weights in a model ensemble, or send critical notifications to administrators to correct the problem. Therefore, the solution should provide a mechanism to easily define conditions, thresholds, and actions.

There are two types of solutions for model monitoring which will be described in the following sections:

*   *Integrated*: An integral part of a data science or MLOps platform. It usually has fewer features but does not require glue logic and separated management.
    
*   *Standalone*: Dedicated monitoring solutions are usually feature-rich, but require manual integrations and separate management.
    

## Integrated Model Monitoring Solutions

Data science or MLops platforms support the tasks required to develop and deploy models. When you deploy the model, the platform often provides basic model monitoring, which can be operated with minimal configuration. Integrated monitoring solutions show the model endpoints with essential performance and health information. Most platforms support drift detection and few support additional monitoring classes.

**AWS SageMaker**

[AWS SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor.xhtml) supports data and model drift detection (see the architecture in [Figure 6-9](#ch06.xhtml#sagemaker-model-monitor-architecture)). The model endpoints capture incoming requests and model results into S3 objects and compare them with baseline and ground truth datasets to calculate the drift and accuracy metrics.

![](assets/sagemaker-model-monitor-architecture.png)

In SageMaker, you manually generate and upload the reference datasets into S3 and then define a scheduled model monitoring job (see [example](https://sagemaker-examples.readthedocs.io/en/latest/sagemaker_model_monitor/introduction/SageMaker-ModelMonitoring.xhtml)).

You can view the model metrics and drift indications in the SageMaker UI (see [Figure 6-10](#ch06.xhtml#sagemaker-model-monitor-ui)).

![](assets/sagemaker-model-monitor-ui.png)

**Google Vertex AI**

Google [model monitoring](https://cloud.google.com/vertex-ai/docs/model-monitoring/overview) supports tracking model requests and results into a BigQuery table and can issue email alerts when the specified threshold is crossed. [Figure 6-11](#ch06.xhtml#vertex-ai-monitoring) demonstrates how the user enables the monitoring, sets the monitoring policy, and uploads the reference data (schema and statistics).

![](assets/vertex-ai-monitoring.png)

The essential model endpoint metrics and drift information are visualized in the UI (see [Figure 6-12](#ch06.xhtml#vertex-ai-endpoints) and [Figure 6-13](#ch06.xhtml#vertex-ai-endpoints-skew)) and you can access the complete data through BigQuery.

![](assets/vertex-ai-endpoints.png)![](assets/vertex-ai-endpoints-skew.png)

**MLRun Model Monitoring**

MLRun open-source MLOps includes an integrated [monitoring service](https://docs.mlrun.org/en/latest/concepts/monitoring.xhtml) for batch and real-time workloads. As shown in [Figure 6-14](#ch06.xhtml#mlrun-monitoring-arch), MLRun model serving endpoints write the performance, inputs, outputs, and user-defined metrics into a stream. Then, a real-time serverless Nuclio function reads and processes the data and writes the results into different types of storage (key/value, time-series database, and parquet files). Scheduled MLRun jobs run periodically, read the data, calculate various metrics (drift, accuracy, and so on), and trigger appropriate alerts.

![](assets/mlrun-monitoring-arch.png)

You can view the model endpoint information in the MLRun UI (see [Figure 6-15](#ch06.xhtml#mlrun-model-endpoints-histogram)), or in Grafana (see [Figure 6-16](#ch06.xhtml#mlrun-monitoring-grafana)) and the production datasets are stored in MLRun’s feature store and can be used for post-production analytics (for example, analyzing data quality, bias, and explainability) or used for retraining a model.

![](assets/mlrun-model-endpoints-histogram.png)![](assets/mlrun-monitoring-grafana.png)

MLRun eliminates many engineering efforts by automatically generating reference and production datasets, managing the data assets and lifecycle, scheduling and scaling monitoring tasks, and more.

When you train the models in MLRun, the reference data (schema, statistics, and so on) is auto-generated and saved with the model. You can also update the reference data manually through the API. To operate the monitoring functionality, you should apply the `_set_tracking()_` option in the serving function.

MLRun supports monitoring plug-ins and has extensibility to support advanced monitoring applications for structured and unstructured data.

## Standalone Model Monitoring Solutions

There are several solutions dedicated to model monitoring. They usually have more advanced features and user interfaces than the integrated options, but require manual integration with data assets, serving, and training frameworks. In addition, they require working with multiple management consoles.

Examples for commercial offerings include [Aporia](https://www.aporia.com/), [Arize](https://arize.com/), [Whylabs](https://whylabs.ai/), and [Mona Labs](https://www.monalabs.io/).

The commercial frameworks usually support multiple monitoring applications (drift, accuracy, data quality, and so on), friendly and rich user interfaces, advanced policies, and multiple alerting and triggering options (email, Slack, webhooks, and more).

You can see an example Aporia UI wizard for creating a new monitoring task in [Figure 6-17](#ch06.xhtml#aporia-wizard).

![](assets/aporia-wizard.png)

The challenge is uploading the prediction and reference data to the storage and managing the data lifecycle, rather than getting it out of the box in integrated platforms such as MLRun. See the code example in [Example 6-17](#ch06.xhtml#aporia-upload-data) for uploading batch prediction data. (Real-time prediction data requires an additional step of conversion from a stream to parquet files).

##### Example 6-17. Write prediction inputs and outputs into a parquet file (source Aporia)

```
import fastparquet

# Preprocess & predict
X = preprocess(...)
y = model.predict(X_pred)

# Concatenate features, predictions and any other metadata
df = ...

# Store predictions
fastparquet.write(
    filename=f"s3://my-models/{MODEL_ID}/{MODEL_VERSION}/serving.parquet",
    data=df,
    append=True,
)
```

An example of an Aporia monitoring dashboard is shown in [Figure 6-18](#ch06.xhtml#aporia-monitor).

![](assets/aporia-monitor.png)

A popular open-source model monitoring framework is [Evidently](https://www.evidentlyai.com/), which monitors drift and data quality. Evidently compares the reference dataset with the prediction dataset and generates beautiful static HTML reports. In addition, it can write the resulting metrics into Prometheus and show them in pre-designed Grafana dashboards.

[Example 6-18](#ch06.xhtml#evidently-code) demonstrates how you generate a report that compares the reference data with the prediction inputs and outputs data (`current_data`). In [Figure 6-19](#ch06.xhtml#evidently-reports) you can see report examples.

##### Example 6-18. Generating Evidently report

```
report = Report(metrics=[
    DataDriftPreset(),
])

report.run(reference_data=reference, current_data=current)
report.save_html("file.xhtml")
```

![](assets/evidently-reports.png)

As you can see, the responsibility for generating the reference and production datasets and generating the report is on the user, who needs to add additional code for monitoring, data generation, and lifecycle management. Therefore, Evidently is an excellent interactive development and comparison solution, and you can extend it manually to handle batch prediction workloads. However, it is unsuitable for continuous batch or real-time model serving and lacks an interactive UI portal, central management, alerts, security, and more.

# Model Retraining

Covid-19 significantly and abruptly changed human behavior across the globe. But the pandemic did not only significantly impact human lives, it also disrupted ML models. Data engineers woke up to find that their ML models, which were trained on pre-pandemic data sets, had suddenly drifted and were not delivering reliable results.

The models’ performance degraded because the pre-Covid data was not reflecting current behaviors and therefore it was no longer relevant or accurate. These models then had to be retrained to ensure their validation and efficacy for the pandemic era.

While Covid is an extreme example, data keeps changing because people change and the world changes. This means the models that are trained on the outdated data lose relevance. Model retraining, also known as continuous training or continual training, is the act of training models again and again on updated data and then redeploying them to production.

By [retraining](https://www.iguazio.com/glossary/model-retraining/), data engineers can ensure the models are up-to-date, valid, and trustworthy. This ensures the predictions and the outputs of models are always accurate for the business use cases they were designed to answer. If models aren’t retrained, they will turn stale.

Accurate models are essential for business success. If an organization uses a model that provides inaccurate outputs, the result could be loss of customers and profit. For example, if a model is supposed to detect fraud but doesn’t do so accurately, this will mean either that fraudsters get away with fraud, costing the company its customers and perhaps a loss of millions in insurance claims, or that there will be too many false positives, resulting in frustrated end-users (who won’t be able to make online purchases) and adverse financial impact to the company’s customers (and again, losing customers).

Automating the process of model retraining makes it reliable and optimized. Automation also reduces the chance of manual errors or data engineers forgetting to retrain models. With automation, data engineers and data scientists can ensure their measurements are defensible and quantitative and that explainability tests are set up.

###### Tip

[Automated](https://www.iguazio.com/glossary/automated-machine-learning/) retraining should take place as part of an MLOps pipeline. It can be integrated as part of the [CI/CD](https://www.iguazio.com/glossary/ci-cd-for-machine-learning/) pipeline and may be triggered automatically by the model monitoring service upon drift detection.

## When to Retrain Your Models

There’s no right or wrong answer when deciding when to retrain (but not retraining is definitely the wrong answer). The answer depends on the business use case. However, the ultimate goal is to avoid drift.

There are two types of drift:

*   *Data drift*: When the statistical distribution of production data is different from the baseline data used to train or build the model. This happens when human behavior changes, training data was inaccurate, or there were data quality issues.
    
*   *Concept drift*: [When](https://www.iguazio.com/glossary/concept-drift/) the statistical properties of the target variable change over time. In other words, the concept, or the relations between the datasets, have drifted.
    

There are four main approaches for retraining:

1.  *Interval-based*: According to a certain schedule or repeating interval. For example, retraining every Sunday night or every end of the month. This ensures the models will always stay up-to-date since they are constantly retrained. However, this method could be costly since resources are used even when retraining is unnecessary.
    
2.  *Performance-based*: Retraining takes place when a predetermined threshold or baseline is crossed, which indicates model degradation and drift. This ensures the model can always answer the business use case. However, if the threshold is inaccurately determined or the data does not come in on time, the model could turn stale before the organization is aware and can retrain it.
    
3.  *Based on data changes*: This type of retraining takes place when there are new data sets or when code changes are made. Such retraining ensures adaptivity to engineering changes but might miss drift that degrades the model performance.
    
4.  *Manually on-demand*: This non-automated retraining method provides complete control for data scientists but is prone to errors and could mean retraining does not occur when needed.
    

## Strategies for Data Retraining

Model retraining takes place by lifting the training data into the retraining pipeline. This data includes features, labels, model parameters, and pipeline parameters.

The question of how much data should be used for retraining depends on the organization’s requirements and restrictions, which determine the strategy. Data amounts required for retraining can be determined through the following approaches:

*   *Fixed window*: A practical yet simplistic approach.
    
*   *Dynamic window*: Optimal for large datasets that are constantly updated but compute-intensive.
    
*   *Representative subsample selection*: Accurate since it’s similar to production data but time-consuming and resource-heavy.
    

Model retraining can be an online or offline event, with offline being the most popular approach. Offline retraining, also known as batch retraining, usually uses all available data or a considerable amount of existing data. It’s easy to do but requires more thought into the retraining strategy.

###### Tip

Online retraining is recommended for real-time streaming use cases. The data used for online retraining is new data, not samples already seen by the models. This makes online retraining more accurate and can help avoid drift, though it is also more costly.

To train models, you need labels (the target values), and labels usually arrive in a delay after the features. For example, a churn prediction application may have all the input features immediately. However, the target label (indicating if there was a churn) can come one month later. Therefore, the training set data window should only cover transactions with labels. In addition, since the model reacts to an expected churn, it may influence the dataset and require adaptations to the dataset, which will make it more balanced.

When the data is complex or unstructured, it is challenging to calculate the labels. In such cases, organizations use manual labeling. They extract a sample of the data, manually label it, compare it with the predicted results, and use the labeled dataset for model retraining and tuning.

Despite the importance of model retraining, it’s important to remember that it also comes at a cost. Model retraining requires resources for data storage, computing, adjusting your architecture for retraining, data professionals’ time, and more. Therefore, some organizations are hesitant about running it continuously. It is recommended to adjust your retraining schedule to your business requirements to ensure cost-effectiveness while maintaining model performance.

## Model Retraining in the MLOps Pipeline

Retraining is achieved by triggering the model development pipeline (data preparation, training, validation, and so on). The trigger can be initiated based on a scheduled event or after a drift indication (triggered by the model monitoring component). An important step is validating the model in a staging environment before deploying it to production. If the results are not as expected, then the models and pipelines need to be retrained.

After retraining, it is recommended to leave the old model running and deployed to production for a specific period of time or until the model has served a certain number of requests. By running these A/B tests you can identify which model performs better, by comparing its predictions with those of the other ones. Another approach is to use an ensemble with the old and new models and change the weight or remove the old model over time.

Finally, model retraining can also be used for training new models. This is called transfer learning. In transfer learning, existing models are reused to retrain new models. This is commonly used in deep learning since it saves resources by reusing models instead of rebuilding them.

# Deployment Strategies

In production, models and applications must always be ready to serve requests and they should not suffer from downtime due to version upgrades. In addition, a more advanced practice is gradually moving requests to the new version, while validating that performance and quality levels are met. Then, if the latest version fails or underperforms, you can roll back to the previous version.

To guarantee no service disruption, you can use a rolling upgrade deployment strategy. This strategy replaces the application version used by the service, one instance at a time. Rolling upgrades are supported out of the box in cloud-native platforms and Kubernetes. However, when you want to evaluate the candidate versions against the current version, you need two or more versions to coexist and serve requests, until you can determine that the new version meets your quality and performance goals. Or, you can use the other versions to serve as a baseline to compare performance with.

There are several standard model deployment and upgrade strategies (see [Figure 6-20](#ch06.xhtml#deployment-strategies)):

*   *A/B Testing*: The new model is deployed alongside the old model and the traffic is divided between the two. Then, the output of both models is monitored to determine which one performs best, and the best-performing model is promoted. This method is a good fit for most use cases. In some cases, you may deploy more than two models for A/B/C testing, A/B/C/D testing, and so on.
    
*   *Blue/Green*: Setting up two identical environments, dubbed *Blue* and *Green*, with one being the live Production environment and the other a Staging environment. The new model is deployed to the Staging environment. Once it operates as expected, Staging becomes Production, the old version is removed, and more recent models are deployed into Staging. This method is straightforward and relatively simple, but it incurs high operational costs since you have to operate two identical environments. This method is recommended for use cases that cannot afford any downtime, like fraud detection or e-commerce.
    
*   *Canary*: Incremental rollout of the new model to a percentage of users to validate efficacy, performance, and functionality. Once validated, the model is gradually rolled out to more users and finally to everyone. This strategy enables controlled rollout with no downtime. However, it is slow, and testing occurs in Production. Therefore, it is recommended for non-critical applications that obtain value from gradual testing in Production, like social media.
    
*   *Champion/Challenger (or Shadow)*: Deploying the Champion model to Production alongside one or more Challenger models. Traffic is routed to all models and the output of the Champion is used as the result, while the Challenger(s) output is only monitored. This strategy enables the highest form of model validation, yet it is also the most expensive.
    

![](assets/deployment-strategies.png)

When traffic is split across multiple models, you must ensure *affinity* (the same user is always routed to the same model and has the same experience for every request). For example, you can use a hash of the user ID to determine where to route the request.

Offline and batch application pipelines do not need to maintain running services or use rolling updates. They can select which version to use on every run. However, you may run two or more batch pipelines in parallel (in A/B or Champion/Challenger deployments), with each pipeline storing its results in different tables/files.

It’s important to remember that deployment is not a one-time process. Instead, deployment is a recurring process that occurs every time we retrain our models and want to ensure the freshest and high-performing models are being used. Therefore, selecting the right deployment strategy is important since you will use it repeatedly in your MLOps pipeline. As always, the strategy you choose depends on multiple factors. The primary considerations to take are:

*   Your business use case
    
*   Retraining and deployment frequency
    
*   Your model size
    
*   Whether you stream data in real-time or batches
    
*   How your model is impacted by drift
    
*   Your desired service uptime and quality
    

# Measuring the Business Impact

Measuring an application’s business impact is essential for determining the model performance. However, measuring it can be a complex task since this involves evaluating the effects of the model on various aspects of the business, such as revenue, customer satisfaction, and operational efficiency.

Here are some ways to measure the impact of ML models:

*   *Define success metrics*: Define measurable and trackable metrics that are aligned with business objectives. For example, model accuracy, precision and recall, the F1 score, Mean Absolute Error (MAE) and Root Mean Squared Error (RMSE), lift, conversion rate, and customer satisfaction.
    
*   *Establish a baseline*: Create a benchmark for measuring the impact of the model on the business. This should be determined before deploying the model to production.
    
*   *Conduct A/B testing*: Compare the business metrics (costs, revenue, number of active users, customer satisfaction, and so on) before and after the deployment of the ML model. This will help determine the incremental impact of the model.
    
*   *Conduct a cost-benefit analysis*: Determine the return on investment (ROI) of the ML model. This will help assess the financial impact of the model on the business. For example, how many new users registered, what is the incremental revenue from the new service, how many calls were saved by using a chatbot, and so on.
    
*   *Gather feedback*: Gather feedback from stakeholders, such as customers and employees, to assess the impact of the ML model on their experience. This will help determine the non-financial impact of the model on the business. You can use surveys, user testing, reviews and ratings, support ticket analysis, or NPS scores, among other methods.
    

Overall, measuring the business impact of ML models requires a comprehensive approach that considers both financial and non-financial metrics, as well as stakeholder feedback. By doing so, businesses can ensure that their ML models deliver tangible value and drive meaningful outcomes.

# Conclusion

In this chapter we finally delved into “the real thing”: building and deploying the application that uses the data and the model that drives decisions and action. We described the steps that will assist in delivering the ML application as a whole. By looking at this bigger picture, we can ensure there are no deficiencies in functionality, operational failures, avoidable risks, and prolonged delays.

This chapter reviewed model registries and model serving, while showing different solutions and how to use them. Then, we discussed advanced serving and application pipelines, which address requirements like API integrations, data enrichment, validations, processing, storage, routing, cascading, merging results from multiple models, and more. Following we explained about various monitoring solutions and how they operate and then retraining and deployment strategies and conditions. Finally, we discussed how to measure the business impact of deployment, which is the most essential step for determining the model performance.

# Critical Thinking Discussion Questions

*   What is the importance of model registries?
    
*   Choose a model serving solution and describe its capabilities. Could it be a good fit for your organization?
    
*   In which cases will you need to enhance or upgrade your application pipeline? Which considerations should you take into account?
    
*   What are the different retraining strategies? When would you choose each one?
    
*   Your manager does not see the need for measuring the business value of your models. Convince them otherwise.
    

# Exercises

*   Write the code for registering one of your models in the registry of your choice.
    
*   Choose an open-source serving solution and define a serverless serving graph function.
    
*   Choose an open-source model monitoring solution and configure it for your models (no need to actually connect to production).
    
*   Build an integration for retraining as part of a CI/CD pipeline.
    
*   Define 4 success metrics for one of your ML models.
    

# Chapter 7. Building Scalable Deep Learning and Large Language Model Projects

*Deep learning* (DL) is a machine learning subdomain inspired by the human brain’s structure and functioning. In deep learning, neural networks consist of interconnected layers of artificial neurons that process data hierarchically and can capture complex patterns in data. Each layer learns and transforms the input data, gradually capturing higher-level features and abstractions.

The DL training process involves feeding labeled data to the neural network and adjusting the weights and biases of the neurons iteratively. It can reduce the dependency on manual feature engineering and achieves impressive results in various domains such as computer vision, natural language processing, speech recognition, and reinforcement learning.

DL technologies are about to transform the world with innovations such as transformers, generative AI, ChatGPT, and more. In addition, larger and more intelligent foundation models can perform human-like tasks, generate and understand content, and more.

Working and developing deep learning models introduce additional operational complexities and scaling challenges. This is where MLOps comes in to help simplify and abstract complexities and operationalize the process of developing and using complex models.

There are multiple deep learning frameworks. The major ones are:

*   *TensorFlow*: Developed by Google, [TensorFlow](https://www.tensorflow.org/) is one of the most widely used deep learning frameworks. TensorFlow is open-source and provides a comprehensive ecosystem of tools, libraries, and high-level APIs (such as Keras) for building and deploying deep learning models.
    
*   *PyTorch*: Developed by Meta’s AI Research lab, [PyTorch](https://pytorch.org/) is an open-source deep learning library that has gained significant popularity since it provides a flexible, dynamic computing graph that makes it easy to build and train deep learning models.
    
*   *Keras*: Initially a standalone library, [Keras](https://keras.io/) has become a part of TensorFlow’s official API. Keras is open-source and provides a simpler high-level API for building and training deep learning models.
    
*   *Caffe*: [Caffe](https://caffe.berkeleyvision.org/) is an open-source deep learning framework developed by Berkeley AI Research (BAIR) that allows building, training, and deploying deep neural networks. Caffe is focused on computer vision tasks and is known for its speed and efficiency.
    

These solutions provide a variety of features and capabilities, including GPU acceleration, distributed training, and pre-built models and architectures, making it easier to develop and train complex deep learning models.

# Distributed Deep Learning

As model size increases and the amounts of training data pile up, a growing need exists to accelerate and distribute the training process across multiple computers. The distributed training process breaks the task into smaller tasks or data elements and combines the results into a larger model. The two widely used methods for distributed (parallel) training are:

*   *Data parallelism*: Replicating the model to multiple systems, and each replica is trained on a subset of the data. The gradients computed on each replica are then averaged to update the shared model parameters. Data parallelism is effective when the model parameters are more significant than the data size.
    
*   *Model parallelism*: Distributing different parts of the model to multiple systems or GPU devices. Each system or device is responsible for computing its assigned model portion’s forward and backward passes. This approach is used when the model is too large to fit in the memory of one system or GPU.
    

[Figure 7-1](#ch07.xhtml#fig-8-data-vs-model-distribution) demonstrates the differences between data and model parallelism.

![](assets/8-data-vs-model-distribution.png)

In distributed training, large amounts of data are exchanged between systems, requiring fast networks and high-performance messaging protocols (such as [MPI](https://en.wikipedia.org/wiki/Message_Passing_Interface)).

TensorFlow and PyTorch provide built-in libraries and solutions for distributing training. Those libraries can be deployed over a Kubernetes cluster, which will allocate the worker nodes.

A more generic and comprehensive option is to run the training over a distributed computing framework such as [Horovod](https://www.uber.com/blog/horovod/) or [Ray](https://www.ray.io/). Both options have tight integrations with TensorFlow and PyTorch.

## Horovod

[Horovod](https://www.uber.com/blog/horovod/) is a distributed training framework developed by Uber. It supports TensorFlow, PyTorch, and other deep-learning libraries. In addition, Horovod supports model parallelism and data parallelism and implements efficient ring-based communication to synchronize gradients across distributed workers (see [Figure 7-2](#ch07.xhtml#fig-8-horovod-architecture)). It is designed to scale efficiently to large clusters and is widely used for distributed deep learning. It can use the high-performance MPI communication library for fast data exchange and synchronization.

![](assets/8-horovod-architecture.png)

## Ray

[Ray](https://www.ray.io/) is a general-purpose framework for distributed computing that includes support for distributed deep learning. It provides a set of dedicated tools for AI (AI Runtime), like Ray Tune for hyperparameter optimization and Ray Train for distributed deep learning training (see [Figure 7-3](#ch07.xhtml#fig-8-ray-architecture)). Ray is designed to be flexible, scalable, and easy to use, making it suitable for various distributed computing and training scenarios. Ray is now widely used for data processing, training, and serving large language models (LLMs).

![](assets/8-ray-architecture.png)

###### Note

MLRun provides Horovod/MPI and Ray serverless functions, eliminating deployment and scaling complexity. It takes code, automatically orchestrate the cluster, and automatically executes and tracks the distributed job.

[DeepSpeed](https://www.deepspeed.ai/), developed by Microsoft Research, is another essential framework for optimizing and distributing the training and serving of large deep learning models. DeepSpeed works on top of Ray or Horovod/MPI.

# Data Gathering, Labeling, and Monitoring in DL

Getting enough data for model training is crucial for building robust and accurate machine-learning models. However, there are several challenges in getting enough data for model training. Inaccurate data can create [drift](https://www.iguazio.com/glossary/drift-monitoring/), which makes model performance unreliable. Here are some of the critical challenges of obtaining relevant data for model training:

*   *Data availability*: Sometimes, the required data may not be available. This can happen due to various reasons, such as data privacy, limited access, or lack of data collection.
    
*   *Data quality*: Even when data is available, it may not be of sufficient quality for training the model. The data may be incomplete, noisy, or inconsistent, which can affect the model’s performance.
    
*   *Data diversity*: A machine learning model needs to be trained on diverse data to generalize well and avoid bias. However, getting diverse data can be challenging as it may require collecting data from various sources, which can be time-consuming and costly.
    
*   *Imbalanced data*: Sometimes, the data may be imbalanced, meaning that the number of instances in each class is unequal. This can lead to biased models that perform poorly on the underrepresented class.
    
*   *Cost*: Collecting large amounts of data can be expensive, especially if the data needs to be collected manually or through external sources.
    
*   *Time*: Gathering enough data for model training can be time-consuming. This can delay the development and deployment of the model, which can be a significant challenge for businesses and organizations.
    

Addressing these challenges requires careful planning and execution of data collection and preprocessing strategies.

*Data labeling*, also known as *data tagging*, is the addition of metadata to data samples as part of the data preparation process for deep learning models. Data labeling helps machine learning algorithms understand the data and make accurate predictions.

Data labeling involves automatically or semi-automatically assigning labels or tags to data samples to help the deep learning algorithm classify and recognize patterns in the data. This includes assigning categories, adding descriptive tags, or annotating specific features in the data.

For example, in an image classification task, data labeling would involve manually tagging each image with the correct label to indicate what object or scene is depicted in the image. In *natural language processing* (NLP), data labeling might involve assigning categories or tags to individual words or phrases in a text document to help the algorithm identify patterns and relationships between them. For example, the labels on photos might identify a face or a house, which words were spoken in an audio recording, or tumors in an x-ray.

Data labeling is essential for training deep learning models, as it helps the algorithm better understand and recognize patterns in the data. Accurate labeling can help improve the accuracy and effectiveness of deep learning models, while inaccurate or inconsistent labeling can lead to inaccurate predictions and reduced performance.

Data labeling is most commonly used for:

*   *Computer vision*: Labeling images, pixels, key points, or a bounding box.
    
*   *NLP*: Tagging important text sections for sentiment analysis, entity name recognition, and character recognition.
    
*   *Audio*: Converting sounds into a structured format.
    

## Data Labeling Pitfalls to Avoid

Several potential pitfalls can arise when labeling data for deep learning models. Some common pitfalls include:

*   *Lack of consistency*: Inconsistency in labeling can lead to confusion and reduced accuracy in the deep learning model.
    
*   *Bias*: Labeling can be influenced by the personal biases of the labeler, which can lead to bias in the resulting model. For example, suppose a labeler prefers a particular category or characteristic. In that case, they may inadvertently label data in a way that reinforces that preference, leading to bias in the model.
    
*   *Insufficient or irrelevant labels*: Labeling data with insufficient or irrelevant information can limit the effectiveness of the deep learning model. For example, labeling an image with a generic category such as “animal” may not provide enough information for the model to accurately classify the image.
    
*   *Insufficient data*: Lack of sufficient labeled data can lead to poor performance of the deep learning model, as it may not have enough examples to learn from.
    
*   *Time-consuming and costly*: Labeling large amounts of data can be time-consuming and costly, particularly for complex data types such as audio or video.
    
*   *Overfitting*: Overfitting occurs when the deep learning model is trained on a limited set of labeled data, which may not represent the full range of possible data. This can lead to the model being overly specialized and performing poorly on new, unseen data.
    

## Data Labeling Best Practices

Data labeling is a critical step in building effective deep-learning models. Several best practices can help ensure accurate and consistent labeling:

*   *Clearly define labels*: It is essential to clearly define labels and provide detailed instructions and guidelines for labelers. This helps ensure that all labelers clearly understand the labeling task and can produce consistent and accurate labels.
    
*   *Use multiple labelers*: Using multiple labelers to annotate each data sample can help identify inconsistencies and improve accuracy. This can be done by having each labeler annotate a subset of the data and comparing the results.
    
*   *Train labelers*: Providing training to labelers can help ensure that they understand the labeling task and can produce accurate and consistent labels. This can include training on the labeling guidelines, examples of correctly labeled data, and feedback on their performance.
    
*   *Monitor and evaluate labelers*: Regularly monitoring and evaluating labelers can help identify inconsistencies or errors and improve labeling quality. This can be done by regularly reviewing labeled data and providing feedback to labelers.
    
*   *Use quality control measures*: Quality control measures such as spot-checking, test sets, and inter-annotator agreement checks can help ensure labeling accuracy and consistency.
    
*   *Validate and refine labels*: Validating and refining labels can help improve the accuracy and effectiveness of the deep learning model. This can include evaluating the model’s performance on the labeled data and adjusting the labeling process as needed.
    
*   *Document the labeling process*: Documenting the labeling process can help ensure that it is replicable and transparent. This can include documenting the labeling guidelines, training materials, and performance evaluation results.
    

By following these best practices, data labeling can be done accurately and consistently, leading to better-performing deep learning models.

## Data Labeling Solutions

There are many commercial solutions available for data tagging that can help automate and streamline the process of labeling large datasets. Here are some of the most popular ones:

*   *Amazon SageMaker Ground Truth*: [Amazon SageMaker Ground Truth](https://aws.amazon.com/sagemaker/data-labeling/) is a fully-managed data labeling service that makes it easy to build highly accurate training datasets for machine learning.
    
*   *Labelbox*: [Labelbox](https://labelbox.com/) is a data labeling platform that provides various tools and services for managing and labeling large datasets, including image, video, and text data.
    
*   *Figure Eight*: [Figure Eight](https://appen.com/products/data-annotation/?amp) (formerly CrowdFlower, acquired by Appen) is a data annotation and collection platform that provides a wide range of annotation services, including image labeling, text annotation, and audio transcription.
    
*   *Scale AI*: [Scale AI](https://scale.com/data-labeling) is a data labeling platform that provides various annotation services, including image and video annotation, 3D point cloud labeling, and natural language processing (NLP) annotation.
    
*   *SuperAnnotate*: [SuperAnnotate](https://www.superannotate.com/) is a data annotation platform that provides various tools and services for image and video annotation, including object detection, segmentation, and classification.
    
*   *Hasty*: [Hasty](https://hasty.cloudfactory.com/) is a data labeling platform that provides various annotation services, including image and video annotation, text annotation, and audio transcription.
    

There are also many open-source solutions available for data tagging, including:

*   *Label Studio*: [Label Studio](https://labelstud.io/) is an open-source data labeling tool that supports various annotation tasks, including image, text, and audio data. It also supports a variety of machine learning frameworks, including PyTorch.
    
*   *OpenLabeling*: [OpenLabeling](https://github.com/Cartucho/OpenLabeling) is an open-source data labeling tool that supports various annotation tasks, including object detection, segmentation, and classification. It also supports multiple data types, including images and videos.
    
*   *VGG Image Annotator (VIA)*: [VGG Image Annotator](https://www.robots.ox.ac.uk/~vgg/software/via/) (VIA) is an open-source image annotation tool that supports various annotation tasks, including object detection, segmentation, and classification. It also supports multiple file formats, including JPEG, PNG, and TIFF.
    
*   *Doccano*: [Doccano](https://github.com/doccano/doccano) is an open-source text annotation tool that supports various annotation tasks, including named entity recognition, sentiment analysis, and text classification. It also supports multiple languages and custom labeling workflows.
    
*   *Anno-Mage*: [Anno-Mage](https://github.com/virajmavani/semi-auto-image-annotation-tool) is an open-source image annotation tool that supports various annotation tasks, including object detection, segmentation, and classification. It also supports multiple annotation formats, including Pascal, VOC, and COCO.
    

[Figure 7-4](#ch07.xhtml#fig-8-labelstudio) demonstrates the process of labeling a picture in Label Studio.

![](assets/8-labelstudio-predict-owl-region.gif)

These open-source solutions provide a variety of features and capabilities, including support for multiple data types, customizable labeling workflows, and integration with machine learning frameworks. They are also highly customizable and can be modified to suit specific use cases and requirements.

## Monitoring DL Models with Unstructured Data

There are many simple ways to monitor ML models that use structured data. For example, drift is detected by observing the statistical skew between training data and production data. Quality can be observed by ensuring values and categories fall into a set or range of expected values, etc. However, monitoring models with unstructured data, such as text and images, can be more complex.

One way to monitor drift in unstructured data is to convert the unstructured data to a structured representation. Here are some examples:

*   Transform an image to a set of average RGB color values or texture and check if the color or texture distribution changed.
    
*   In image or text classification problems, you can measure the distribution of the target classes and the scoring probability, or use clustering techniques and compare the results between training and production.
    
*   Use image or text embeddings and track the change of distribution of the embeddings.
    
*   In textual data, you can check for changes in word frequency or extract topics from the text, and measure changes in topics.
    

Monitoring is not confined to drift. Additional model and data attributes can be monitored, for example:

*   *Performance*: Measure various performance metrics such as latency, throughput, errors, and so on.
    
*   *Resource usage*: Memory, GPU, CPU, and IO per task.
    
*   *Accuracy*: The prediction accuracy. This usually requires human labeling to map the text or image to predefined classes.
    
*   *Sentiments or toxicity*: Check if there is toxic language or monitor sentiments in the provided or responded text.
    
*   *PII (Personally Identifiable Information) violations*: Check if the requests or responses contain PII data, such as credit card numbers, names, emails, phone numbers, social security, and more.
    
*   *Bias and fairness*: Measure the bias in the text along specific demographic attributes.
    

# Build vs Buy Deep Learning Models

Building a solution for training deep learning models involves creating a custom system tailored to the organization’s or individual’s specific needs and requirements. This can include designing and building custom hardware, developing custom software and algorithms, and acquiring and cleaning large data sets. The key differences between buying and building a solution for training deep learning models are:

*   *Time to deployment*: Buying a pre-built solution can allow for faster deployment as the necessary tools and infrastructure are already in place. On the other hand, building a custom solution can take longer as it requires designing, building, and testing the necessary components.
    
*   *Customizability*: Building a custom solution allows for greater control and customization over the training process, including flexibly modifying and optimizing algorithms and infrastructure to fit specific needs. Buying a pre-built solution may not offer the same level of customization. For example, integrations might be challenging.
    
*   *Cost*: Building a custom solution can seem more cost-effective upfront, but may be more expensive in the long run. The resources required to design, build, maintain, and update the solution will increase over time, taking a huge overhead toll on the team. Foundation and LLMs may cost tens of millions of dollars just to train.
    
*   *Access to expertise*: Many pre-built solutions are developed and maintained by experts in deep learning, meaning that users can benefit from their expertise and experience. Building in-house means the team needs to acquire this knowledge themselves.
    
*   *Data*: Large models are trained using large amounts of data, which may take significant time and processing to prepare and, in some cases, is not available or too expensive to store.
    
*   *Reduced risk*: Pre-built solutions are typically tested and validated by the vendor, which can reduce the risk of errors or failures during the training process.
    
*   *Vendor dependency*: Users of pre-built solutions may be dependent on the vendor for support, updates, and new features. This can be problematic if the vendor goes out of business or discontinues support for the product.
    

Today there are open-source model repositories (such as [Hugging Face](https://huggingface.co/)) where you can find pre-trained models for different applications and do not need to buy them from a vendor.

Another option exists which doesn’t require training a model from scratch. For example, you can extend a commercial or open-source baseline model through transfer learning and fine-tuning. Transfer learning lets you tune the parameters or weights, improving the model performance for specific tasks or content. Later in this chapter we will provide an example of tuning an LLM using a custom dataset.

Models require constant improvement by retraining the model with new data or fixing model behavior through *reinforced learning with human feedback* (RLHF). This should be factored into the decision.

# Foundation Models, Generative AI, LLMs

A new generation of foundation models has emerged. Applications such as ChatGPT have sparked the imagination and raised fears in people’s minds. The new technologies are transformative and will likely impact our lives in the near future. In this section, you will learn more about them, how to use them, and how to develop solutions around them.

First, there are several terms used in that context that require defining:

*   *Foundation models (FMs)*: Sizeable deep learning models (such as GPT-3, GPT-4) that are pre-trained with attention mechanisms on massive datasets and adaptable to various downstream tasks, including content generation. This contrasts with the traditional AI approach of designing models for specific tasks.
    
*   *Generative AI*: Methods and tools that generate content, such as images, text, or music using algorithms, typically using foundation models.
    
*   *LLM (Large Language Model)*: A type of foundation model (like ChatGPT) that can perform a variety of natural language processing (NLP) tasks. LLMs are trained on extensive text datasets and learn to generate human-like text. These models can understand context, answer questions, write essays, summarize texts, translate languages, and even generate code.
    

[Figure 7-5](#ch07.xhtml#fig-8-foundation-models) illustrates the different use cases for foundation models (and LLMs).

![](assets/8-foundation-models.jpg)

Foundation models are trained on a broad data corpus. This can include the entire content in Wikipedia, code in GitHub, and other public sources of knowledge. Unfortunately, training a foundation model is a costly and lengthy task. For example, according to public sources, the recent GPT-4 model was trained on a cluster with 25,000 GPUs. The training cycle took over a month and is estimated to have cost over $10,000,000.

Today there is an arms race of foundation models. The primary technology companies such as Microsoft (OpenAI), Google, Meta, NVIDIA, and newer entrants are working on new models that will be more intelligent, safer, faster, or better in specific domains. In addition, open-source LLM efforts are gaining momentum, and they catch up on intelligence and performance with their commercial counterparts in many use cases. Therefore, make sure you introduce flexibility into your solution, allowing you to switch between baseline models without refactoring your work.

[Figure 7-6](#ch07.xhtml#fig-8-llm-landscape) shows the current foundation model landscape (yellow indicates an open-source model).

![](assets/8-llm-landscape.png)

Most organizations can’t afford the luxury and do not need to train their foundation models. Foundation models can be tuned or customized to specific tasks without full training.

There are two main approaches to using foundation models and LLMs:

*   *Prompt engineering*: Feeding the model with engineered requests (prompts), including specific content, details, clarifying instructions, and examples that guide the model into the expected and most accurate answer.
    
*   *Transfer learning (or fine tuning)*: Using an existing pre-trained model and further training it with an application-specific dataset, significantly reducing the time and cost of computation and the training dataset size and achieving better accuracy.
    

The advantage of prompt engineering is that you don’t need to train or host the model. Instead, you access a readymade model through an API. However, the downside is that inference performance is lower, and the API calls cost is higher (due to the large prompt and prompt processing overhead). In addition, the prompt is usually limited to a few thousand words, limiting the input content’s size or leading to multiple API requests. There may also be other concerns, such as protecting or violating intellectual property.

Fine tuning allows you to extend the model’s knowledge with significantly more data and tuning it to your needs. However, it adds the complexity and costs of training (tuning) and validating the derived model. In addition, you need to handle all the deployment, upgrade, and maintenance aspects.

Suppose you use the model frequently and need it to answer more specific questions on well-defined content. Then, tuning and deploying your own model make sense. In addition, smaller open-source models can often perform better than larger ones for specific tasks.

If you use the model rarely, having a dedicated infrastructure and GPUs to host a model may be too expensive. Instead, consider the option of using prompt engineering with an existing model.

In many cases, foundation models and LLMs don’t produce the expected results and should be constantly improved and fixed. Therefore, you should apply technologies such as *reinforcement learning from human feedback (RLHF)* to continuously correct and “teach” the model. Unfortunately, this adds additional complexities and costs to the project.

## Risks and Challenges With Generative AI

While large language models and ChatGPT are extremely useful, they can be dangerous. There are several risks you should be aware of and address as part of your solution. They can be divided into the following categories:

*   *Fairness and bias*: Models are trained based on generic internet data containing inherent bias, which may be amplified in the language or be misinterpreted by the model. In addition, data may have an under-representation of voices and communities, leading the model to respond with biased and unfair answers.
    
*   *Intellectual property and privacy*: The training dataset comes from public sources. This can lead to results that may include sensitive, personal, or proprietary information that infringes copyright laws and licensing.
    
*   *Toxicity*: Data collected from the internet includes toxic, offensive, non-ethical, and harmful language. This may lead to outputs that contain explicit or implicit toxicity, including hate speech, harassment, misinformation, or discriminatory content.
    
*   *Regulatory*: When you deploy or use a generative model, you are responsible for all its outcomes and violations of legal standards, compliance, intellectual property, security, safety, and so on.
    
*   *Misuse*: Models can be misused to create harm, including generating fake and deep fake content, harassment, abuse, impersonation, terror, spam, and phishing.
    
*   *Hallucination*: Models generate answers based on statistical and mathematical methods. They do not “understand” the content but choose the most probable words based on pattern learning. There are numerous examples of models that sound confident but generate false and inaccurate answers.
    

[Figure 7-7](#ch07.xhtml#fig-8-hallucination-example) and [Figure 7-8](#ch07.xhtml#fig-8-misuse-example) demonstrate examples of model hallucination and misuse. You can notice that safeguard mechanisms were added to GPT to avoid toxic language or misuse. However, those are not yet bulletproof.

![](assets/8-hallucination-example.png)![](assets/8-misuse-example.png)

Organizations can mitigate the risks by ensuring that only valid data is used to train the models, implementing data protection and security, and ensuring compliance with regulatory standards. In addition, higher quality, reliability, and risk reduction can be achieved by implementing safety and quality measures in your data processing, training, testing, and serving pipelines.

Data processing and validation are the most critical tasks when building a Generative AI application. Here are examples of data processing steps:

1.  *Data cleansing*: Remove unwanted text, symbols, ads, spelling and grammar mistakes, irrelevant information, URLs, and so on. Use a heuristic-based approach such as perplexity to eliminate low-quality text.
    
2.  *Toxicity detection and filtering*: Identify toxic language, banned words, hate speeches, and racist remarks and remove them from the training dataset or block the transaction in case of an interactive application.
    
3.  *Bias detection and mitigation*: Measure the bias along specific demographic attributes and balance it in the training set or alternatively remove identifying details.
    
4.  *Privacy protection*: Remove any Personal Identifiable Information (PII), including names, phone numbers, credit card numbers, ids, email addresses, and so on. Check for potential leakage of private documents or code.
    
5.  *De-duplication*: Remove duplicate text segments from the dataset to improve model accuracy and reduce hallucinations.
    
6.  *Formatting and tagging*: Organize and label the data in a contextual format to improve model reliability. For example, break a document into paragraphs or sections with clear headers (covering the content of that section). Another example is organizing the text as questions and answers or text and class pairs.
    
7.  *Vector embeddings and indexing*: Convert paragraphs or images into a numerical vector representation to capture the semantic and syntactic relationships and index the vectors to quickly identify similarities.
    
8.  *Tokenization*: Break down text sequences into smaller units called tokens. These tokens can be words, subwords, or characters, depending on the chosen tokenization strategy. Tokenization enables text transformation into a format that machines can process and analyze.
    

[Figure 7-9](#ch07.xhtml#fig-8-nlp-data-pipeline) illustrates a data processing pipeline in an NLP (natural language processing) application.

![](assets/8-nlp-data-pipeline.png)

Data controls can be implemented in one or more of the following tasks:

1.  *Data pre-processing*: Data cleaning and preparation before the data is sent to the model tuning or vector indexing process.
    
2.  *Model and application testing*: Testing the model with different input prompts and verifying that the outputs and model behavior conforms with the expected standards and quality.
    
3.  *Processing of user requests*: Checking the incoming user requests and prompts (in real-time) before they are sent to the model.
    
4.  *Validating model responses*: Verifying the compliance and quality of the results in real-time before they are returned to the user, and blocking them or providing an alternative response.
    
5.  *Monitoring the application logs*: Sending all the requests (prompts) and responses to a monitoring system that checks the compliance, accuracy, and quality of the results.
    

There are advantages and disadvantages to each approach, it is always better to address the problem in the earliest stage (data pre-processing), but in many cases, you cannot implement a solution on the application data path due to performance or complexity reasons and the tracking is done in the monitoring system and its feedback is used to further tune the system.

Another significant element in ensuring high quality and reliability is adding user feedback *(humans in the loop)*. This means sampling the results in the production data and verifying by humans that they meet the expected behavior. If they don’t, correcting them and re-training or tuning the model to fix that behavior.

## MLOps Pipelines For Efficiently Using and Customizing LLMs

Earlier in this chapter, we described two methods to customize LLMs for specific data and applications, prompt engineering and fine-tuning. In many applications, we will use both to maximize performance and reliability. Data preparation and validation are crucial parts of the process in both cases.

In fine-tuning, you ingest and prepare the application data and feedback data, run a transfer learning job to tune the base foundation model and extensive testing, and finally deploy the newly built model into the staging or production environment (see [Figure 7-10](#ch07.xhtml#fig-8-llm-tuning-pipe)).

![](assets/8-llm-tuning-pipe.png)

The pipeline can run anytime you have new data or enough feedback data (generated through human labeling). In addition, since the target model is large, it requires distributed training (tuning) and validation over multiple systems and GPUs, which can be done with frameworks such as Horovod/MPI or Ray.

When you implement prompt engineering and need to feed the prompt with reference documents or context, the context should be prepared and indexed for efficient retrieval. For example, using embeddings and vector or keyword databases (see [Figure 7-11](#ch07.xhtml#fig-8-llm-indexing-pipe)). Furthermore, the data fed and indexed in the database must be thoroughly cleaned and prepared to maximize the response quality and avoid risks.

![](assets/8-llm-indexing-pipe.png)

Multiple new frameworks target data processing and indexing for language processing tasks. For example, [LangChain](https://python.langchain.com/en/latest/index.xhtml), [Llama Index](https://gpt-index.readthedocs.io/en/latest/), [Spacy](https://spacy.io/), and [Unstructured](https://unstructured-io.github.io/unstructured/index.xhtml) to name a few.

The pre-indexed data and tuned model are used in the interactive or real-time pipeline, which intercepts the user requests and responds with the expected answer. [Figure 7-12](#ch07.xhtml#fig-8-llm-serving-pipe) illustrates the real-time pipeline. The first step is to receive and process the request, followed by data enrichment, prompt engineering, LLM prediction, post-processing, and returning a response to the user. In addition, the incoming data and responses are sent to the monitoring system, stored, and used to identify drift, performance problems, or risks. Finally, portions of the monitored data can be sent to human labeling and used for re-tuning the model (RLHF).

![](assets/8-llm-serving-pipe.png)

The real-time pipeline introduces several challenges:

*   Relative complexity and integration of disparate elements (data processing and enrichment, application logic, model prediction, monitoring, human feedback, and so on).
    
*   Partitioning the large model to multiple GPU devices and systems.
    
*   Model performance (LLMs are notoriously slow and may require multiple calls for a single user flow).
    
*   Real-time request and response validation to avoid risks.
    
*   Continuous deployment and rolling upgrades.
    

Elastic serverless frameworks and automation can reduce the complexity.

## Application Example: Fine-Tuning an LLM Model

The following tutorial provides an example of building and operationalizing an intelligent question-and-answering application using a fine-tuned large language model.

The tutorial contains two main parts:

1.  Data preparation and tuning pipeline
    
2.  Real-time application and model serving pipeline
    

The application is tuning an LLM with the content of [Iguazio’s MLOps blog](https://www.iguazio.com/blog/) and it answers MLOps-related questions. It uses [MLRun](https://www.mlrun.org/) to rapidly build, run, and monitor the two pipelines.

The complete source code can be found at: [*https://github.com/mlrun/demo-llm-tuning*](https://github.com/mlrun/demo-llm-tuning). Open the *tutorial.ipynb* file to see the tutorial flow.

### Data Preparation and Tuning

In this tutorial, the open-source LLM model is downloaded from [Hugging Face](https://huggingface.co/) and fine-tuned with the MLOps dataset generated from the blog pages. The first steps of the pipeline are reading the pages from the internet (blog), converting them to cleaned-up text, and pre-processing them to maximize the model’s performance. The following step uses Horovod, MPI, and [DeepSpeed](https://www.microsoft.com/en-us/research/project/deepspeed/) to scale the tuning process across multiple systems and GPUs. The final step evaluates the model. See the flow described in [Figure 7-13](#ch07.xhtml#fig-8-training-pipeline).

![](assets/8-training-pipeline.png)

###### Note

Many more tests will likely be added in a real-world application.

But first, let’s verify that the base LLM only knows a little about MLOps by loading it and asking a question. See the code in [Example 7-1](#ch07.xhtml#code-8-test-base-llm).

##### Example 7-1. Testing the base LLM (before tuning)

```
from transformers import GPT2Tokenizer, GPT2LMHeadModel, GenerationConfig, pipeline

model_name = "gpt2-medium"
tokenizer = GPT2Tokenizer.from_pretrained(model_name)
model = GPT2LMHeadModel.from_pretrained(model_name)
generation_config = GenerationConfig.from_pretrained(model_name)
generator = pipeline("text-generation", model=model, tokenizer=tokenizer)

def prompt_to_response(prompt: str) -> str:
    return generator(prompt, generation_config=generation_config, max_length=50,
                     pad_token_id=tokenizer.eos_token_id)[0]["generated_text"]

print(prompt_to_response(prompt="What is MLRun?"))

> MLRun is a tool that allows you to run a simple ML program on your computer...
```

#### Data Preparation

Feeding the raw HTML pages into the model will likely result in poor and inaccurate answers. In that case, the data needs to be cleaned from redundant symbols, broken into text sections with clear titles (topics) and labeled with unique tokens to identify and protect against model hallucinations.

The function `mark_header_tags()` in the file *data\_collection.py* (see code [Example 7-2](#ch07.xhtml#code-8-mark-header-tags)) converts the HTML text and headers into marked text.

##### Example 7-2. Converting the HTML text and headers into marked text

```
def mark_header_tags(soup: BeautifulSoup):
    """
    Adding header token and article token prefixes to all headers in html,
    in order to parse the text later easily.

    :param soup: BeautifulSoup object of the html file
    """
    nodes = soup.find_all(re.compile("^h[1-6]$"))
    # Tagging headers in html to identify in text files:
    if nodes:
        content_type = type(nodes[0].contents[0])
        nodes[0].string = content_type(
            ARTICLE_TOKEN + normalize(str(nodes[0].contents[0]))
        )
        for node in nodes[1:]:
            if node.string:
                content_type = type(node.contents[0])
                if content_type == Tag:
                    node.string = HEADER_TOKEN + normalize(node.string)
                else:
                    node.string = content_type(HEADER_TOKEN + str(node.contents[0]))
```

The function `convert_textfile_to_data_with_prompts()` in the file *data\_preprocess.py* (see [Example 7-3](#ch07.xhtml#code-8-test-base-llm2)) converts the document text into a set of prompts and answers based on the topics of each section.

##### Example 7-3. Testing the base LLM (before tuning)

```
def convert_textfile_to_data_with_prompts(txt_file: Path):
    """
    Formatting the html text content into prompt form.
    Each header-content in the article is an element in the list of prompts

    :param txt_file: text content as a string with tokens of headers.
    :returns: list of prompts
    """
    # Read file:
    with open(txt_file, "r") as f:
        lines = f.readlines()

    start = 0
    end = 0
    subject_idx = []
    data = []
    # Dividing text into header - paragraph prompts:
    for i, line in enumerate(lines):
        if not start and line.startswith(ARTICLE_TOKEN):
            start = i
        elif HEADER_TOKEN + END_OF_ARTICLE in line:
            end = i
            break
        if line.startswith(HEADER_TOKEN):
            subject_idx.append(i)
    article_content = lines[start:end]
    subject_idx = [subject_i - start for subject_i in subject_idx]
    article_name = article_content[0].replace(ARTICLE_TOKEN, "")
    for i, subject in enumerate(subject_idx):
        if subject + 1 in subject_idx:
            continue
        subject_data = article_content[subject].replace(HEADER_TOKEN, "")
        if i + 1 == len(subject_idx):
            content_end = len(article_content)
        else:
            content_end = subject_idx[i + 1]
        content_limits = subject + 1, content_end
        data.append(
            DATA_FORMAT.format(
                article_name,
                subject_data,
                "".join(article_content[content_limits[0] : content_limits[1]]),
            )
        )
    return data
```

#### Model Tuning

The `train()` function in *train.py* (see [Example 7-4](#ch07.xhtml#code-8-tune-func)) fine-tunes the LLM on the data. It runs the training on OpenMPI and uses DeepSpeed for distributing the model and data between multiple workers, splitting the work between nodes and GPUs. MLRun will auto-log the entire training process.

**TBD - add a description of the train function**

##### Example 7-4. Model tuning function (in *train.py*)

```
def train(
    context: MLClientCtx,
    dataset: DataItem = None,
    pretrained_tokenizer: str = None,
    pretrained_model: str = None,
    model_class: str = None,
    tokenizer_class: str = None,
    model_name: str = "huggingface-model",
    use_deepspeed: bool = True,
):
    """
    Training a model with HuggingFace transformers api.

    :param context:              MLRun context
    :param dataset:              Training dataset uri, should be passed as input.
    :param pretrained_tokenizer: the pretrained tokenizer name.
    :param pretrained_model:     the pretrained model name to train.
    :param model_class:          The model class of the model,
                                 e.g., "transformers.GPT2LMHeadModel"
    :param tokenizer_class:      The tokenizer class of the model,
                                 e.g., "transformers.AutoTokenizer"
    :param model_name:           The name of the trained model (in the registry)
    :param use_deepspeed:        Use deepspeed lib. Recommended for fine-tuning LLMs.
    """
    torch.cuda.empty_cache()
    deepspeed_config_json = None
    if use_deepspeed:
        deepspeed_config_json = os.path.join(tempfile.mkdtemp(), "ds_config.json")
        with open(deepspeed_config_json, "w") as f:
            json.dump(DEEPSPEED_CONFIG, f)
    # Creating tokenizer:
    if tokenizer_class:
        tokenizer_class = create_class(tokenizer_class)
    else:
        tokenizer_class = AutoTokenizer

    tokenizer = tokenizer_class.from_pretrained(
        pretrained_tokenizer, model_max_length=512
    )
    tokenizer.pad_token = tokenizer.eos_token

    train_dataset = Dataset.from_pandas(dataset.as_df())

    def preprocess_function(examples):
        return tokenizer(examples["text"], truncation=True, padding=True)

    tokenized_train = train_dataset.map(preprocess_function, batched=True)
    tokenized_test = None

    data_collator_kwargs = _get_sub_dict_by_prefix(
        src=context.parameters, prefix_key=KWArgsPrefixes.DATA_COLLATOR
    )
    data_collator = DataCollatorForLanguageModeling(
        tokenizer=tokenizer, mlm=False, **data_collator_kwargs
    )

    # Parsing kwargs:
    train_kwargs = _get_sub_dict_by_prefix(
        src=context.parameters, prefix_key=KWArgsPrefixes.TRAIN
    )
    if use_deepspeed:
        train_kwargs["deepspeed"] = deepspeed_config_json
    model_class_kwargs = _get_sub_dict_by_prefix(
        src=context.parameters, prefix_key=KWArgsPrefixes.MODEL_CLASS
    )

    # Loading our pretrained model:
    model_class_kwargs["pretrained_model_name_or_path"] = (
        model_class_kwargs.get("pretrained_model_name_or_path") or pretrained_model
    )
    train_kwargs["hub_token"] = train_kwargs.get("hub_token") or pretrained_tokenizer
    if not model_class_kwargs["pretrained_model_name_or_path"]:
        raise mlrun.errors.MLRunRuntimeError(
            "Must provide pretrained_model name as "
            "function argument or in extra params"
        )
    model = create_class(model_class).from_pretrained(**model_class_kwargs)

    # Preparing training arguments:
    training_args = TrainingArguments(
        output_dir=tempfile.mkdtemp(),
        **train_kwargs,
    )

    trainer = Trainer(
        model=model,
        args=training_args,
        train_dataset=tokenized_train,
        eval_dataset=tokenized_test,
        tokenizer=tokenizer,
        data_collator=data_collator,
    )

    apply_mlrun(trainer, model_name=model_name)

    # Apply training with evaluation:
    context.logger.info(f"training '{model_name}'")
    trainer.train()

    temp_directory = tempfile.gettempdir()
    trainer.save_model(temp_directory)

    # Zip the model directory:
    shutil.make_archive(
        base_name="model",
        format="zip",
        root_dir=temp_directory,
    )

    # Log the model:
    context.log_model(
        key="model",
        db_key=model_name,
        model_file="model.zip",
        tag="",
        framework="Hugging Face",
    )
```

#### Model Evaluation

The `evaluate()` function in *train.py* (see [Example 7-5](#ch07.xhtml#code-8-eval-func)) evaluates the model using the Perplexity metric.

**TBD - more description of the train eval**

##### Example 7-5. Evaluating the tuned model (in *train.py*)

```
def evaluate(context, model_path, data: pd.DataFrame):
    """
    Evaluating the model using perplexity, for more info visit:
    https://huggingface.co/docs/transformers/perplexity

    :param context:     mlrun context
    :param model_path:  path to the model directory
    :param data:        the data to evaluate the model
    """
    # Get the model artifact and file:
    (
        model_file,
        model_artifact,
        extra_data,
    ) = mlrun.artifacts.get_model(model_path)

    # Read the name:
    model_name = model_artifact.spec.db_key

    # Extract logged model files:
    model_directory = os.path.join(os.path.dirname(model_file), model_name)
    with zipfile.ZipFile(model_file, "r") as zip_file:
        zip_file.extractall(model_directory)

    # Loading the saved pretrained tokenizer and model:
    dataset = Dataset.from_pandas(data)
    tokenizer = GPT2Tokenizer.from_pretrained(model_directory)
    model = GPT2LMHeadModel.from_pretrained(model_directory)
    encodings = tokenizer("\n\n".join(dataset["text"][:5]), return_tensors="pt")

    max_length = model.config.n_positions
    stride = 512
    seq_len = encodings.input_ids.size(1)

    nlls = []
    prev_end_loc = 0
    for begin_loc in range(0, seq_len, stride):
        end_loc = min(begin_loc + max_length, seq_len)
        trg_len = end_loc - prev_end_loc  # may be different from stride on last loop
        input_ids = encodings.input_ids[:, begin_loc:end_loc]
        target_ids = input_ids.clone()
        target_ids[:, :-trg_len] = -100

        with torch.no_grad():
            outputs = model(input_ids, labels=target_ids)

            # loss calculated using CrossEntropyLoss which averages over valid labels
            neg_log_likelihood = outputs.loss

        nlls.append(neg_log_likelihood)

        prev_end_loc = end_loc
        if end_loc == seq_len:
            break

    ppl = torch.exp(torch.stack(nlls).mean()).item()
    context.log_result("perplexity", ppl)
```

#### Defining an MLRun Project and Running the Pipeline

In MLRun, the pipelines, artifacts, and models are parts of a named project. Projects usually map to a GIT project. They are versioned and can be deployed into a development or production environment using a single command. In addition, projects are managed entities with strict membership and access control. The first step is to create a project and its assets (functions, pipelines, and so on). See [Example 7-6](#ch07.xhtml#code-8-project-setup).

##### Example 7-6. MLRun project setup

```
from src.project_setup import create_and_set_project

project = create_and_set_project(
    git_source="git://github.com/mlrun/demo-llm-tuning.git#main",
    name="mlopspedia-bot",
    default_image="yonishelach/mlrun-hf",
    user_project=True,
)
```

The next step is to run the pipeline. Pipelines are executed with the `project.run()` command, and accept a set of arguments that can be used to parametrize the pipeline and allow for potential reuse in different projects. See [Example 7-7](#ch07.xhtml#code-8-run-pipeline).

##### Example 7-7. Running the tuning pipeline

```
workflow_run = project.run(
    name="training_workflow",
    arguments={
        "html_links": 'https://www.iguazio.com/blog/',
        "model_name": "gpt2-medium-mlrun",
        "pretrained_tokenizer": model_name,
        "pretrained_model": model_name,
        "model_class": "transformers.GPT2LMHeadModel",
        "tokenizer_class": "transformers.GPT2Tokenizer",
        "epochs": 20,
        "use_deepspeed": True,
    },
    watch=True,
    dirty=True
)
```

The pipeline progress can be viewed interactively in the client or notebook. When the pipeline ends it generates a summary of the results (see [Figure 7-14](#ch07.xhtml#fig-8-mlrun-tune-report)).

![](assets/8-mlrun-tune-report.png)

However, the UI provides a more interactive and rich experience (see [Figure 7-15](#ch07.xhtml#fig-8-workflow-train)).

![](assets/8-workflow-train.png)

One of the unique features of MLRun is the ability to seamlessly distribute the workload across multiple systems and GPUs (by orchestrating the underline Kubernetes, containers, and MPI resources). For example, in [Figure 7-16](#ch07.xhtml#fig-8-data-vs-model-distribution2) (taken from MLRun UI), we can see that MLRun distributed the tuning task across 16 workers and GPUs.

![](assets/8-workers-training.png)

When the pipeline ends, the model gets registered automatically and can be used in the application pipeline.

###### Note

It is advised to add deployment tests that try to deploy the application and model pipeline into a staging environment and run exhaustive tests on it to verify its reliability and performance.

### Application and Model Serving Pipeline

The application pipeline, especially in LLM use cases, consists of many steps to accept and process the request. These include steps for data enrichment and pre-processing, prompt engineering, model prediction, application control flow, safety, risk control, data post-processing and formatting, monitoring, and more. Therefore, building, deploying, and scaling the application pipeline is challenging. In addition, addressing operational considerations such as performance, security, availability versioning, and rolling upgrades can drain significant engineering resources and take time.

MLRun framework uses elastic serverless functions to automate the build and deployment of application pipelines by allowing the composition of a graph (DAG) made of multiple custom and built-in steps and translating it automatically to a distributed pipeline running over microservices.

This tutorial implements the application pipeline using MLRun Serving Graph made of four steps (see [Figure 7-17](#ch07.xhtml#fig-8-serving-graph)):

1.  *Data pre-processing (preprocess)*: Fit the user prompt into the model prompt structure (“Subject - Content”).
    
2.  *LLM prediction (LLMModelServer)*: Serve our trained model and perform inferences to generate answers.
    
3.  *Post-processing (postprocess)*: Check if the model generated reliable answers and format the output.
    
4.  *Toxicity filter (ToxicityClassifierModelServer)*: Use the Hugging Face Evaluate package model and perform inferences to catch toxic prompt or responses and respond with a proper answer instead.
    

![](assets/8-serving-graph.png)

Steps 2 and 4 are generic and can be used in multiple applications, while steps 1 and 3 are specific. You can see the full code implementation of the steps in the GIT repository (in *src/serving.py*). Note that a real application will likely include more steps and logic.

The [Example 7-8](#ch07.xhtml#code-8-set-serving-graph) code defines the graph by specifying the function or class per step, step parameters, and graph relations. The `plot()` method allows you to visualize the graph topology.

##### Example 7-8. Define the serving graph topology

```
# Set the topology and get the graph object:
graph = serving_function.set_topology("flow", engine="async")

# Add the steps:
graph.to(handler="preprocess", name="preprocess") \
     .to("LLMModelServer",
         name="mlopspedia",
         model_class="GPT2LMHeadModel",
         tokenizer_class="GPT2Tokenizer",
         model_path=project.get_artifact_uri("gpt2-medium-mlrun"),
         use_deepspeed=True) \
     .to(handler="postprocess", name="postprocess") \
     .to("ToxicityClassifierModelServer",
         name="toxicity-classifier",
         threshold=0.7).respond()

# Plot to graph:
serving_function.plot(rankdir='LR')
```

![](assets/8-serving-pipe-graph.png)

Once the real-time application graph is defined, you can debug it locally, save it, or deploy it to the cluster with a single API call or SDK command, as shown here:

```
# Deploy the serving function:
deployment = mlrun.deploy_function("serving")
```

To test the deployed application, you can use the `invoke()` method and specify the API parameters (the text question and model parameters in this case). The first example (see [Example 7-9](#ch07.xhtml#code-8-test-model1)) demonstrates a simple question and answer.

##### Example 7-9. Test the application pipeline

```
generate_kwargs = {"max_length": 150, "temperature": 0.9, "top_p": 0.5,
                   "top_k": 25, "repetition_penalty": 1.0}

response = serving_function.invoke(
    path='/predict',
    body={"prompt": "What is machine learning?", **generate_kwargs}
)
print(response["outputs"])
```

```
MLRun is an open source MLOps orchestration framework that streamlines the automation
of machine learning projects. MLRun empowers data scientists, data engineers and
DevOps teams to develop, deploy and manage machine learning applications faster and
more accurately.
MLRun integrates with popular tools such as Jupyter, PyCharm, Spark, Etc., and
provides a robust set of built-in orchestration and monitoring capabilities.
```

[Example 7-10](#ch07.xhtml#code-8-test-model2) sends a toxic question, and the model responds with a controlled answer.

##### Example 7-10. Try the toxic language filter

```
response = serving_function.invoke(
    path='/predict',
    body={"prompt": "You are stupid!", **generate_kwargs}
)
print(response["outputs"])
```

```
This bot does not respond to toxicity.
```

### Adding a Web Interface

You can use GRadio to rapidly create a UI to demo the behavior of your chat application pipeline. [Example 7-11](#ch07.xhtml#code-8-build-gradio) defines a chat window and control widgets.

##### Example 7-11. Create the GRadio interactive UI

```
import json

import gradio as gr
import requests

# Get the serving url to send requests to:
serving_url = deployment.outputs["endpoint"]


def generate(prompt, temperature, max_length, top_p, top_k, repetition_penalty):
    # Build the request for our serving graph:
    inputs = {
        "prompt": prompt,
        "temperature": temperature,
        "max_length": max_length,
        "top_p": top_p,
        "top_k": top_k,
        "repetition_penalty": repetition_penalty,
    }

    # call the serving function with the request:
    resp = requests.post(serving_url, data=json.dumps(inputs).encode("utf-8"))

    # Return the response:
    return resp.json()["outputs"]


# Set up a Gradio frontend application:
with gr.Blocks(analytics_enabled=False, theme=gr.themes.Soft()) as demo:
    gr.Markdown(
        """# LLM Playground
Play with the `generate` configurations and see how they make the
LLM's responses better or worse.
"""
    )
    with gr.Row():
        with gr.Column(scale=5):
            with gr.Row():
                chatbot = gr.Chatbot()
            with gr.Row():
                prompt = gr.Textbox(
                    label="Subject to ask about:",
                    placeholder="Type a question and Enter",
                )

        with gr.Column(scale=1):
            temperature = gr.Slider(
                minimum=0,
                maximum=1,
                value=0.9,
                label="Temperature",
                info="Choose between 0 and 1",
            )
            max_length = gr.Slider(
                minimum=0,
                maximum=1500,
                value=150,
                label="Maximum length",
                info="Choose between 0 and 1500",
            )
            top_p = gr.Slider(
                minimum=0,
                maximum=1,
                value=0.5,
                label="Top P",
                info="Choose between 0 and 1",
            )
            top_k = gr.Slider(
                minimum=0,
                maximum=500,
                value=25,
                label="Top k",
                info="Choose between 0 and 500",
            )
            repetition_penalty = gr.Slider(
                minimum=0,
                maximum=1,
                value=1,
                label="repetition penalty",
                info="Choose between 0 and 1",
            )
            clear = gr.Button("Clear")

    def respond(
        prompt,
        chat_history,
        temperature,
        max_length,
        top_p,
        top_k,
        repetition_penalty,
    ):
        bot_message = generate(
            prompt, temperature, max_length, top_p, top_k, repetition_penalty
        )
        chat_history.append((prompt, bot_message))

        return "", chat_history

    prompt.submit(
        respond,
        [prompt, chatbot, temperature, max_length, top_p, top_k, repetition_penalty],
        [prompt, chatbot],
    )
    clear.click(lambda: None, None, chatbot, queue=False)
```

See the resulting interactive window in [Figure 7-18](#ch07.xhtml#fig-8-radio-qa-screen).

![](assets/8-gradio-qa-screen.png)

MLRun automated the packaging, delivery, and deployment of the project. The project can be saved into a GIT repository along with the code, workflows, and configurations and be loaded into a staging or production environment with a single command or API call. In addition, MLRun has glueless integration with CI/CD systems such as GitHub Actions, Jenkins, and GitLab CI, which allow full automation and CI/CD without additional coding or DevOps activities.

# Summary

In this chapter we delved into some of the most cutting-edge and transformative ML tech: deep learning and large language models (LLMs) projects. We started out with an overview of the deep learning training process, which is based on distributed deep learning. Then, we discussed the data that is used for training and the important considerations to take when labeling it. and the monitoring process.

In the following section, we described the innovation in foundation models, GenAI, and LLMs and dedicated a special section to their risks. Finally, we explained in great detail how to build an MLOps pipeline for using and customizing LLMs. This includes data preparation, model tuning, and model evaluation. This chapter also included an extensive demonstration of how such a pipeline operates.

# Critical Thinking Discussion Questions

*   What are the two main methods for distributed deep learning and when should you use each one?
    
*   What are the risks of data labeling and how can you overcome them?
    
*   What are the main considerations for buying deep learning models?
    
*   How can organizations ensure the authenticity of data trained on GenAI?
    
*   What’s the value of model tuning?
    

# Exercises

*   Choose an open-source data labeling solution and build a data labeling plan that would employ it.
    
*   Create a mockup process for ensuring the training data isn’t toxic, biased or poses a security risk.
    
*   Download an LLM model from Hugging Face (if you don’t have one) and fine tune it with the content of your favorite website.
    
*   Write the code for risk-free model training.
    
*   Build an application and model serving pipeline for your model from #3.
    

# Chapter 8. Solutions for Advanced Data Types

This chapter delves into the intricacies of data analysis and interpretation, focusing on modern techniques and approaches in time series analysis, natural language processing (NLP), video, and image classification. It aims to comprehensively discuss advanced data types and their applications in tackling complex problems for seasoned data scientists and beginners.

The chapter discusses the challenges and options associated with data processing and model selection, particularly concerning time series data. We’ll explore different types of solutions, weigh the trade-offs, and discuss specific considerations in the field of MLOps. We will broaden our scope to include various platforms such as Amazon Web Services (AWS), Google Cloud Platform (GCP), Hugging Face, and Apple’s CreateML. Each platform offers a unique set of tools and services which can effectively cater to different needs and preferences. By providing an unbiased comparison of these platforms and discussing their pros and cons, we aim to help you make a well-informed decision.

To get a sense of the variety of data types MLOps developers may use, look at Apple’s CreateML interface in [Figure 8-1](#ch08.xhtml#Figure-9.7-create-ml-project-types). As illustrated, there are categories in Image, Video, Motion, Sound, Text, and Tables.

![](assets/9.7-create-ml-project-types.png)

We’ll return to CreateML toward the end of the chapter.

# ML Problem Framing with Time Series

An inflection point critical decision when dealing with time-series data is whether to use machine learning (ML) or to use traditional statistical techniques. Time-series data, which involves observations of a particular variable over a specific period, is widely used in many fields, including finance, economics, weather forecasting, and more. Traditional statistical techniques such as ARIMA (Autoregressive Integrated Moving Average), ETS (Error-Trend-Seasonality), and Holt-Winters have been used for a long time to analyze and forecast time-series data.

One way to phrase this dilemma is to call it [machine learning problem framing](https://developers.google.com/machine-learning/problem-framing/problem-framing). Google refers to these steps as “Determining whether ML is the right approach for solving a problem” and then, if suitable, “Framing the problem in ML terms.”

###### Note

If you want to learn more about problem framing in the context of machine learning on the Google Cloud Platform, Noah has a certification course on the topic at [Google Professional Machine Learning Engineer Course 2023](https://learning.oreilly.com/videos/google-professional-machine/03212023VIDEOPAIML/) .

Nevertheless, ML brings several advantages over traditional statistical methods when it comes to analyzing time-series data:

*   *Non-linearity*: ML models, especially techniques like neural networks, can capture complex non-linear relationships in the data that traditional statistical methods might miss. Time-series data often exhibit non-linear patterns, and ML can handle these complexities nuancedly.
    
*   *Interactions among variables*: ML algorithms can identify and leverage complex interactions among multiple input features in ways that can be challenging for traditional statistical techniques. For instance, in a situation where the interaction of various variables influences the output, ML can model these relationships more effectively.
    
*   *Automated feature engineering*: Some ML techniques, like DL can automatically extract relevant features from the data, reducing the need for manual feature engineering. This capability can be particularly beneficial when dealing with high-dimensional data.
    
*   *Signal to noise*: ML algorithms can often be more vigorous to noise and anomalies in the data. While outliers or irregularities can significantly impact traditional statistical techniques, ML models can handle these challenges more effectively.
    
*   *Scalability*: ML techniques can scale more effectively to large datasets and use emerging technologies like custom silicon for ML. Traditional statistical methods can become computationally intractable as the size of the dataset increases, whereas ML models, especially those designed for big data environments, can handle large volumes of data more efficiently.
    

The choice between traditional statistical techniques and machine learning sometimes needs to be clarified. It depends on the specific requirements of the problem, the nature of the data, the resources available, and even the cloud platform or data platform you have immediate access to. A hybrid approach that combines elements of both might be the best solution in some cases.

With some theory out of the way, let’s look at AWS-specific solutions, followed by GCP for time series problems.

## Navigating Time Series Analysis with AWS

*Time series analysis*, a method for analyzing data patterns over time, is critical to understanding and forecasting trends. While these techniques can be applied using various tools and platforms, we focus on how AWS can facilitate this process in this section.

We will explore methods such as ARIMA and Forecast DeepAR+ for trend analysis and forecasting, which AWS supports. These techniques help decipher insights from time series data, guiding predictions for future outcomes. The choice between AWS and other platforms or even deciding to build a custom solution will depend on various factors like cost, complexity, scalability requirements, and your team’s expertise.

AWS offers several services that cater to time series analysis, each accessible through Python. Three primary ways to interact with AWS are:

*   The AWS Management Console
    
*   The AWS Command Line Interface (CLI)
    
*   The AWS Software Development Kit (SDK)
    

Each method provides unique advantages, and choosing the right one depends on your use case.

The AWS Management Console is a web-based graphical interface that provides a simplified way to interact with AWS services. You can use the console to perform tasks such as launching EC2 instances, creating S3 buckets, managing IAM users, and using AI/ML services like Amazon Forecast. The console is available at [console](https://aws.amazon.com/console/).

The AWS CLI is a command-line interface that allows you to perform AWS tasks from the command line. The AWS CLI provides a unified way to interact with AWS services, and it is helpful for tasks such as managing EC2 instances, uploading files to S3, and doing a forecast. The AWS CLI is available at [cli](https://aws.amazon.com/cli/). In many ways, the CLI is ideal for doing high-level experiments, as shown in the following example that creates an ARIMA forecast.

```
$ aws forecast create-predictor \
  --predictor-name "time-series-forecast-predictor" \
  --algorithm-arn "arn:aws:forecast:::algorithm/ARIMA" \
  --forecast-horizon 7 \
  --perform-auto-ml "false" \
  --input-data-config '{"DatasetGroupArn": \
  $(aws forecast list-dataset-groups | jq '.DatasetGroups[] \
  | select(.DatasetGroupName == "time-series-forecast-dataset-group") \
  | .DatasetGroupArn'), "SupplementaryFeatures
```

###### Note

The AWS Command Line Interface (CLI) is widely recognized as a highly effective tool for managing Amazon Web Services (AWS) resources, providing outstanding efficiency and flexibility compared to the console or other GUIs. These strengths are due to its ability to simply and rapidly execute complex commands while providing granular control over AWS resources through a comprehensive set of commands and options. The AWS CLI is optimized for scalability and can easily be integrated with other AWS services, making it well-suited for managing large-scale deployments and complex infrastructures. First, try CLI when exploring a service with complex inputs; it will often be more straightforward.

The AWS SDK is a collection of libraries and tools for developing applications interacting with AWS services. It provides a programmatic way to interact with AWS services and supports multiple programming languages, including Java, .NET, PHP, Python, and Rust. The AWS SDK is available at [developer tools](https://aws.amazon.com/developer/tools/).

*   [*Amazon Forecast*](https://aws.amazon.com/forecast/) - a fully managed service that uses machine learning algorithms to provide accurate forecasts based on historical time series data:
    

```
import boto3

forecast = boto3.client('forecast')
response = forecast.create_dataset_group(
    DatasetGroupName='forecast_dataset_group',
    Domain='CUSTOM',
)
```

*   [*Amazon QuickSight*](https://aws.amazon.com/quicksight/) - a business intelligence service that provides fast and easy-to-use data visualization and insights for time series data:
    

```
import boto3

quicksight = boto3.client('quicksight')
response = quicksight.create_data_source(
    AwsAccountId='AWS_ACCOUNT_ID',
    DataSourceId='data_source_id',
    Name='data_source_name',
    Type='ADOBE_ANALYTICS',
)
```

*   [*Amazon Kinesis Data Streams*](https://aws.amazon.com/kinesis/data-streams/) - a service for streaming real-time data used for time series analysis and visualization:
    

```
import boto3

kinesis = boto3.client('kinesis')
response = kinesis.create_stream(
    StreamName='kinesis_stream_name',
    ShardCount=1,
)
```

*   [*Amazon Kinesis Data Analytics*](https://aws.amazon.com/kinesis/data-analytics/) - a service for real-time data streams used for time series analysis:
    

```
import boto3

kinesis_analytics = boto3.client('kinesisanalytics')
response = kinesis_analytics.create_application(
    ApplicationName='kinesis_analytics_app',
    RuntimeEnvironment='SQL-1.0',
)
```

*   [*Amazon SageMaker*](https://aws.amazon.com/sagemaker/) - a service for building, training, and deploying machine learning models, including time series models:
    

```
import boto3

sagemaker = boto3.client('sagemaker')
response = sagemaker.create_notebook_instance(
    NotebookInstanceName='notebook_instance_name',
    InstanceType='ml.t2.medium',
)
```

*   [*Amazon DynamoDB*](https://aws.amazon.com/dynamodb/) - a NoSQL database service that stores and retrieves time series data:
    

```
import boto3

dynamodb = boto3.client('dynamodb')
response = dynamodb.create_table(
    TableName='dynamodb_table_name',
    KeySchema=[
        {
            'AttributeName': 'attribute_name',
            'KeyType': 'HASH'
        },
    ],
    AttributeDefinitions=[
        {
            'AttributeName': 'attribute_name',
            'AttributeType': 'S'
        },
    ],
    ProvisionedThroughput={
        'ReadCapacityUnits': 5,
        'WriteCapacityUnits': 5
    }
)
```

*   [*Amazon EMR*](https://aws.amazon.com/emr/) - a service for processing big data useful for time series analysis and visualization:
    

```
import boto3

emr = boto3.client('emr')
response = emr.run_job_flow(
    Name='emr_cluster_name',
    Instances={
        'InstanceGroups': [
            {
                'Name': 'master_instance_group',
                'InstanceRole': 'MASTER',
                'InstanceType': 'm5.xlarge',
                'InstanceCount': 1,
            },
        ],
        'Ec2KeyName': 'ec2_key_name',
        'KeepJobFlowAliveWhenTerminationProtected': False,
    },
    JobFlowRole='EMR_EC2_DefaultRole',
    ServiceRole='EMR_DefaultRole',
    VisibleToAllUsers=True,
    Applications=[
        {
            'Name': 'Spark'
        },
    ],
)
```

These AWS services support time series analysis and can be used to build and deploy scalable time series solutions for MLOps projects.

## Diving into Time Series with DeepAR+

DeepAR+, an AWS machine learning service, harnesses the power of deep learning algorithms for time series forecasting. Leveraging deep learning in time series can offer unique capabilities, as discussed earlier in this chapter. DeepAR+ excels in managing large-scale, intricate time series data, providing valuable predictions for various use cases, from demand forecasting to traffic and equipment maintenance forecasting.

DeepAR+ builds upon the DeepAR algorithm, an advanced deep learning algorithm developed specifically for time series forecasting. Remember our definition of “signal to noise” on the limitations of statistical techniques with missing data or outliers earlier in the chapter? DeepAR+ steps in here with its feature to handle data with missing values and irregular intervals.

To employ DeepAR+, you must upload your time series data to Amazon S3 and create a DeepAR+ model. Beyond just predictions, DeepAR+ also generates confidence intervals for its forecasts, enabling organizations to perceive the range of potential outcomes and make more informed decisions.

The workflow in [Figure 8-2](#ch08.xhtml#Figure-9.1-deepar-forecast) illustrates each step using the AWS CLI interface to build up to the final forecast.

![](assets/9.1-deepar-forecast.png)

The following code examples mirror this process: you can replace them with your time series dataset and adjust for future AWS CLI changes. Through these examples, we establish an S3 bucket, upload the time series data to S3, create a DeepAR+ dataset, import the data into the DeepAR+ dataset, create a DeepAR+ predictor, and finally, create a forecast using the DeepAR+ predictor.

1.  Create an S3 bucket to store the data:
    

```
$ aws s3 mb s3://deepar-example-bucket
```

1.  Upload the time series data to S3:
    

```
$ aws s3 cp time-series-data.csv s3://deepar-example-bucket/data.csv
```

1.  Create a DeepAR+ dataset following the pattern `^[a-zA-Z][a-zA-Z0-9_]*`:
    

```
$ aws forecast create-dataset \
  --dataset-name "DeepARExampleDataset" \
  --data-frequency "H" \
  --dataset-type "TARGET_TIME_SERIES" \
  --domain "CUSTOM" \
  --schema '{"Attributes":[{"AttributeName":"timestamp",\
  "AttributeType":"timestamp"},{"AttributeName":"target_value",\
  "AttributeType":"float"},{"AttributeName":"item_id",\
  "AttributeType":"string"}]}'
```

1.  import the time series data into the DeepAR+ dataset:
    

```
$ aws forecast create-dataset-import-job \
  --dataset-import-job-name "DeepARExampleDatasetImportJob" \
  --dataset-arn $(aws forecast list-datasets | \
  jq '.Datasets[] | select(.DatasetName == "DeepARExampleDatasett") | \
  .DatasetArn') \
  --data-source "S3" \
  --data-source-config '{"S3Config": {"Path": \
  "s3://deepar-example-bucket/data.csv", "RoleArn": \
  "arn:aws:iam::ACCOUNT_ID:role/ForecastRole"}}'
```

1.  create a DeepAR+ predictor:
    

```
$ aws forecast create-predictor \
  --predictor-name "DeeparExamplePredictor" \
  --algorithm-arn "arn:aws:forecast:::algorithm/Deep_AR_Plus" \
  --forecast-horizon 24 \
  --perform-auto-ml "false" \
  --input-data-config '{"DatasetGroupArn": $(aws forecast list-dataset-groups | \
  jq '.DatasetGroups[] | select(.DatasetGroupName == "DeepARExampleDatasetGroup") | \
  .DatasetGroupArn'), "SupplementaryFeatures": [{"Name": "item_id", "Value": "item1"}, \
  {"Name": "timestamp", "Value": "yyyy-MM-dd HH:mm:ss"}]}' \
  --featurization-config '{"ForecastFrequency": "H"}'
```

1.  Finally, create a forecast using the DeepAR+ predictor:
    

```
# create a forecast using the DeepAR+ predictor
$ aws forecast create-forecast \
  --forecast-name "DeeparExampleForecast" \
  --predictor-arn $(aws forecast list-predictors | \
  jq '.Predictors[] | select(.PredictorName == "DeeparExamplePredictor") | \
  .PredictorArn')
```

The critical insight here is that the CLI offers an accessible entry point for exploring cloud-based time series forecasting, often proving simpler than starting with a notebook or SDK when leveraging AWS ML and AI services. We’ll pivot to a contrasting methodology: utilizing SQL through Google BigQuery. This shift introduces an alternative avenue for our exploration, extending our toolkit beyond notebooks and Python.

## Time Series with the GCP Big Query and SQL

One platform that takes time-series analysis flexibility to another level is Google BigQuery. The trade-offs discussed earlier are less challenging because you can use both statistical techniques and machine learning techniques side-by-side. BigQuery is unique in modeling time-series data for several reasons:

*   *Scalability*: BigQuery handles large-scale data, making it ideal for simultaneously analyzing millions of time series. It can process petabytes of data quickly, making it possible to analyze and forecast trends across multiple, large numbers of time series columns.
    
*   *Multiple Time-Series Forecasting*: BigQuery ML allows forecasting multiple time series with a single query. This capability is advantageous when dealing with a vast quantity of time series variables, as it eliminates the need for running individual queries for each one.
    
*   *ARIMA and ARIMA\_PLUS Models*: BigQuery ML supports ARIMA (AutoRegressive Integrated Moving Average) models, particularly suited to time-series data. The ARIMA\_PLUS model also includes holiday effects and can change model behavior based on external factors, which is quite beneficial for certain types of time-series data.
    
*   *Integration with Google Cloud*: BigQuery is part of the Google Cloud Platform (GCP), allowing seamless integration with other GCP services. This synergy makes it easier to incorporate time-series analysis into broader data workflows.
    
*   *SQL-Based ML*: BigQuery uses SQL, a language familiar to many data professionals, for machine learning tasks. The SQL language lowers the entry barrier for those who want to build time-series models but don’t have a strong machine learning or programming background. Additionally, many people prefer SQL to query data vs. Python or R.
    
*   *Evaluation Tools*: BigQuery provides built-in functions like `ML.EVALUATE` for evaluating the accuracy of your time-series forecasts. This capability simplifies the process of assessing model performance and making necessary adjustments.
    

This [tutorial](https://cloud.google.com/bigquery/docs/arima-speed-up-tutorial) on Citi Bike trips in New York City demonstrates how to efficiently train time series models and perform multiple time-series forecasts with a single query using BigQuery. The tutorial utilizes the New York City Citi Bike trip dataset hosted on GCP and the Iowa liquor sales dataset. Key steps include:

*   Creating a dataset in BigQuery to store the ML model.
    
*   Creating the time series to forecast using the Citi Bike dataset.
    
*   Simultaneously forecasting multiple time series with default parameters speeds up the process compared to using multiple CREATE MODEL queries.
    
*   Evaluating forecasting accuracy for each time series using the `ML.EVALUATE` function.
    
*   Evaluating the overall forecasting accuracy for all the time series.
    

The gist of this style is the creation of a SQL query that includes the `model_type = 'ARIMA_PLUS'` shown here:

```
CREATE OR REPLACE MODEL bqml_tutorial.nyc_citibike_arima_model_default
OPTIONS
  (model_type = 'ARIMA_PLUS',
   time_series_timestamp_col = 'date',
   time_series_data_col = 'num_trips',
   time_series_id_col = 'start_station_name'
  ) AS
SELECT *
FROM bqml_tutorial.nyc_citibike_time_series
WHERE date < '2016-06-01'
```

Finally, run `ML.EVALUATE` to see how accurate the forecast created is:

```
SELECT *
FROM
  ML.EVALUATE(MODEL bqml_tutorial.nyc_citibike_arima_model_default,
              TABLE bqml_tutorial.nyc_citibike_time_series,
              STRUCT(7 AS horizon, TRUE AS perform_aggregation))
```

Once you get the hang of using SQL to forecast time-series data sets using BigQuery, it becomes easy to chain together a complete pipeline ranging from the original problem formation to the data ingestion, to the modeling, to the conclusion.

Let’s look at how to do those steps using the [BigQuery examples found here](https://github.com/nogibjj/gcp-ml-cert/tree/GPU/bigquery). First, let’s see the total Wikipedia pageviews, but filter out queries we don’t query about, like the front page:

```
SELECT
  views,
  title
FROM
  `bigquery-public-data.wikipedia.pageviews_2023`
WHERE
  DATE(datehour) = "2023-04-18"
  AND NOT (title LIKE "Main_Page"
    OR title LIKE "Special:%"
    OR title LIKE "Wikipedia:%"
    OR title LIKE "Wikidata:%"
    OR title LIKE "Cookie_(informatique)"
    OR title LIKE "Wikipédia:Accueil_principal"
    OR title LIKE "メインページ")
ORDER BY
  views DESC
LIMIT
  1000
```

You can see the interface of Google Big Query in [Figure 8-3](#ch08.xhtml#Figure-9.16-bq-wiki); notice how you can also “explore the data”; this option allows you even to export it into a colab notebook.

![](assets/9.16-bq-wiki.png)

The code for colab with visualization is as follows; notice how easy it is to convert the results of the previous query into a dataframe that we chart with seaborn.

```
from google.colab import auth
from google.cloud import bigquery
from google.colab import data_table

project = 'platinum-lead-379722'  # Project ID
location = 'US'  # Location
client = bigquery.Client(project=project, location=location)
data_table.enable_dataframe_formatter()
auth.authenticate_user()

job = client.get_job('bquxjob_3f8ffc04_18795d1ee67')  # Job ID
print(job.query)

results = job.to_dataframe()

# Group by title and sum the views
results = results.groupby("title").sum().reset_index()

# Sort by views in descending order
results = results.sort_values(by="views", ascending=False)

# Print the result
print(results.head(10))

# Now visualize
import seaborn as sns
import matplotlib.pyplot as plt

# Visualize
# Select top 25 pages by views
top25 = results.head(25)

# Create a bar plot using seaborn
sns.set(style="whitegrid")
plt.figure(figsize=(12, 8))
ax = sns.barplot(x="views", y="title", data=top25)
ax.set_title("Top 25 Most Viewed Wikipedia Pages on 2023-04-18")
ax.set_xlabel("Number of Views")
ax.set_ylabel("Title")
plt.show()
```

In [Figure 8-4](#ch08.xhtml#Figure-9-17-bq-colab-chart) you can see the results.

![](assets/9-17-bq-colab-chart.png)

Google Big Query lets you run an entire pipeline for time-series data that fits within an MLOPs workflow via SQL queries.

# Build vs. Buy for MLOps NLP Problems

NLP is ubiquitous in organizations looking to implement machine learning solutions. There are many ways to integrate solutions with NLP, including building custom models from scratch or purchasing pre-trained models from vendors.

As language models become more sophisticated, whether to *build* or *buy* has significant implications for businesses. This decision impacts cost, resources, time, customization, and control.

This section delves into the considerations surrounding this decision in the context of Large Language Models (LLMs). These models, trained on vast amounts of data, have shown an impressive ability to analyze and generate human-like text, making them a potent tool for many applications.

The *build vs. buy* decision in the context of LLMs is especially salient, given their resource requirements for training and the expertise needed for customization. We will explore these factors, and others, to provide a framework for making this critical decision in your enterprise’s MLOps strategy.

# Build vs Buy: The Hugging Face Approach

In the age of LLMs, Hugging Face has emerged as an influential player. Its hub acts as a repository for ML, offering models, datasets, and demos. It’s a one-stop shop for ML resources, akin to BigQuery’s expansive data warehouse.

However, what if you’re unsure whether to build your solution or leverage Hugging Face’s offerings? Let’s delve into this dilemma:

*   *Models*: Hugging Face provides a wide range of pre-trained models for NLP, vision, and audio tasks. Think of it as a ready-to-use AWS library but focused on ML. Model cards provide information about limitations, biases, tasks, languages, etc. They’re like BigQuery’s schema descriptions but for ML models.
    
*   *Datasets*: Similar to BigQuery’s vast datasets, Hugging Face hosts over 5,000 datasets in over 100 languages. Dataset Cards and Dataset Previews offer insights into the data, just as BigQuery provides table previews.
    
*   *Spaces*: Hugging Face Spaces are interactive apps to showcase your models. Imagine it as an AWS Lambda function but for demoing your ML models. You can create your own space, upgrade it for GPU support, and even showcase it at conferences or meetings.
    
*   *Organizations*: Similar to AWS Organizations, Hugging Face provides a feature for grouping accounts and managing datasets, models, and Spaces. This feature enables better collaboration and resource management across teams and projects.
    
*   *Security*: Hugging Face has robust security features, similar to AWS’s IAM roles and BigQuery’s access control. User Access Tokens, access control for organizations, commit signing with GPG, and malware scanning ensure your work is secure.
    

Hugging Face has emerged as a powerful and adaptable platform for executing machine learning workflows. Building your solution from scratch or harnessing an existing one is a tough choice. Hugging Face achieves a desirable equilibrium between ready-to-use resources and customization opportunities to cater to your requirements by allowing you to use a model as is or fine-tune it. Numerous examples of Hugging Face exist throughout this book for your perusal. However, to avoid redundancy and conserve space, we will forego adding another Hugging Face example here.

# Exploring Natural Language Processing with AWS

Before we dive into a hands-on example, let’s first set the context for the role of Natural Language Processing (NLP) within MLOps. NLP is a branch of artificial intelligence that helps machines analyze, interpret, and generate human language. In MLOps, NLP can play a crucial role in various tasks, such as sentiment analysis, text classification, language translation, and information extraction. Organizations can make more data-driven decisions, improve customer experiences, and optimize business processes by automating these tasks.

Similar to our exploration of time series analysis, we’ll examine how AWS provides a comprehensive suite of ready-to-use solutions for NLP tasks, offering an alternative to platforms like Hugging Face. AWS’s services enable rapid implementation of NLP tasks, making it a practical choice for MLOps workflows.

Now, let’s get our hands dirty with some NLP. Start by downloading the [Sentiment Labelled Sentences](https://archive.ics.uci.edu/ml/machine-learning-databases/00331/sentiment%20labelled%20sentences.zip) dataset from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/Sentiment+Labelled+Sentences). After downloading, open the *yelp\_labelled.txt* file. Next, navigate to the [AWS Comprehend interface](https://aws.amazon.com/comprehend/). To see AWS Comprehend in action, paste a row from the *yelp\_labelled.txt* file into the AWS Comprehend console, as shown in [Figure 8-5](#ch08.xhtml#Figure-9.8-comprehend-sentiment). This platform allows you to prototype and visualize NLP tasks such as sentiment analysis quickly. The shift from the traditional notebook or Python SDK methodologies to a more direct, user-friendly interface underscores AWS’s commitment to providing practical and accessible tools for NLP tasks within MLOps workflows.

The figure provides a graphical representation of the AWS Comprehend console interface. This web-based user interface allows you to interact with AWS Comprehend without requiring programming or command-line interactions. You can input text data directly into the console, which the service analyzes to identify key sentiment metrics.

![](assets/9.8-comprehend-sentiment.png)

A sentence from the Yelp-labelled dataset enters into the console in this example. The console’s output shows the sentiment analysis results from AWS Comprehend. It demonstrates the primary sentiments detected (Positive, Negative, Neutral, or Mixed), and the confidence scores associated with each emotion. These scores reflect the probability assigned by the model for each sentiment category. This visualization is an excellent example of how AWS Comprehend lets you quickly prototype and test sentiment analysis tasks, making it a valuable tool for MLOps. It shows the ease of transitioning from data input to insightful results, all within a user-friendly, intuitive interface.

###### Note

Sentiment analysis, sometimes called *opinion mining*, is a facet of Natural Language Processing (NLP) concerned with predicting words’ emotional tone. This inference evaluates attitudes, opinions, and emotions expressed in text data. This technique is common in areas like the voice of the customer (VoC) analysis, brand monitoring, and social media monitoring. It’s a powerful tool for understanding public sentiment, aligning closely with the concepts we’ve explored around time series analysis and forecasting. When paired with machine learning services like AWS Comprehend or tools like Hugging Face, sentiment analysis can provide valuable insights into trends, helping organizations make informed decisions. Just as we used AWS CLI for time series forecasting, AWS Comprehend can also be leveraged through similar means for sentiment analysis, underscoring the versatility of these platforms.

We previously utilized AWS CLI to conduct time series forecasting. Another service, AWS Comprehend, can perform sentiment analysis using the CLI or SDK, further demonstrating the adaptability and utility of these pre-trained model services in the MLOps context.

Now, let’s segue into the hands-on part. It’s crucial to understand that experimenting with the AWS Console and the Python SDK provides a valuable feedback loop. This iterative process allows you better to understand the ins and outs of AWS services, making integrating them into your MLOps workflows easier. An excellent example of this is as follows:

```
import boto3

# initialize comprehend client
comprehend = boto3.client(service_name='comprehend', region_name='your_aws_region')

# input text for sentiment analysis
text = 'Crust is not good.'

# detect the sentiment of the input text
response = comprehend.detect_sentiment(Text=text, LanguageCode='en')

# print the sentiment score
print(response['SentimentScore'])
```

Let’s use this approach again to start translating some text, first with the console as shown in [Figure 8-6](#ch08.xhtml#Figure-9.9-translate). That exact string of text translates from English to Portugal Portuguese.

![](assets/9.9-translate.png)

Next, let’s build several Python functions to help us translate text—all of the source code lives in this [repository](https://github.com/nogibjj/assimilate-aws). First, let’s make a function that lists the available languages to build a command-line tool that gives the end user many options:

```
def list_languages():
    """List available languages"""

    client = boto3.client("translate")
    result = client.list_languages()
    return result["Languages"]
```

The following function translates text:

```
def translate_text(text, source, target):
    """Translate text from source to target language"""

    client = boto3.client("translate")
    result = client.translate_text(
        Text=text, SourceLanguageCode=source, TargetLanguageCode=target
    )
    return (
        result["TranslatedText"],
        result["SourceLanguageCode"],
        result["TargetLanguageCode"],
    )
```

Next, build a [click command-line tool](https://github.com/nogibjj/assimilate-aws/blob/main/translator_cli.py) with two sub-commands where each sub-command maps to the functions:

```
#!/usr/bin/env python

from awstools.translatelib import translate_text, list_languages
import click
from random import choices


# build out click group
@click.group()
def cli():
    """A simple command line interface for AWS Translate"""


# build out click command to list languages
@cli.command("languages")
def cli_list_languages():
    """List available languages"""

    colors = ["red", "green", "blue", "yellow", "magenta", "cyan", "white"]
    languages = list_languages()
    for language in languages:
        # randomly select a color
        color = choices(colors)
        # print the language name in the randomly selected color
        result = f"{language['LanguageName']}, {language['LanguageCode']}"
        click.secho(result, fg=color[0])


@cli.command("translate")
@click.argument("text")
@click.option("--source", default="en", help="Source language")
@click.option("--target", default="es", help="Target language")
def translate(text, source, target):
    """Translate text from source to target language

    Example:
    ./translator_cli.py translate "Hello World" --source en --target es

    """

    text, source, target = translate_text(text, source, target)
    # use colored text to highlight the source and target languages
    click.secho("Source: {}".format(source), fg="blue")
    click.secho("Target: {}".format(target), fg="yellow")
    click.secho(text, fg="white")


# run the cli
if __name__ == "__main__":
    # pylint: disable=no-value-for-parameter
    cli()
```

Language translation, an integral part of NLP, is another task that MLOps professionals often encounter. It is beneficial in multi-lingual data environments, where understanding and translating data across languages are critical for global data-driven decision-making. AWS, Google Cloud, and other cloud services offer solutions for this, but the choice of tool largely depends on your specific needs and the context of your project.

The colored output in [Figure 8-7](#ch08.xhtml#Figure-9.10-colored-languages) showcases the diversity of languages supported by our chosen tool. This visualization was created using a simple trick of randomizing the alternate lines to give a rainbow color effect (note that you print readers will need to run this yourself to see the colors on a terminal.

![](assets/9.10-colored-languages.png)

Next, we demonstrate a translation example from the command line using a custom-built Python tool. The following command translates the phrase “Crust is not good” from English (en) to Portuguese (pt-PT).

```
python translator_cli.py translate --source en --target pt-PT "Crust is not good"
```

The translated text output is shown in [Figure 8-8](#ch08.xhtml#Figure-9.11-translated), again with the customized terminal work.

![](assets/9.11-translated.png)

This example illustrates how easy it is to automate language translation in an MLOps context, emphasizing the role of command-line tools in facilitating these tasks. While focusing on a custom tool, remember that cloud-based services offer similar functionalities, often with more robust and scalable solutions. The choice between using a custom tool or a cloud service depends on your specific requirements and the scale of your data.

## Exploring NLP with OpenAI

OpenAI, a leading NLP player, provides robust solutions that can significantly augment your MLOps workflow. Leveraging their readily available tools, we will explore constructing an “off-the-shelf” MLOps solution using OpenAI’s [Python SDK](https://github.com/openai/openai-python). The final product, a command-line interface (CLI) tool, can be accessed here: [here](https://github.com/nogibjj/assimilate-aws/blob/main/openaiAnswerbotCli.py). This CLI tool exemplifies how pre-existing solutions can effectively employ in an MLOps context.

The following code does a few things: it grabs a URL, parses the text, and summarizes it. Then it acts like a question/answer bot:

```
#!/usr/bin/env python

import openai
import os
import click
import urllib.request
from bs4 import BeautifulSoup


def extract_from_url(url):
    req = urllib.request.Request(
        url,
        data=None,
        headers={
            "User-Agent": ("Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_3) "
                           "AppleWebKit/537.36 (KHTML, like Gecko) "
                           "Chrome/35.0.1916.47 Safari/537.36")
        },
    )
    html = urllib.request.urlopen(req)
    parser = BeautifulSoup(html, "html.parser")
    text = ''.join(paragraph.text for paragraph in parser.find_all("p"))
    return text[:1500]  # return a max of 1500 characters


def submit_to_openai(text):
    openai.api_key = os.getenv("OPENAI_API_KEY")
    result = openai.Completion.create(
        prompt=text,
        temperature=0,
        max_tokens=300,
        top_p=1,
        frequency_penalty=0,
        presence_penalty=0,
        model="text-davinci-002",
    )
    return result["choices"][0]["text"].strip(" \n")


def summarize(text):
    return submit_to_openai(f"{text}\n\nTl;dr")


def submit_question(text):
    return submit_to_openai(text)


@click.group()
def cli():
    """An OpenAI tool to answer questions"""


@cli.command("question")
@click.argument("text")
def question(text):
    print(submit_question(text))


@cli.command("summarize")
@click.argument("url")
def summarize_url(url):
    print(summarize(extract_from_url(url)))


if __name__ == "__main__":
    cli()
```

First, let’s parse a website, then use OpenAI to summarize it.

```
./openaiAnswerbotCli.py summarize \
"https://en.wikipedia.org/wiki/2020_Summer_Olympics"
```

The utility of OpenAI in the realm of NLP is indeed profound, as the tool significantly simplifies many complex tasks. OpenAI is an invaluable resource for both developing and seasoned MLOps professionals. The following example showcases the tool’s ability to summarize lengthy text concisely:

```
"The 2020 Summer Olympics were postponed to 2021 due to the global COVID-19
pandemic. The event was largely held behind closed doors with no public
spectators permitted due to the declaration of a state of emergency in the
Greater Tokyo Area in response to the pandemic. The Games were the most
expensive ever, with total spending of over $20 billion."
```

Other tools in the market also offer similar capabilities, such as AWS Comprehend, Google Cloud’s Natural Language API, and Microsoft’s Azure Text Analytics. Each tool has its strengths and weaknesses, and the choice between them often boils down to your project’s specific requirements, budget, and comfort level with the respective platforms.

OpenAI stands out due to its powerful machine learning models like GPT-4 and user-friendly Python SDK, making it accessible for developers with varying levels of expertise. Its ability to generate human-like text makes it an exceptional tool for summarizing text, developing content, and answering context-based questions.

However, while OpenAI simplifies many tasks, it doesn’t negate the need for a deeper understanding of NLP and its underlying principles obsolete. A strong foundation in NLP will enable you to leverage OpenAI’s capabilities more effectively and tailor its use to your specific needs.

Next, we’ll explore another intriguing aspect of OpenAI - its capabilities for image generation.

# Video Analysis, Image Classification, and Generative AI

OpenAI’s repertoire also includes a remarkable tool called DALL-E 2. This model leverages the power of AI to generate images from textual descriptions, demonstrating a significant leap in AI image generation. It’s essentially an artist at your command, creating visuals from mere sentences.

DALL-E 2’s capacity to translate text into visual representations can be valuable in numerous MLOps scenarios. For instance, one use case is to generate data for training other models, visualizing concepts for better understanding, or creating illustrations for documentation or presentations. Its capabilities can enhance data understanding and exploratory data analysis and fuel creativity in the MLOps space.

Its accessibility via the OpenAI API means it can incorporate into existing MLOps workflows. This capability facilitates swift prototyping and iterative development, aligning with the principles of MLOps. Let’s now dive into Python and code a solution that utilizes DALL-E 2 via the OpenAI API. The complete example is at [this link](https://github.com/nogibjj/assimilate-openai/blob/main/imageGen.py):

```
#!/usr/bin/env python
"""
A command line tool that uses click to generate images using OpenAI.
"""
import click
from oalib.image_gen import generate_image

# build a click command line tool that takes a prompt and returns a generated image
@click.group()
def cli():
    """A command line tool that uses click to generate images using OpenAI."""


@cli.command("generate")
@click.option("--prompt", help="The prompt to use for image generation.")
@click.option("--size", default="1024x1024", help="The size of the image to generate.")
def generate(prompt, size):
    """Generate an image using OpenAI's API."""
    image_url = generate_image(prompt, size)
    print(image_url)


if __name__ == "__main__":
    cli()
```

Next, take let’s run this code example:

```
./imageGen.py generate --prompt "cats playing with dogs"
```

One of the exciting aspects of building your tools is the level of customization and flexibility it offers. You can tailor the AI solutions to your specific needs, intertwining APIs with your trained models or even integrating multiple APIs. Combining AI solutions allows for creation of robust, multi-faceted systems that can handle complex tasks.

For MLOps professionals, this level of customization and integration can be precious. For instance, DALL-E 2 can generate synthetic data for model training or create visual representations of complex concepts for better comprehension. Combining different AI tools, you can build a comprehensive system covering various aspects of the MLOps lifecycle, from data collection and preprocessing to model training and evaluation. Look at the command prompt in [Figure 8-9](#ch08.xhtml#Figure-9.12-cats-playing-with-dogs) for an example of what is achievable.

![](assets/9.12-cats-playing-with-dogs.png)

## Image Classification Techniques with CreateML

Let’s now look at a training-based prototyping tool, Apple’s CreateML. We’ll undertake a high-level image classification task to demonstrate its capabilities with a practical example. You can download the cats and dogs dataset from this Kaggle link: [link](https://www.kaggle.com/c/dogs-vs-cats). Alternatively, feel free to use any other image dataset you have.

As our final demonstration, [Figure 8-10](#ch08.xhtml#Figure-9.14-training) shows how rapidly a modest dataset of 200 images trains using these high-level tools. It’s important to note that speed and efficiency are paramount in MLOps. Quick model training allows for more iterations, ultimately leading to better-performing models. It also facilitates rapid prototyping and testing of different models and hypotheses. This process underscores the value of these high-level tools in an MLOps workflow, as they can significantly streamline operations and increase productivity.

![](assets/9.14-training.png)

Instead of starting from scratch, an experienced MLOps professional might use pre-trained models, like those found on [Kaggle](https://www.kaggle.com/models), as part of their workflow. These models have been previously trained on extensive datasets and can provide a solid foundation for many machine-learning tasks.

Pre-trained models can save considerable time and computational resources, as training is primarily complete. They can be instrumental when working with limited data, where training a complex model from scratch might be challenging.

[Figure 8-11](#ch08.xhtml#Figure-9.15-kaggle-pre-trained) illustrates the various pre-trained models available on Kaggle. They cover a range of tasks from image and text classification to regression and more. These models can be easily integrated into your workflow, tuned to your specific job, and deployed, making them a high-level tool that bolsters efficiency in the MLOps landscape.

![](assets/9.15-kaggle-pre-trained.png)

# Composite AI

Composite AI is an emerging approach in artificial intelligence that combines various AI technologies and models to solve complex problems. It’s more than just the sum of its parts; it’s about creating systems that can leverage the strengths of various AI techniques, such as natural language processing, image recognition, and machine learning, to deliver more comprehensive and practical solutions.

In the context of MLOps, Composite AI can be a game-changer. It allows for developing more sophisticated models that can handle multi-faceted tasks and significantly enhance your AI solutions’ robustness and flexibility.

Composite AI combines different AI techniques and integrates AI with other technologies, such as cloud computing, big data analytics, and the Internet of Things (IoT). This technique enables processing vast amounts of data, real-time decision-making, and automating complex processes.

The potential of Composite AI is enormous. It can help organizations tackle challenges that would be difficult or even impossible to solve using a single AI technique, acting as a significant driver of innovation in the AI industry.

While numerous platforms and tools are available for implementing Composite AI solutions, AWS offers an exceptionally comprehensive and versatile ecosystem. AWS combines various services, from low-level tools like Sagemaker to high-level AI and ML services like [AWS Comprehend](https://aws.amazon.com/comprehend/) for NLP or [AWS Rekognition](https://aws.amazon.com/rekognition/) for image analysis.

However, AWS is one of many players in the field. Platforms like Google Cloud, Microsoft Azure, and IBM Watson also provide tools and services for building Composite AI solutions. As an MLOps professional, understanding the capabilities of these different platforms and how to leverage them effectively is crucial to building robust, efficient, and innovative AI systems.

Next, let’s explore how AWS builds a Composite AI solution.

## Getting Started with Serverless for Composite AI

The foundation of serverless on AWS is Lambda. Lambda is a service that allows you to run your code without provisioning or managing servers. It executes your code only when needed and scales automatically, making it an ideal component of a serverless architecture.

Our focus is on how Lambda is part of a hybrid AI solution. To illustrate this, let’s consider a simple example: Lambda responds to events triggered by a timer, processes some data, and places the results in a queue.

This architectural structure is in [Figure 8-12](#ch08.xhtml#Figure-9.2-serverless-ai).

![](assets/9.2-serverless-ai.jpg)

This figure shows how the event triggers the Lambda function, which processes the data and places the results in an Amazon Simple Queue Service (SQS) queue.

In [Figure 8-13](#ch08.xhtml#Figure-9.4-trigger-sqs), we see a visual representation of how the SQS queue fits into our serverless architecture. Once the Lambda function has processed the data, it places the results in the SQS queue, ready to be consumed by other system components.

![](assets/9.4-trigger-sqs.png)

The complete code repository is the following [repo](https://github.com/noahgift/awslambda), but let’s look at the highlights of this Python example:

```
def lambda_handler(event, context):
    """Entry Point for Lambda"""
    LOG.info(f"SURVEYJOB LAMBDA, event {event}, context {context}")

    receipt_handle = event['Records'][0]['receiptHandle']  # sqs message
    event_source_arn = event['Records'][0]['eventSourceARN']

    names = []  # Captured from Queue

    # Process Queue
    for record in event['Records']:
        body = json.loads(record['body'])
        company_name = body['name']

        # Capture for processing
        names.append(company_name)

        extra_logging = {"body": body, "company_name": company_name}
        LOG.info(
            f"SQS CONSUMER LAMBDA, splitting sqs arn with value:"
            f" {event_source_arn}", extra=extra_logging
        )
        qname = event_source_arn.split(":")[-1]
        extra_logging["queue"] = qname
        LOG.info(
            f"Attemping Deleting SQS receiptHandle {receipt_handle} "
            f"with queue_name {qname}", extra=extra_logging
        )
        res = delete_sqs_msg(queue_name=qname, receipt_handle=receipt_handle)
        LOG.info(
            f"Deleted SQS receipt_handle {receipt_handle} with res {res}",
            extra=extra_logging
        )

    # Make Pandas dataframe with wikipedia snippets
    LOG.info(f"Creating dataframe with values: {names}")
    df = names_to_wikipedia(names)

    # Perform Sentiment Analysis
    df = apply_sentiment(df)
    LOG.info(f"Sentiment from FANG companies: {df.to_dict()}")

    # Write result to S3
    write_s3(df=df, name=names.pop(), bucket="fangsentiment")
```

This code is a Python script that serves as the entry point for an AWS Lambda function. The function triggers by receiving a message from an Amazon Simple Queue Service (SQS) queue. The purpose of the function is to process the message, perform sentiment analysis on a list of company names contained in the message, and write the result to an Amazon S3 bucket.

The script logs the event and context that triggered the Lambda function. It extracts the receipt handle and event source ARN from the SQS message. After processing, the receipt handles and deletes the message from the queue. The event source ARN determines the name of the SQS queue.

Next, the script loops through all the records in the event and extracts the company name from each record. The company names pass to a function named `names_to_wikipedia`, which returns a Pandas DataFrame with the names and their corresponding Wikipedia snippets.

The script then calls a function named `apply_sentiment` to perform sentiment analysis on the DataFrame. The sentiment analysis result is logged and then written to an S3 bucket named *fangsentiment*.

Finally, the script deletes the processed SQS message using the `delete_sqs_msg` function, passing in the queue name and receipt handle. With a primary case out of the way, let’s build on this and dive deeper into a hybrid AI solution.

## Use Cases of Composite AI with Serverless

Composite AI with serverless technologies presents a powerful combination for tackling complex tasks cost-efficiently. AWS provides various services, making it an ideal platform for deploying these solutions. One such scenario is hosting a pre-trained machine learning model on AWS’s Elastic File System (EFS) and leveraging AWS Lambda, a serverless computing service, for inference.

The advantage of this setup is the ability to use a low inference, low-memory language, which optimizes cost. Serverless technology, particularly when combined with well-optimized, high-performance code, can be cost-effective due to AWS Lambda’s billing model, which charges based on the number of requests and the duration of code execution. In addition to Lambda and EFS, other AWS services like Rekognition, which makes it easy to add image and video analysis to applications, can be incorporated for more comprehensive solutions. An example of such a composite AI solution shows in [Figure 8-14](#ch08.xhtml#Figure-9.3-efs-lambda-llms).

![](assets/9.3-efs-lambda-llms.png)

While this chapter focuses on AWS services, it’s worth noting that similar concepts can be applied using other cloud platforms. For instance, Google Cloud offers Cloud Functions as a serverless execution environment, and Azure provides Azure Functions. Both used their respective storage and AI/ML services to create composite AI solutions.

![](assets/9.5-host-virtualization.png)

Let’s wrap up the chapter and reflect on the key takeaways.

# Summary

This chapter embraced both diving into advanced data types and doing so pragmatically, such as using APIs, GUI tools, and pre-trained models. We also showed how AWS is an excellent base of operations for doing quick and dirty prototypes that later get put into command-line tools.

Next, go through the exercises and critical thinking questions to hammer home the ideas of this chapter.

# Exercises

*   Train a time series forecasting model using AWS SageMaker and compare its performance to a traditional time series analysis approach.
    
*   Build an NLP-based sentiment analysis model using AWS Comprehend and evaluate its accuracy on a real-world dataset.
    
*   Train an image classification model using AWS DeepLens and deploy it to a real-time video stream for object detection.
    
*   Use AWS Rekognition to perform object recognition in video streams and compare its performance to other video analysis techniques.
    
*   Create a composite AI solution using AWS services such as SageMaker, Comprehend, and Rekognition to analyze and interpret complex data sets.
    

# Critical Thinking Discussion Questions

*   What are the advantages and disadvantages of using time series analysis for data interpretation and prediction?
    
*   How can LLM NLP tools be used to address specific challenges in sentiment analysis and text classification?
    
*   What are the key considerations when using video analysis and image classification in real-world applications?
    
*   What are the critical components of a composite AI solution, and how can they be integrated to solve complex data problems?
    
*   How can AWS be used to support the entire lifecycle of a data science project, from data processing to model deployment and monitoring?
    

# Chapter 9. Solutions for Common Use Cases

Sometimes conventional wisdom isn’t conventional at all. With a new generation of data scientists coming into the industry with various backgrounds, it is crucial to explain hard-earned wisdom from the world of production software. This chapter aims to explain the history behind some of the current best practices in software engineering and how they apply to data science through MLOPs.

Additionally, this chapter shows many everyday use cases that involve MLOPs workflows, including using pre-trained models from Hugging Face, OpenAI, and others. Another discussion point is how reproducible cloud-based development environments like GitHub Codespaces with a container-first workflow create a production-first mindset.

# Relearning the Lessons of DevOps

In the early part of my career, I worked in the television and film industry, essentially an incubator for automating tasks with Python, especially around data workflows in the late 1990s and early 2000s. The official term for this is *film pipelines*, which are remarkably similar to today’s data pipelines.

The film industry had a well-deserved reputation for building brittle pipelines that constantly broke. It was widespread for film software engineers to put all of the code for a project in a “God system” that lacked even version control and mixed HTML, SQL, and Python or PHP. The official term for this development style at the time was LAMP, otherwise known as a Linux, Apache, MySQL, or PHP developer, as shown in [Figure 9-1](#ch09.xhtml#Figure-10-12-jpc-lamp). Notice the surprising similarities in a PHP application that combines markup language, SQL or Pandas, and procedural code to a typical Jupyter project. The lesson is that history repeats itself.

![](assets/10-12-jpc-lamp.png)

What is suitable for rapid experimentation, like in film or academics, is often not ideal for production because there are tradeoffs between research and operations. Similarly, if you went into an organic chemistry lab in a research facility or university, you wouldn’t necessarily want to start mixing compounds and then sell them out of the trunk of your car; there needs to be a process to operationalize research into a product.

Real-world scenarios require tradeoffs to meet business requirements. The result of these fragile software engineering practices has been more and more of a focus on DevOps. DevOps culture is directly the result of the problems created by a lack of operational excellence. DevOps ultimately comes down to behavior, as shown in [Figure 9-2](#ch09.xhtml#Figure-10-15-devops-communication-collaboration).

![](assets/10-15-devops-communication-collaboration.png)

This behavior is foundational to MLOPs because the baseline of MLOPs is DevOps. If you don’t have DevOps, you don’t have MLOps. First, you need to build those organizational capabilities. Notice that the workflow is that a feature gets created, which then leads to a change, triggers the build, and starts the deployment. Each change improves the system, and the goal is to make the system better continuously. The behavior of the participants in a DevOps workflow enables technical achievement.

In many ways, the field of data science is well suited to DevOps. The entire point of DevOps is to improve systems through feedback that uses data continuously. A well-designed software engineering architecture has extensive load-testing, instrumentation, monitoring, and automated processes that deliver actional insights. Let’s explore this concept more by discussing a production-first mindset with containers.

## Production First Mindset With Containers

Gartner believes 70% of organizations will use containers in 2023 (MLOps and DevOps heavily skew towards the use of containers), and [stackoverflow](https://www.docker.com/blog/key-insights-from-stack-overflows-2022-developer-survey/) shows that in 2022 75% of developers loved containers, as shown in [Figure 9-3](#ch09.xhtml#Figure-10-13-stackoverflow-containers).

![](assets/10-13-stackoverflow-containers.png)

Why containers?

1.  Because they allow you to *reduce* the number of dependencies in your production deployment vs. *God tools* (i.e., complex installer tools that aim to solve multiple problems simultaneously).
    
2.  With Python, you only need a [Dockerfile](https://docs.docker.com/engine/reference/builder/) and [pip](https://pip.pypa.io/en/stable/).
    
3.  Containers are a production-first mindset in that your customer is the production deployment, and you build for it initially.
    
4.  [Distroless containers](https://github.com/GoogleContainerTools/distroless) allow minimal container size, as tiny as 20MB for Microservices with [compiled languages](https://github.com/nogibjj/rust-mlops-template/tree/main/distroless-web).
    

In practice, this means an MLOps workflow starts with the following steps in which containers play a critical role:

1.  Provision a cloud-based IDE with a containerized environment almost identical to production (or very close).
    
2.  Set up the DevOps pipeline, i.e., `Makefile` (or equivalent), and get the build system working for each step: `install`, `test`, `lint`, `format`, `deploy`, etc. You can see these exact steps in this [GitHub Actions cicd.yml file.](https://github.com/nogibjj/mlops-template/blob/GPU/.github/workflows/cicd.yml)
    

```
name: CI
on:
  push:
    branches: [ "GPU" ]
  pull_request:
    branches: [ "GPU" ]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: install packages
        run: make install
      - name: lint
        run: make lint
      - name: test
        run: make test
      - name: format
        run: make format
      - name: deploy
        run: make deploy
```

These first two steps correspond to the actual [`Makefile`](https://github.com/nogibjj/mlops-template/blob/GPU/Makefile) and allow for an easy link from local testing to testing on the build system.

*   Enable AI pair programming tools, like [GitHub Copilot](https://github.com/features/copilot) with Codespaces or Cloud9 with [Code Whisperer](https://aws.amazon.com/codewhisperer/). These steps embed into a reproducible `devcontainer.json` configuration, as shown in the following section of the file. Each extension is prebuilt and preloaded in the environment:
    

```
 // Add the IDs of extensions you want installed when the container is created:
	 "extensions": [
	 "GitHub.vscode-pull-request-github",
	 "GitHub.copilot-nightly",
	 "GitHub.copilot-labs",
	 "ms-azuretools.vscode-docker",
	 "ms-toolsai.jupyter",
	 "ms-toolsai.jupyter-keymap",
	 "ms-toolsai.jupyter-renderers",
	 "ms-python.vscode-pylance",
	 "ms-python.python",
	 "ms-vscode.makefile-tools"
	 ]
```

*   Write library code (which may use a pre-trained model, i.e., Hugging Face) or use a model in a model registry. You can see a module [library here](https://github.com/nogibjj/assimilate-openai/tree/main/oalib).
    
*   Turn library code into a command-line tool first to prototype it. You can see an example of a [cli tool here](https://github.com/nogibjj/assimilate-openai/blob/main/questionAnswerCLI.py).
    
*   Then invoke library code through Jupyter, FastAPI, [Streamlit](https://streamlit.io/), [Gradio](https://gradio.app/), etc., i.e. clients of the code.
    

In practice, what this means is that with Python functions, you can build once, i.e., your library code, then “use many” and “deploy many,” as shown in [Figure 9-4](#ch09.xhtml#Figure-10-16-functions-python). This fact is why functional programming is critical to building a DevOps foundational layer for projects doing machine learning. If you first create a library that conforms to DevOps best practices, then adding this onto an MLOps workflow in which a Jupyter Notebook may or may exist is straightforward.

![](assets/10-16-functions-python.png)

# Building Solutions with Pre-Trained Models

One unique aspect of MLOps is that you don’t always need to build the model yourself to embrace the methodology surrounding MLOPs. Several important trends support this movement:

*   Carbon footprint of machine learning
    
*   Microeconomic principles of economies of scale.
    
*   Emerging technology solutions and platforms. At the forefront of this movement are two companies: [OpenAI](https://openai.com) and [Hugging Face](https://huggingface.co).
    

Later in the chapter, this container-first mindset manifests itself in building solutions with GitHub Codespaces.

## Carbon Footprint of Machine Learning

A significant challenge in modern machine learning is managing the carbon footprint of machine learning. A recent paper [The Carbon Footprint of Machine Learning Training Will Plateau, Then Shrink](https://arxiv.org/pdf/2204.05149.pdf) enumerates four practices that reduce energy:

1.  *Model*: Selecting efficient models reduces computation by 5-10 times.
    
2.  *Compute*: More specialized chips like [TPUs](https://cloud.google.com/tpu/docs/tpus) or modern GPUs are more efficient at reducing energy 1.4-2 times than other forms of a compute like a CPU.
    
3.  *Cloud*: Efficient cloud data center energy can reduce carbon usage by 1.4-2 times.
    
4.  *Regional specificity*: Cloud computing users can select data center regions with the highest percentage of clean energy.
    

While these factors are appealing and actionable, there is yet another component to reducing the carbon footprint of machine learning. Using pre-trained models eliminates the need to train new models or significantly reduces the training time through [fine-tuning](https://huggingface.co/docs/transformers/training).

###### Note

*Fine-tuning* is a technique that allows a practitioner to leverage highly complex machine-learning models with billions of parameters and create specialized versions of these models through minimal data and training.

## Economies of scale of Pre-Trained Models

Several microeconomic trends support using pre-trained models. Let’s start with the comparative advantage example. Large organizations can access thousands of expensive GPUs that train models with billions of parameters. Additionally, world-class machine learning engineers help architect models on tasks for computer vision, natural language processing, and more. Organizations that produce these sophisticated pre-trained models have a comparative advantage over smaller organizations or individuals.

As a result, there is much to be said for using pre-trained models either as they come or fine-tuning them. Being thoughtful about comparative advantage is a way to leverage the power of experts, get better business outcomes, and reduce carbon footprint.

# Emerging Technologies and Platforms for Pre-trained Models

The early part of 2020 saw the emergence of the pre-trained model revolution. Companies like [Hugging Face](https://huggingface.co/), [Stability AI](https://stability.ai/faq), and [Open AI](https://openai.com/) have pushed the envelope on a new form of machine learning in the form of pre-trained large models.

## LLM (Large Language Models)

Stanford’s Institute for Human-Centered Artificial Intelligence, in a recent article on large language models, [argues that LLMs will transform society](https://hai.stanford.edu/news/how-large-language-models-will-transform-science-society-and-ai). To show this idea in action, I used the Hugging Face interface and their BigScience, or Bloom Model, to help write a section of this book seen below, as shown in [Figure 9-5](#ch09.xhtml#Figure-10-4-bloom-llm). Notice the first sentence, *“Large language models can completely transform how we see the world.”* prompted the second sentence, *“They can be used to understand the meaning of a sentence, to generate new sentences, and to answer questions”.*

![](assets/10-4-bloom-llm.png)

One of the more famous examples of an LLM is GPT-3, [released in July 2020 by OpenAI](https://openai.com/blog/gpt-3-apps/). One of the more powerful new technologies is, [ChatGPT](https://chat.openai.com/chat) which can do things like write code on the fly and answer general search queries like “what is the life expectancy around the world for women?”.

###### Note

Large corporations often create LLMs, but [BLOOM](https://huggingface.co/bigscience/bloom) is one exciting open-source collaboration sponsored by the French government and Hugging Face. The hardware is 384 A100 80GB GPUS (48 nodes) with an 8-GPUs- per-node configuration.

Let’s start with OpenAI and use their models to build solutions next.

## OpenAI

I created a collection of examples for this book in the repo: [Assimilate OpenAI](https://github.com/nogibjj/assimilate-openai).

###### Note

You can view walkthroughs of this content on the [YouTube playlist for OpenAI](https://www.youtube.com/playlist?list=PLdfopzFjkPz-kUcaflzMqbfm6B6UVA5Iq) or [O’Reilly](https://learning.oreilly.com/videos/assimilate-openai/08252022VIDEOPAIML/)

In this example, the first library gets created by exploring the [OpenAI API](https://openai.com/api/). This code is inside a module, as shown in the following code example, which makes a simple function that accepts input and returns output, i.e., the results from the pre-trained module.

The first function, `submit_question`, can return a completion prompt using the `text-davinci-002` model. The second function, `create_code` takes a comment from any programming language and converts it into a code example. Later the first function is imported from a command-line script.

```
"""Library with OpenAI API solutions as functions

References:

For building code:  https://beta.openai.com/docs/guides/code/introduction

"""

import openai
import os


def submit_question(text):
    """This submits a question to the OpenAI API"""

    openai.api_key = os.getenv("OPENAI_API_KEY")
    prompt = text

    result = openai.Completion.create(
        prompt=prompt,
        temperature=0,
        max_tokens=300,
        top_p=1,
        frequency_penalty=0,
        presence_penalty=0,
        model="text-davinci-002",
    )["choices"][0]["text"].strip(" \n")
    return result


# build a function that converts a comment into code in any language
def create_code(text, language):
    """This submits a comment to the OpenAI API to create code in any language

    Example:
        language = '# Python3'
        text = f"Calculate the mean distance between an array of points"
        create_code(text, language)

    """
    openai.api_key = os.getenv("OPENAI_API_KEY")
    prompt = f"## {language}\n\n{text}"

    result = openai.Completion.create(
        prompt=prompt,
        temperature=0,
        max_tokens=300,
        top_p=1,
        frequency_penalty=0,
        presence_penalty=0,
        model="davinci-codex",
    )["choices"][0]["text"].strip(" \n")
    return result
```

Here, due to the previous work, it is straightforward to import this into a command-line script as shown in the following example:

```
#!/usr/bin/env python3

"""
An openai api key is required to use this script.
This uses an advanced GPT-3 model
I also used AI via Github Copilot to write this command-line interface.
"""
import click
from oalib.solutions import submit_question


@click.command()
@click.argument("text")
def main(text):
    """

This is the main function to ask OpenAI API a question:

    example: /.questionAnswerCLI.py "Who won the 2020 Summer Olympics?"

    """
    print(submit_question(text))


if __name__ == "__main__":
    # pylint: disable=no-value-for-parameter
    main()
```

As long as a valid token exists, it is easy to invoke from the command line, for example:

`./questionAnswerCLI.py "What is a good science fiction book I can read?"`

This tool returns the following output: `A good science fiction book to read is 1984 by George Orwell.`.

Next let’s process images:

```
"""A library for generating images using OpenAI."""

import openai

# build a function that takes a prompt and
# returns a generated image
def generate_image(prompt, size="1024x1024"):
    """Generate an image using OpenAI's API."""
    response = openai.Image.create(
        prompt=prompt,
        n=1,
        size=size,
    )
    image_url = response["data"][0]["url"]
    return image_url
```

```
#!/usr/bin/env python
"""
A command line tool that uses click to generate images using OpenAI.
"""
import click
from oalib.image_gen import generate_image

# build a click command line tool that takes a prompt and
# returns a generated image
@click.group()
def cli():
    """A command line tool that uses click to generate images using OpenAI."""


@cli.command("generate")
@click.option("--prompt",
	help="The prompt to use for image generation.")
@click.option("--size", default="1024x1024",
	help="The size of the image to generate.")
def generate(prompt, size):
    """Generate an image using OpenAI's API."""
    image_url = generate_image(prompt, size)
    print(image_url)


if __name__ == "__main__":
    cli()
```

To invoke this script to generate images, I can feed it any wild prompt I want:

`./imageGen.py generate --prompt "A Husky romping in the snow while licking giant lollypops"`

The Python script does a reasonable job of creating a good result based on the prompt, as shown in [Figure 9-6](#ch09.xhtml#Figure-10-18-husky).

![](assets/10-18-husky.png)

Another area to explore with OpenAI is [whisper](https://openai.com/blog/whisper/), which approaches “human level robustness and accuracy on English speech recognition,” according to OpenAI.

One of this model’s more notable use cases is generating data for other AI pipelines. The following examples, which you can also find [here](https://github.com/nogibjj/assimilate-openai/blob/main/transcribeCLI.py), show how `whisper` can quickly be wrapped in Python to transcribe multiple audio or video files at once:

```
#!/usr/bin/env python
"""
transcribe multiple files using open AI whisper and python subprocess
whisper utils/four-score.m4a --model large --language English """
import os
import click
import subprocess

# create a python function that returns files in a directory
def get_files(directory):
    """get files in a directory"""
    files = []
    for file in os.listdir(directory):
        files.append(file)
    return files


# find the size of all the files in a directory using subprocess and ls -lah
def get_size(directory):
    """get size of files in a directory"""
    filesize = subprocess.check_output(["ls", "-lah", directory])
    return filesize


# transcribe multiple files in a directory
def transcribe(directory, model, language):
    files = get_files(directory)
    for file in files:
        subprocess.run(
            [
                "whisper",
                f"{directory}/{file}",
                "--model",
                model,
                "--language",
                language,
            ],
            check=True,
        )


@click.group()
def cli():
    """transcribe multiple files using open AI whisper and python subprocess"""


@cli.command("transcribe")
@click.option("--directory", default="utils",
              help="transcribe files in a directory")
@click.option("--model", default="tiny",
              help="model to use for transcribing")
@click.option("--language", default="English",
              help="language to use for transcribing")
def transcribe_cli(directory, model, language):
    """transcribe multiple files using open AI whisper and python
    example: whisper utils/four-score.m4a --model large --language English"""
    transcribe(directory, model, language)

@cli.command("size")
@click.option("--directory", default="utils",
              help="directory to find size of files")
def size(directory):
    """get size of files in a directory"""
    click.echo(get_size(directory))


# create a command that finds files in a directory
@cli.command("find")
@click.option("--directory", default="utils", help="directory to find files")
def find_files(directory):
    """find files in a directory"""
    files = get_files(directory)
    for file in files:
        print(file)


# invoke the command
if __name__ == "__main__":
    cli()
```

First, use it to find files in a directory to verify this is where the transcription task should start:

`./transcribeCLI.py find --directory utils`

In this GitHub project running in Codespaces it returns the following result:

```
utils_four-score2.m4a
utils_four-score.m4a
```

Now that we know the tool can find the correct files, it can point to transcribing them.

`./transcribeCLI.py transcribe --directory utils`

You can see it run with the following output:

```
(.venv) @noahgift ➜ /workspaces/assimilate-openai (main) $
./transcribeCLI.py transcribe --directory utils
[00:00.000 --> 00:08.100]  Four score and seven years
[00:08.100 --> 00:14.800]  conceived in liberty and dedicated to...
[00:14.800 --> 00:20.360]  Now we are engaged in a great civil war...
[00:20.360 --> 00:24.240]  conceived and so dedicated can long endure.
[00:24.240 --> 00:26.760]  We are met on a great battlefield of that war.
```

One neat item to point out is that it also works extremely with GPUs in GitHub Codespaces, you can see an example of that in the [MLOps Template repo here](https://github.com/nogibjj/mlops-template#verify-gpu-works) and in [Figure 9-7](#ch09.xhtml#Figure-10-20-gpu-whisperer). Notice that the command `nvidia-smi -l 1` will show the GPU getting saturated.

![](assets/10-20-gpu-light.png)

We’ll look at Hugging Face next and see how it can accomplish tasks using pre-trained models.

# Hugging Face

One of the more exciting ways to use pre-trained models is by generating AI art. Let’s look at a Hugging Face Space that uses [stable-diffusion](https://huggingface.co/spaces/stabilityai/stable-diffusion) . By using the following prompt, we can develop an AI robot:

```
hero robot, robot saves dog, utopian robot, rainbow exhaust,\
periwinkle skies, steampunk style, dieselpunk style, 3D, \
realistic graphics, photorealism, hyperrealism, digital art, \
 cinematic, ArtStation, Unreal Engine, Autodesk Arnold, \
Pixar RenderMan, V-Ray, path tracing, dynamic lighting, \
dynamic shadows, dynamic reflection, ray tracing, \
volumetric lighting, perfectly shaped, hyper render, \
HD, 4K resolution, 8K resolution, razor sharp image, \
crystal clear, ultra crisp, hyper detail, best quality, \
128 samples, iteration 10000, NVIDIA OptiX denoising
```

You can view this art in [Figure 9-8](#ch09.xhtml#Figure-10-5-ai-robot). The key takeaway is that there is a bit of art and science in generating imagery with AI, and things will only improve as AI technology improves.

![](assets/10-5-ai-robot.png)

Let’s see how you can also build out a fine-tuning example using the [official tutorial](https://huggingface.co/docs/transformers/training). [Figure 9-9](#ch09.xhtml#Figure-10-21-fine-tune-hf) shows that workflow in action.

![](assets/10-21-fine-tune-hf-white.png)

The following code based on the official HuggingFace tutorial shows the process of fine-tuning with Python.

```
#!/usr/bin/env python

"""
Fine Tuning Example with HuggingFace

Based on the official tutorial
"""

from transformers import AutoTokenizer
from datasets import load_dataset, load_metric
from transformers import (AutoModelForSequenceClassification,
                          TrainingArguments, Trainer)
import numpy as np

dataset = load_dataset("yelp_review_full")
tokenizer = AutoTokenizer.from_pretrained("bert-base-cased")

def tokenize_function(examples):
    return tokenizer(examples["text"], padding="max_length", truncation=True)
tokenized_datasets = dataset.map(tokenize_function, batched=True)

model = AutoModelForSequenceClassification.from_pretrained(
    "bert-base-cased", num_labels=5)

metric = load_metric("accuracy")
def compute_metrics(eval_pred):
    logits, labels = eval_pred
    predictions = np.argmax(logits, axis=-1)
    return metric.compute(predictions=predictions, references=labels)

small_train_dataset = tokenized_datasets["train"].shuffle(seed=42).select(range(1000))
small_eval_dataset = tokenized_datasets["test"].shuffle(seed=42).select(range(1000))

training_args = TrainingArguments(output_dir="test_trainer", evaluation_strategy="epoch")
trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=small_train_dataset,
    eval_dataset=small_eval_dataset,
    compute_metrics=compute_metrics,
)

trainer.train()
```

In a nutshell, the GPU GitHub Codespace creates an ideal fine-tuning environment. This cloud-based development environment enables a powerful way to code up solutions quickly using pre-trained models and traditional software engineering toolchains.

## Creating Iterative Feedback Loops with Pre-Trained Models

You can see in [Figure 9-10](#ch09.xhtml#Figure-10-6-iterative-enterprise-mlops) that a pre-trained model opens up a whole work style. For example, you can do MLOps on a pre-trained model, push it to Hugging Face and Azure, and serve it via a model-serving framework like MLRun.

![](assets/10-6-iterative-enterprise-mlops.png)

## Building an End-to-End Solution with Multiple Tools

Another cool thing pre-trained models enable or have enabled is multiple tools in a chained AI architecture. One advance triggers another advance, which starts another; you can see the [source code for the examples here](https://github.com/nogibjj/sqlite-ai-pubtools). This workflow in [Figure 9-11](#ch09.xhtml#Figure-10-17-multi-tool-framework) shows how the OpenAI tool transcodes audio, which gets extracted into a SQLite database as keywords, which are targets for zero-shot classification with Hugging Face, which in turn allows for easy creation of YouTube playlists.

![](assets/10-17-multi-tool-framework.png)

Let’s look at how the extraction could look, assuming the text is processed; you can see a version of it [here](https://github.com/nogibjj/sqlite-ai-pubtools/blob/main/text.txt).

First, a proof of concept script flushes out how to extract the keywords, and you can grab [that here](https://raw.githubusercontent.com/nogibjj/sqlite-ai-pubtools/main/main.py). This reads in the file and uses a library to find these keywords:

```
#!/usr/bin/env python3

"""
Main cli or app entry point
"""
import yake
import click

# create a function that reads a file
def read_file(filename):
    with open(filename, encoding="utf-8") as myfile:
        return myfile.read()


# def extract keywords
def extract_keywords(text):
    kw_extractor = yake.KeywordExtractor()
    keywords = kw_extractor.extract_keywords(text)
    return keywords


# create a function that makes hash tags
def make_hashtags(keywords):
    hashtags = []
    for keyword in keywords:
        hashtags.append("#" + keyword[0].replace(" ", ""))
    return hashtags


@click.group()
def cli():
    """A cli for keyword extraction"""


@cli.command("extract")
@click.argument("filename", default="text.txt")
def extract(filename):
    """Extract keywords from a file"""
    text = read_file(filename)
    keywords = extract_keywords(text)
    click.echo(keywords)


@cli.command("hashtags")
@click.argument("filename", default="text.txt")
def hashtagscli(filename):
    """Extract keywords from a file and make hashtags"""
    text = read_file(filename)
    keywords = extract_keywords(text)
    hashtags = make_hashtags(keywords)
    click.echo(hashtags)


if __name__ == "__main__":
    cli()
```

Now that workflow is established, the next step is to put the output into a SQLite database as shown in the following example. Notice how a SQL query can serve as a critical component of the new part of the pipeline that feeds [zero-shot classification](https://raw.githubusercontent.com/nogibjj/mlops-template/8a1adcb3635a4e75bcdf0e57ed8b510ffaf6e54b/hugging-face/zero_shot_classification.py).

```
#!/usr/bin/env python3

"""
Extract keywords from a text file and load them into a database.
"""
#!/usr/bin/env python3

from main import extract_keywords, make_hashtags, read_file
import click
import sqlite3
import os
from os import path

DATABASE = "keywords.db"

def load_keywords(keywords, score, hashtags):
    db_exists = False
    if path.exists(DATABASE):
        db_exists = True

    conn = sqlite3.connect(DATABASE)
    c = conn.cursor()
    c.execute(
        "CREATE TABLE IF NOT EXISTS keywords (keyword TEXT, score REAL, hashtags TEXT)"
    )
    for keyword, score, hashtags in zip(keywords, score, hashtags):
        c.execute("INSERT INTO keywords VALUES (?, ?, ?)", (keyword, score, hashtags))
    conn.commit()
    conn.close()
    return db_exists

def collect_extract(filename):
    keywords = []
    score = []
    text = read_file(filename)
    extracted_keyword_score = extract_keywords(text)
    for keyword_score in extracted_keyword_score:
        keywords.append(keyword_score[0])
        score.append(keyword_score[1])
    hashtags = make_hashtags(extracted_keyword_score)
    return keywords, score, hashtags

def extract_and_load(filename):
    keywords, score, hashtags = collect_extract(filename)
    status = load_keywords(keywords, score, hashtags)
    return status

def query_database(order_by="score", limit=10):
    """Query the database and return keywords, hashtags, and scores"""
    conn = sqlite3.connect(DATABASE)
    c = conn.cursor()
    c.execute(
        f"SELECT * FROM keywords ORDER BY {order_by} DESC LIMIT {limit}"
    )
    results = c.fetchall()
    conn.close()
    return results

@click.group
def cli():
    pass

@cli.command("etl")
@click.argument("filename", default="text.txt")
def etl(filename):
    path_to_db = path.abspath(DATABASE)
    click.echo(
        click.style(
            f"Running ETL to extract keywords from {filename} and load them into a database: {path_to_db}",
            fg="green",
        )
    )
    result = extract_and_load(filename)
    if result:
        click.echo(click.style("Database already exists", fg="yellow"))
    else:
        click.echo(click.style("Database created", fg="green"))

@cli.command("query")
@click.option("--order_by", default="score", help="Order by score or keyword")
@click.option("--limit", default=10, help="Limit the number of results")
def query(order_by, limit):
    results = query_database(order_by, limit)
    for result in results:
        print(
            click.style(result[0], fg="red"),
            click.style(str(result[1]), fg="green"),
```

Finally, those keywords can feed into the final Hugging Face zero shot classification step, as shown in the following example.

```
#!/usr/bin/env python
"""Create Zero-shot classification command-line tool using Hugging Face's transformers library."""

from transformers import pipeline
import click

def read_file(filename):
    with open(filename, encoding="utf-8") as myfile:
        return myfile.read()

def read_labels(kw_file):
    return read_file(kw_file).splitlines()

def classify(text, labels, model="MoritzLaurer/mDeBERTa-v3-base-mnli-xnli"):
    classifier = pipeline("zero-shot-classification", model=model)
    results = classifier(text, labels, multi_label=False)
    return results

@click.group()
def cli():
    """A cli for zero-shot classification"""

@cli.command("classify")
@click.argument("filename", default="four-score.m4a.txt")
@click.argument("kw_file", default="keywords.txt")
def classifycli(filename, kw_file):
    """Classify text using zero-shot classification"""
    text = read_file(filename)
    labels = read_labels(kw_file)
    results = classify(text, labels)
    for label, score in zip(results["labels"], results["scores"]):
        click.secho(f"{label}\t{score:.2f}", fg="green")

if __name__ == "__main__":
    cli()
```

In a nutshell, many tools working together allow for a fantastic content pipeline quickly using AI tools.

# Data Poisoning Prevention in ML Systems

The concept of data poisoning, i.e. polluting training data, is one issue that should be top of mind for every organization doing MLOps. Even systems designed by the best practitioners worldwide are subject to surprising unintended consequences^([1](#ch09.xhtml#idm46247689795440)).

One way to begin discussing this scenario is [Figure 9-12](#ch09.xhtml#Figure-10-3-data-poisoning), which shows two potential attack vectors. In the first attack vector, an employee or contractor with written access to a system like Google Drive or iCloud or any cloud storage system, which screens for prohibited images, could intentionally plant either a prohibited image or an image that triggers a false positive for a prohibited image. In a second attack, an insider to a corporation that uses AI to scan for prohibited images corrupts the training data set with enough false positives that it becomes possible to trick the machine learning system to flag accounts with these same false positives later.

![](assets/10-3-data-poisoning.png)

# Recommendation Engines

Recommendation engines are at the center of an exciting problem in MLOps^([2](#ch09.xhtml#idm46247689750256)). On the one hand, recommendation engines are “table stakes” for many organizations and one of the oldest problems amongst modern machine learning pipelines. The most famous example is the Netflix prize. Most famously, it didn’t get [implemented in production](https://netflixtechblog.com/netflix-recommendations-beyond-the-5-stars-part-1-55838468f429), i.e., one of the original MLOps problems, way back in 2012. The gist of the story is initially, Netflix optimized for accuracy when doing their contest but not for operational complexity, something we now refer to as MLOps. When faced with the difficulty of the “winning” solution, they abandoned it for something easier to deploy into production.

The key issues were that the engineering requirements were too complicated, and the problem framing changed. These are critical issues that go beyond just the category of recommendation engines and are essential to any MLOps problem. There are a few unique issues related to MLOps and recommendation engines, though, and they are worth discussing. Let’s dive into user experience, negative externalities, and heuristics.

## Recommendation Engine User Experience

The most important part of a recommendation engine may be something other than the actual recommendation engine but the user experience surrounding it. Let’s quickly look at one such example with [GitHub](https://github.com). When this chapter was written in 2022, Github Feed recommended users to follow in my GitHub feed. While many people love to geek out over the intricacies of the machine learning algorithms involved in recommendation engines, a critical MLOps mindset problem is the user experience. In [Figure 9-13](#ch09.xhtml#Figure-10-1-recommendation) Noah’s GitHub feed shows two recommended users (The user’s names are highlighted out to preserve privacy). Things get interesting, though, if Noah wants to avoid following them, perhaps because he doesn’t like their code, he already follows too many people, or for a host of other reasons.

![](assets/10-1-recommendation.png)

Diving further into the user’s experience, we can drill further into controls for a particular user, as shown in [Figure 9-14](#ch09.xhtml#Figure-10-2-user-actions) (username is marked out for privacy). Still, these only let us block a user or report abuse, which are extreme actions unrelated to the recommendation engine user experience.

![](assets/10-2-user-actions.png)

The outcome is that a user who wants a constant stream of new recommended users to follow gets stuck in a black hole of user experience. The challenge with GitHub’s recommendation user experience lies in its inability to provide a continuous stream of relevant user suggestions. Users who desire new recommendations for users to follow often find themselves trapped in a problematic UX scenario. While using the platform, they must either stop receiving recommendations altogether or accept suggestions they aren’t interested in to ensure they get the recommendations they do want in the future. This workflow creates an unsatisfactory and frustrating experience for users seeking meaningful connections on the platform.

So from an MLOps perspective, this highlights that the machine learning part of the recommendation might be the least important part of the problem. Instead, a company may consider using a heuristic first, like popularity or similarity, and building a robust user experience system and ground truth data collection system.

Yet another approach is to leverage the simplicity of a `docker` command, as this [TensorFlow Recommendation systems examples shows](https://www.tensorflow.org/resources/recommendation-systems), which runs in the [MLOps template GPU enabled repo](https://github.com/nogibjj/mlops-template):

```
# Deploy the retrieval model with TensorFlow Serving
docker run -t --rm -p 8501:8501 \
  -v "RETRIEVAL/MODEL/PATH:/models/retrieval" \
  -e MODEL_NAME=retrieval tensorflow/serving &

# Retrieve top movies that user 42 may like
curl -X POST -H "Content-Type: application/json" \
  -d '{"instances":["42"]}'  \
  http://localhost:8501/v1/models/retrieval:predict

# Output
# {
#    "predictions":[
#       {
#          "output_1": [2.032, 1.969, 1.813],
#          "output_2": ["movie1”, “movie2”, “movie3”]
#       }
#    ]
# }

# Deploy the ranking model with TensorFlow Serving
docker run -t --rm -p 8501:8501 \
  -v "RANKING/MODEL/PATH:/models/ranking" \
  -e MODEL_NAME=ranking tensorflow/serving &

# Get the prediction score for user 42 and movie 3
curl -X POST -H "Content-Type: application/json" \
  -d '{"instances":[{"user_id":"42", "movie_title":"movie3"}]}' \
  http://localhost:8501/v1/models/ranking:predict

# Output:
# {"predictions": [[3.66357923]]}
```

# Ditching the Notebooks

As MLOps matures, it becomes apparent that the environment is evolving for MLOPs. In cloud computing, companies with a data center often do a “lift and shift”, (migrate the same architecture from a data center to the cloud not optimizing for new cloud-based advantages like serverless). The disadvantage for these organizations is that they are not using new paradigms but forcing old patterns onto new capabilities. Similarly, with academic workflows, a monolithic Juypter Notebook that uses unstable installation tools living on a powerful workstation under a person’s desk makes a lot of sense until you want to deploy this experiment. In [Figure 9-15](#ch09.xhtml#Figure-10-8-mlops-v2), notice that a typical academic workflow uses Jupyter Notebooks as the center of a workflow for data science. This workflow involves heavy-weight virtual machines and limited software engineering best practices when ported to cloud environments. Software installation also uses tools optimized for experimental workflows that may be challenging to reproduce.

![](assets/10-8-mlops-v2.png)

Another emerging scenario is to use cloud-native offerings that center around modern tools. Instead of a Jupyter Notebook being the start of an MLOps project, traditional software engineering tools are front and center. A production-first mindset means that, ideally, the development environment has an almost identical configuration to production. An excellent example of this in practice is the GitHub ecosystem. Let’s take a look at this evolutionary workflow for MLOPs next.

## Cloud-based Development Environment With GitHub Ecosystem

One of the more elegant aspects of developing with a cloud development-first approach is how the feedback loops for MLOps are made more efficient. If you look at [Figure 9-16](#ch09.xhtml#Figure-10-9-mlops-github-ecosystem), notice that AI pair programming coupled with traditional DevOps workflows creates an enhanced feedback loop since suggestions from the AI pair programming assistant increase in quality by linting and testing. Further, a local containerized development environment allows for an easy deployment transition to the cloud since the local environment matches production.

![](assets/10-9-mlops-github-ecosystem.png)

The question for organizations to ask themselves is how they can create a production-first mindset in which the development environment is as close as possible to production. Immediately this question negates where a Notebook-centric workflow is a centerpiece because many essential parts of a software engineering life-cycle are gone. You cannot reasonably build out containers within a Notebook; you need to edit the Dockerfile with an editor. Similarly, Notebooks don’t work with linting and testing or Infrastructure as Code, i.e., there are only hacks that don’t work as well as traditional source code files with linting, testing, formatting, and code completion.

One recommended option for implementing workflows with AI pair programming is shown in [Figure 9-17](#ch09.xhtml#Figure-10-10-copilot-ai-coding). Notice how a “DevOps in the Loop” intercepts the input from the code recommendations and then enhances the original suggestion by applying DevOps best practices.

![](assets/10-10-copilot-ai-coding-light.png)

###### Note

You can watch a Python for DevOps 2023 live coding session demonstrating this iterative process at [YouTube here](https://www.youtube.com/watch?v=lbm9ckutS3k) and [O’Reilly here](https://learning.oreilly.com/videos/python-for-devops/12012022VIDEOPAIML/).

This process is further illustrated in [Figure 9-18](#ch09.xhtml#Figure-10.11-devops-in-the-loop) in a production-first configuration. With an environment like [GitHub Codespaces](https://github.com/nogibjj/mlops-template/actions/workflows/codespaces/create_codespaces_prebuilds), you create a [Dockerfile](https://github.com/nogibjj/mlops-template/blob/GPU/.devcontainer/Dockerfile) that mimics a production environment. There are Visual Studio Code plugins enabled such as GitHub Copilot via the .devcontainer.

![](assets/10.11-devops-in-the-loop.png)

The following is roughly equivalent to the Dockerfile in the [MLOps template here](https://github.com/nogibjj/mlops-template/tree/GPU/.devcontainer). Notice that only the Python standard library tools of `virtualenv`, and `pip` are included along side some Ubuntu packages. This methodology allows for a robust and straightforward workflow for future production deployments without needing third-party installer tools:

```
FROM mcr.microsoft.com/vscode/devcontainers/universal:2-focal

RUN apt-get update && apt-get -y install --no-install-recommends \
   ffmpeg \
   python3.8-venv \
   gcc \
   pciutils

RUN apt-get update \
&& apt-get install -y nvidia-docker2 \
&& systemctl restart docker

#create global virtual environment
ARG USER="codespace"
ARG VENV_PATH="/home/${USER}/venv"
COPY requirements.txt /tmp/
COPY Makefile /tmp/
RUN su $USER -c "/usr/bin/python3 -m venv /home/${USER}/venv" \
   && su $USER -c "${VENV_PATH}/bin/pip \
 	--disable-pip-version-check \
	--no-cache-dir install -r /tmp/requirements.txt" \
   && rm -rf /tmp/requirements.txt
```

The other two notable items are the *devcontainer.json* [found here](https://github.com/nogibjj/mlops-template/blob/GPU/.devcontainer/devcontainer.json) and a setup script. The following subsection of the *devcontainer.json* shows how after installation, the final [*setup.sh*](https://raw.githubusercontent.com/nogibjj/mlops-template/GPU/setup.sh) enables the virtualenv:

```
 "postCreateCommand": "bash setup.sh"
	}
```

```
#!/usr/bin/env bash
source /home/codespace/venv/bin/activate
#append it to bash so every shell launches with it
echo 'source /home/codespace/venv/bin/activate' >> ~/.bashrc
```

One of the fantastic things about this workflow is that it makes it possible to run GPU containers and GPU projects. [Figure 9-19](#ch09.xhtml#Figure-10-14-mlops-tensorflow-gpu) shows the Tensorflow Docker container running inside GitHub codespaces using the GPU.

![](assets/10-14-mlops-tensorflow-gpu-light.png)

You can read more about [TensorFlow GPU documentation here.](https://www.tensorflow.org/install/docker). A few things to point out include the following commands:

*   Run `docker run --gpus all -it --rm tensorflow/tensorflow:latest-gpu \ python -c "import tensorflow as tf; print(tf.reduce_sum(tf.random.normal([1000, 1000])))"`
    
*   Also interactively explore: `docker run --gpus all -it --rm tensorflow/tensorflow:latest-gpu`, then when inside run: `apt-get update && apt-get install pciutils` then `lspci | grep -i nvidia`
    
*   To mount the code into your container: `docker run --gpus all -it --rm -v $(pwd):/tmp tensorflow/tensorflow:latest-gpu /bin/bash`. Then do `apt-get install -y git && cd /tmp`. Then all you need to do is run `make install`. Now you can verify you can train deep learning models by doing `python utils/quickstart_tf2.py`
    

# Bridging the Gap to Notebooks

(*guest author: andrew cui* from Linea)

Another strategy to ditch Notebooks in the MLOps cycle is to bridge the gap between the monolithic Juypter Notebooks and the cloud-native development environment. Instead of having the center of a workflow for data science be the Notebook, it should serve as a gateway to the rest of the MLOps development ecosystem.

This type of workflow is already illustrated in [Figure 9-15](#ch09.xhtml#Figure-10-8-mlops-v2) by the Notebook as a secondary tool. However, even using the notebook as a secondary tool in a cloud-native development environment brings with it many of the old challenges faced by the “lift and shift” paradigm of productionizing Notebooks in a Notebook-centric environment.

## Problems with Notebooks: Linea Can Help

There are many significant challenges brought about by using Notebooks even as a secondary tool in an MLOps-centric development environment when trying to take work from the Notebook to other MLOps-focused tools. Data science and engineering teams face many common problems including:

*   *Dependency management and toolchain setup*: Many headaches come from having the development environment set up with the same dependencies and toolchains as the production environment. This process can cause issues when trying to promote components and pipelines to run in production, as they will fail due to a lack of dependencies or missing tools.
    
*   *Management and organization of reusable code components*: A common theme among inefficient organizations with poor MLOps practices is the need for more ease of sharing code modules, leading to a proliferation of repeated code with very little code management. This process makes it much more difficult from an ops perspective to identify these reusable components so they can have good operational practices applied to them, including reviewing, testing, and deploying their code.
    
*   *Recovering and cleaning code*: In many organizations, the engineers tasked with taking notebooks to production are given messy unreadable notebooks, further complicating the above issues. Figuring out which cells are not necessary to the primary data flow is complex, and finding code that may have been in a deleted partition or executed out of order is very difficult from just the notebook is nearly impossible.
    

## What Linea Does

[Linea](https://linea.ai/) is a tool that focuses on tackling these problems, positions itself between the Jupyter Notebook, and connects it to all the other essential MLOps tools in a cloud-based development environment. This step allows for ease of integrating Jupyter Notebooks into the cloud-based development environmen while avoiding many problems associated with using Jupyter Notebooks.

Figure [Figure 9-18](#ch09.xhtml#Figure-10.11-devops-in-the-loop) illustrates where these new tools live and where they can provide benefits.

Linea can help with many of the complicated and intractable tasks of automatically bridging this gap by capturing and understanding the execution of the Notebook. Linea uses a semantic understanding of what the user ran to offer features necessary for an Ops-focused organization and directly tackles the problems using notebooks as a secondary tool.

## Solution: Dependency Management

One of the essential functionalities Linea offers is automatically creating docker files that containerize functions run in the Notebook. The ability to containerize workloads is at the heart of good MLOps practice. It is critical for scaling the capabilities of a data organization and avoiding costly issues of mismatched dependencies or toolchain wrangling.

Linea can handle dependency resolution automatically, creating Dockerfiles that regulate the installation of the minimum dependencies and tools necessary to run its payload. This step is crucial since data scientists import a lot of dependencies in their notebooks, leading to a “kitchen sink” of dependencies with all the libraries and tools they’ve ever needed to use.

By identifying the minimum set of dependencies, Linea makes it easier to set up and deploy containerized workloads and improves the reliability of these workloads as well.

The following is an example of how easy and [*https://docs.lineapy.org/en/latest/guide/build\_pipelines/pipeline\_basics.xhtml?highlight=dockerfile#output-files*](https://docs.lineapy.org/en/latest/guide/build_pipelines/pipeline_basics.xhtml?highlight=dockerfile#output-files) \[automatic this can be\].

## Solution: Identifying and using reusable components

Another critical pillar of good MLOps practice is the ability to treat code as code and use the appropriate tools to manage it. As discussed earlier, the GitHub Ecosystem is also critical to managing code. Still, a problem arises in identifying the pieces of code from a notebook taken to the GitHub ecosystem.

Fortunately, Linea can capture the semantics of what the user runs in the notebook environment and suggest high-value reusable components shared and used by many to be added and maintained in the GitHub Ecosystem. Examples of standard reusable components include an extract transform load (ETL) component to transform critical and commonly used datasets to a format used by the rest of the organization’s libraries or perhaps a model training loop that gets the credentials necessary to access GPU resources in the organization.

Once these reusable components are under the management of the Github Ecosystem, they can be versioned, edited, and developed with good MLOps practices in mind, totally avoiding the pitfalls of managing critical code in the Jupyter environment.

## Solution: Cleaning Code From Notebooks

Good clean code is a hallmark of good DevOps practices, and the same applies to MLOps. Clean code, which has been broken down nicely into functions with unnecessary code removed, is critical for understanding and taking work from notebooks to other MLOps tools which work with a production-first mindset.

Jupyter notebooks tend to trend towards messy unmaintained code and take a lot of manual cleanup effort. Linea can detect out-of-order execution or cell deletion, which usually makes notebooks messy and hard to read and reproduce. The code capture capabilities Linea provides in the development jupyter notebook environment can be used to understand the desired workflow and produce the clean minimal set of reproducible code, which can then taken and used in the rest of the [*https://github.com/LineaLabs/lineapy#use-case-1-cleaning-messy-notebooks*](https://github.com/LineaLabs/lineapy#use-case-1-cleaning-messy-notebooks) \[cloud-based development environment\].

Linea exists in the space of bridging the gap between notebooks and all the tools in a cloud-based development or production environment, providing exciting new capabilities with great potential. Linea tackles many of the most challenging problems faced by data practitioners trying to adhere to good MLOps practices. This process is where any organization looking to set up and instill a solid production-first MLOps culture can find features and technologies with huge potential to take their organizations to the next level.

# Summary

The spirit of this chapter is to highlight how cloud-based development environments allow a closer approximation of production. Counter-intuitively academic tools and workflows can take a lot of work to operationalize. The containerized development environment of GitHub Codespaces shows an emerging style of development that opens up new productivity gains, including using AI Pair Programming tools alongside pre-trained models.

The pre-trained models from OpenAI, Hugging Face, and others feed into the tools used to develop AI software, like Visual Studio Code. In essence, there is a recursive feedback loop where newer advances in AI tooling and AI pre-trained models allow better AI solutions. This chapter demonstrates how to use these exciting workflows in your work. Look through the following exercises and critical thinking challenges to take what you learned to the next level.

This chapter also sets the stage for many data science problems. One solution that solves aspects of performance, reproducibility, management, and safety is the Rust language. In the next chapter Rust for MLOps is proposed as a potential new and emerging solution for MLops.

# Exercises

*   Build a Hugging Face command-line tool that uses a locally downloaded and versioned model developed in GitHub Codespaces. You can refer to this [example GitHub Project for ideas](https://github.com/nogibjj/Coursera-MLOps-C2-Final-HuggingFace).
    
*   Look at the configuration in this [MLOps template](https://github.com/nogibjj/mlops-template) GitHub Repo. Fork it and make your own customized GitHub Template to deploy containerized microservices that use both a GPU and a pre-trained model that deploys to a cloud environment.
    
*   Build out a Tensorflow Recommender using [this example walkthrough](https://codelabs.developers.google.com/tfrecommenders-flutter#0).
    
*   Build a command-line tool that performs zero-shot classification using this [example as a starting point](https://github.com/nogibjj/mlops-template/blob/GPU/hugging-face/zero_shot_classification.py).
    
*   Run a Kubernetes-based experiment tracking service like MLRun or Bento using the [MLOps template as a starting point](https://github.com/nogibjj/mlops-template).
    

# Critical Thinking Discussion Questions

Use the following questions to think further about the topics discussed in this chapter.

*   Why does a notebook-based workflow for MLOps create new challenges over one for Data Science?
    
*   Why could it be helpful to use the least amount of tools possible in an MLOPs workflow, such as the standard library tools of [pip](https://pip.pypa.io/en/stable/) and [venv](https://docs.python.org/3/library/venv.xhtml) alone with [Docker](https://www.docker.com/)?
    
*   How could an AI Pair Programming workflow enhance the productivity life-cycle of MLOPs?
    
*   What are the core principles of DevOps, and how can you test them to ensure they have been implemented successfully as a pre-condition for doing MLOps?
    
*   Why could there be a significant advantage to using GPU as part of containers such as this command: `docker run --gpus all -it --rm tensorflow/tensorflow:latest-gpu \ python -c "import tensorflow as tf; print(tf.reduce_sum(tf.random.normal([1000, 1000])))"`
    

^([1](#ch09.xhtml#idm46247689795440-marker)) A recent [example covered in the New York Times](https://www.nytimes.com/2022/08/21/technology/google-surveillance-toddler-photo.xhtml) explains AI systems can create false positives with devastating consequences.

^([2](#ch09.xhtml#idm46247689750256-marker)) In the podcast [“How much does the future matter"'](https://www.samharris.org/podcasts/making-sense-episodes/292-how-much-does-the-future-matter), William MacAskill raised the point that the majority of advanced machine learning researchers place “a 2.5% chance of AI in the next twenty years causing either death or disempowerment in your lifetime. This statistic is more significant than your chance of dying in a car crash.” A key question is whether recommendation engines are in this category of AI prone to catastrophic risk

# Chapter 10. Implementing MLOps using Rust

Operational efficiency must be at the core of any technology system. MLOps builds upon DevOps, which builds on the concept of *kaizen*, the Japanese word for continuous improvement. Without continuous improvement, you wouldn’t have DevOps or, by extension, MLOps.

At the heart of continuously improving operations is a simple question: “Can we improve operational performance—from training and inference to packaging and delivery—by ten times or more?” If the answer is yes, as it will be with many organizations using Python for data science, the next question should be: “Why are we not doing it?”

Organizations have had few options for decades besides pure C or C` and Python for machine learning solutions. C` may provide more efficiency in terms of performance, but Python is generally easier to learn, implement, and maintain, which is why Python has taken off in data science. The hard choice between the performant but complex C++ and the easy-to-learn but comparatively slow Python ultimately results in many companies choosing Python.

But there is another way. Rust consistently ranks among the [most performant and energy-efficient languages](https://greenlab.di.uminho.pt/wp-content/uploads/2017/10/sleFinal.pdf). It is also among the most loved languages in [StackOverflow’s annual developer survey](https://survey.stackoverflow.co/2022/). Though some Python libraries widely used in data science are written in C and can provide some of the performance benefits of running a compiled language, Rust provides a more direct route to bare metal while using a single language.

Rust is also far easier to learn and use than C or C++, which makes it a realistic solution for those who want the performance of a compiled language. That’s especially true when using [GitHub Copilot](https://github.com/features/copilot). This AI-powered pair programming assistant uses the OpenAI Codex to suggest code and entire functions in real time to developers while they code. Let’s discuss this strategy next.

###### Note

The phrase *AI-powered pair programming assistant* refers to an improvement over the classic pair programming style where you sit next to another developer and write code together. With emerging developer tools like [GitHub Copilot X](https://github.blog/2023-03-22-github-copilot-x-the-ai-powered-developer-experience/), you can chat with a coding assistant to get ideas on a coding project as well as get coding suggestions as you type.

# The Case for Rust for MLOps

GitHub Copilot is a revolutionary new change in the way developers work. GitHub Copilot and tools like it are game changers since they minimize the impact of syntax on productivity. With Rust, you spend more time compiling code, which is an investment in future returns, much like saving for the future in a retirement account. Rust has excellent performance and safety, but the syntax can be challenging. With GitHub Copilot, the syntax becomes less of an issue since the suggestions eliminate many of the difficulties in programming. Additionally, because of the robustness of the Rust toolchain for linting, formatting, and compiling, any errors or false starts from GitHub Copilot are caught by these tools, making the combination of Rust and GitHub Copilot an emergent front-runner in AI-assisted coding.

###### Note

There are several reasons to consider Rust other than performance. Rust is a modern language that first appeared in 2010. It lacks the baggage that older languages carry, but it is mature enough that we can rest assured it isn’t going anywhere anytime soon. Further, other trends are supporting a hard look at Rust.

Rust was designed from the ground up to support modern computing capabilities, like multi-core threads, that are often “bolted on” to older languages like Python. By designing the language to support these features from the start, Rust can avoid awkwardness in many other languages. A great example of how simple multi-core threads are in Rust is the following snippet from the [Rust rayon library](https://docs.rs/rayon/latest/rayon/iter/index.xhtml):

```
    use rayon::prelude::*;
    fn sum_of_squares(input: &[i32]) -> i32 {
        input.par_iter()
             .map(|i| i * i)
             .sum()
    }
```

There are no gimmicks or hacks to the code; the threads “just work” across all the machine cores, and the code is just as readable as Python.

Likewise, Rust natively supports typing, so the entire toolchain, from the linter to the editor to the compiler, can leverage this capability. Rust also makes packaging a breeze. Cargo provides a Python-esque “one obvious way” to install packages.

Of course, there are still areas where Python excels. It’s fantastic for API documentation and readability in general. If you need to try out an idea, it is hard to beat using Python in an interactive prompt, like [IPython](https://ipython.org/), to explore a concept. But MLOps is more sensitive to performance requirements than other data science fields and are heavily dependent on software engineering best practices conducive to implementation in Rust. A new superset of Python called [Mojo](https://github.com/modularml/mojo) might solve many performance and deployment issues soon, but it’s still in development while Rust is available here and now.

One common objection to using Rust is that it doesn’t have as large and established an ecosystem as Python for working with data. But remember that this ecosystem isn’t necessarily optimal for the needs of MLOps. In particular, the stack I call #jcpennys (Jupyter, Conda, Pandas, Numpy, Sklearn) is straight from academia, heavyweight, and optimized for use with small data. In academics, much is to be said for a “God environment” with everything in one spot. But in real-world production MLOps, you don’t want extra packages or brittle tools that are difficult to test, like notebooks. Meanwhile, the Rust ecosystem is growing. For example, [Polars](https://www.pola.rs/) is a performant data frame library taking the data space by storm.

## Leveling Up with Rust, GitHub Copilot, and Codespaces

Next, look at how you can use the GitHub ecosystem to level up to a more robust language in Rust [Figure 10-1](#ch10.xhtml#Figure-11-01-prompt-engineering). Prompt engineering occurs when you ask GitHub Copilot in step B to generate code; you then test out the idea for a CLI to ensure it works and clean up any suggestions with a series of Makefile commands.

![](assets/11-01-prompt-engineering.png)

All Rust projects can follow this pattern:

1.  Create a new repo using [Rust New Project Template](https://github.com/noahgift/rust-new-project-template).
    
2.  Next, create a new [Codespace](https://github.com/features/codespaces) and use it.
    
3.  Use *main.rs* to call the handle CLI and *lib.rs* to handle logic and import `clap` in *Cargo.toml* as shown in this project.
    
4.  Use `cargo init --name 'hello'` or whatever you want to call your project.
    
5.  Put your “ideas” in as comments in Rust to seed GitHub Copilot, i.e add a comment as shown: `//build an add function`
    
6.  Run `make format` i.e. `cargo format`
    
7.  Run `make lint` i.e. `cargo clippy --quiet`
    
8.  Run project: `cargo run -- --help`
    
9.  Push your changes to allow GitHub Actions to: `format` check, `lint` check, and other actions like binary deploy.
    

###### Note

You can view two videos showing this workflow here. - [Direct link to Video](https://www.youtube.com/watch?v=U_qyJZfiMLo) - [Direct link to Repo in video](https://github.com/nogibjj/hello-rust-template-example).

Here’s an [example repository](https://github.com/nogibjj/hello-rust). A good starting point for a new Rust project is the following pattern:

To run: `cargo run -- marco --name "Marco"` Be careful to use the *name* of the project in the `Cargo.toml` to call `lib.rs` as in:

```
[package]
name = "hello"
```

For example, see the name `hello` invoked alongside `marco_polo`, which is in *lib.rs* code:

```
/* A Marco Polo game. */

/* Accepts a string with a name.
If the name is "Marco", returns "Polo".
If the name is "any other value", it returns "Marco".
*/
pub fn marco_polo(name: &str) -> String {
    if name == "Marco" {
        "Polo".to_string()
    } else {
        "Marco".to_string()
    }
}
```

`main.rs` code:

```
fn main() {
    let args = Cli::parse();
    match args.command {
        Some(Commands::Marco { name }) => {
            println!("{}", hello::marco_polo(&name));
        }
        None => println!("No command was used"),
    }
}
```

This style is a new emerging pattern ideal for systems programming in Rust, as certain combinations lead to further advances. For example, steel is a composite of iron and carbon, making a new substance stronger and more rigid than iron. Similarly, GitHub Copilot’s suggestions, a next-generation compiled language like Rust, and its ecosystem of formatting, linting, and packaging tools can lead to a more robust software development experience than Python.

Let’s consider some potential benefits that Rust could bring to the world of MLOps:

*   *Performance*: Rust has a very efficient memory model with no garbage collector, which can significantly increase the speed of your MLOps pipelines. This capability is critical for MLOps tasks that quickly handle large volumes of data.
    
*   *Concurrency*: Rust’s memory safety guarantees enable safe concurrency, enabling you to quickly leverage multiple cores to speed up your processing tasks.
    
*   *Interoperability*: Rust has excellent interoperability with C and can call C libraries directly. This capability could allow MLOps developers to leverage existing C libraries for numerical computation and machine learning tasks.
    
*   *Security*: Rust’s emphasis on memory and type safety can lead to more secure applications. This process is crucial for MLOps tasks where security is critical, such as healthcare or finance.
    
*   *Robustness*: Rust’s compile-time error checking can catch many errors before your code runs. This process can lead to more robust MLOps pipelines less prone to runtime errors.
    
*   *Developer productivity*: Rust has a steeper learning curve than Python, so tools like GitHub Copilot can help you write Rust code more quickly and easily.
    

Of course, Rust is not a silver bullet, and Python will continue to play an essential role in MLOps. However, for MLOps tasks that need the performance, security, and robustness that Rust can provide, Rust is an option worth considering.

Another bolt-on problem with Python is the packaging. Even though the Python standard library includes two tools that make it relatively straightforward to install packages in [pip](https://pip.pypa.io/en/stable/) and [virtualenv](https://virtualenv.pypa.io/en/latest/) as I [demonstrate in a Python MLOPs repository](https://github.com/nogibjj/mlops-template), there is an explosion of tools to handle “edge cases.” Ironically, the [Zen of Python](https://peps.python.org/pep-0020/), actually cautioned against this decades ago in the statement, “There should be one-- and preferably only one --obvious way to do it”. Python means *except packaging*, which has an almost exponential way of doing things. Additionally, distributing a binary command-line tool efficiently in Python is non-trivial since this capability is not part of the language; this is different from a default workflow included in the language, like it is with Rust or Go. An excellent example of good binary tool distribution is the [Hugo framework](https://gohugo.io/) written in Go. One reason it is so popular is it is easy to install.

These critiques of Python don’t mean it is an imperfect solution to what it excels at; it just isn’t *the* solution to every single problem. Python is fantastic for API documentation and readability in general. If you need to try out an idea, it is hard to beat using Python in an interactive prompt, like [IPython](https://ipython.org/), to explore a concept. It struggles with packaging, performance, and language safety, all things Rust excels at doing. This strength is why Rust is an ideal language for enterprise MLOps.

What could be the alternative for MLOps if there is a better solution than academic “data science” tools?

*   Is there a more performant Data Frame library? (Rust has [Polars](https://www.pola.rs/)).
    
*   Why not have a [compiler to optimize code](https://rustc-dev-guide.rust-lang.org/overview.xhtml)?
    
*   Why not have a simple packaging solution with “one way to install” ([Rust has Cargo](https://doc.rust-lang.org/cargo/))?
    
*   Why not have a breakneck computational speed for ML ([some benchmarking shows 25X speed improvements](https://www.secondstate.io/articles/performance-rust-wasm/))?
    
*   Why not be able to write both for the [Linux Kernel](https://thenewstack.io/rust-in-the-linux-kernel/) and general-purpose scripting?
    
*   Why not see if there is a better solution than Python (which is essentially two languages scientific python and regular Python)?
    
*   Python is more or less the least green language in energy efficiency, and Rust is more or less the best. With the rise of machine learning, it is important to consider carbon footprint and sustainability goals.
    

###### Note

O’Reilly has several fantastic books on or involving Rust worth referring to if you want to dive deeper into Rust. These include the following books:

*   [Command-Line Rust](https://learning.oreilly.com/library/view/command-line-rust/9781098109424/)
    
*   [Programming Rust, 2nd Edition](https://learning.oreilly.com/library/view/programming-rust-2nd/9781492052586/)
    
*   [WebAssembly: The Definitive Guide](https://learning.oreilly.com/library/view/webassembly-the-definitive/9781492089834/) (Includes Rust Web Assembly examples).
    

Two GitHub repositories you might look at are [brief Rust tutorial](https://github.com/nogibjj/rust-tutorial) and [Rust MLOps template](https://github.com/nogibjj/rust-mlops-template). With the case for Rust made theoretically, let’s dive into the details in more detail next.

## In The Beginning Was the Command-Line

What could MLOps and data science look like in the near future and beyond without Jupyter Notebook and complex install tools as the center of the universe? It could be the command line. Initially, it was the command line at the beginning of computing in the 1970s and 1980s, and it may be the best solution for the domain of MLOps.

Science fiction author Neal Stephensen wrote an essay in 1999 called “In the Beginning …​Was the Command Line”. The following snippet highest his thoughts at the time on the emergence of GUI-based systems: “What would the engineer say after you had explained your problem and enumerated all the dissatisfactions in your life? He would probably tell you that life is a very hard and complicated thing; that no interface can change that; that anyone who believes otherwise is a sucker; and that if you don’t like having choices made for you, you should start making your own.”

Similarly, with the constantly evolving domain of MLOPs, it is easy to get caught in the trap of thinking of a notebook-based workflow alone as the solution to every problem. In a recent seminar where Noah discussed MLOps and some of the drawbacks of notebooks, one data scientist “cutting and pasting” code from a notebook to a script file. Back in the early days of web development in the 1990s, the concept of cut-and-paste coding debunked itself as an anti-pattern, and here in 2023, there are advocates for it in MLOps with a straight face!

Ultimately the flexibility of command-line interfaces, coupled with systems programming approaches like those with C, C++, Go, and Rust, are too valuable to ignore, especially if the language is approachable by a developer that is more familiar with high-level languages. According to a [newstack article](https://thenewstack.io/rust-in-the-linux-kernel/) on Rust for the Linux kernel, in April of 2021, a developer Ojeda introduced a Request for Comment (RFC) on making Rust a part of Linux. Linus Torvalds, the creator of Linux, said, “Unless something odd happens, it \[Rust\] will make it into 6.1.” (i.e., late 2022 or early 2023). And this happened in 6.1, and Rust has continued updates in each subsequent release.

Similarly, Amazon is active in open-source Rust development via the [Firecracker project](https://firecracker-microvm.github.io/), which is “an open-source virtualization technology.” One example of the power of this technology is a [firecracker demo repo](https://github.com/firecracker-microvm/firecracker-demo), which shows 4000 simultaneous VMs launching. Amazon also has an alpha release of a [Rust SDK on GitHub](https://github.com/awslabs/aws-sdk-rust).

Finally, because of how easy Rust makes command-line tools to distribute, doing MLOPs for the CLI is an optimal use case for Rust. One of the more remarkable examples is the [diffusers-rs project](https://github.com/LaurentMazare/diffusers-rs) that invokes Stable Diffusion using Rust and [libtorch](https://pytorch.org/get-started/locally/).

```
cargo run --example stable-diffusion --features clap --\
 --prompt "A rusty robot using the command-line terminal and throwing\
away notebooks"
```

The reason solutions like the Stable Diffusion example exist along with bindings for [Rust with PyTorch](https://github.com/LaurentMazare/tch-rs/) is the exponential growth of Rust modules, as shown in [Figure 10-2](#ch10.xhtml#Figure-8-4-rust-modules).

![](assets/8-4-rust-modules.png)

Now that the case for Rust for MLOps exists, let’s dive into getting started with Rust itself.

# Getting Started with Rust for MLOps

Rust is one of the more accessible languages from an installation perspective. You run the [rustup](https://rustup.rs/) command, and that is all you need to install! Another option is to use GitHub Codespaces as a development environment. One of the advantages of codespaces is that it has a generous free tier and allows for easy customization. You can create a new repository for developing with Rust using the Rust [new project template](https://github.com/noahgift/rust-new-project-template) we made available for this book.

Once you install, check to see if things work by running the following command: `rustc --version`

Another option is to [create a Makefile](https://github.com/noahgift/rust-new-project-template/blob/main/Makefile#L1-L7) and put key commands in it such as `make rust-version`, which checks both the `cargo` and `rust` version. You can see several tools help you get things done in Rust:

```
rust-version:
	@echo "Rust command-line utility versions:"
	rustc --version 			#rust compiler
	cargo --version 			#rust package manager
	rustfmt --version			#rust code formatter
	rustup --version			#rust toolchain manager
	clippy-driver --version		#rust linter
```

To run everything locally in the future, you could do `make all`, which will format/lint/test all projects [in this repository](https://github.com/noahgift/rust-new-project-template/blob/main/Makefile#L24).

Next, to build a hello-world example, you can use the built-in `cargo` command. Cargo is one of the most substantial and valuable parts of the Rust ecosystem.

Create a project directory: `cargo new hello`. The cargo command creates a structure you can see with `tree hello`:

```
hello/
├── Cargo.toml
└── src
    └── main.rs
1 directory, 2 files
```

The *Cargo.toml* file is where the project configuration lives, i.e., if you need to add a dependency. The source code file has the following content in *main.rs*. It looks like Python or any other modern language, and this function prints a message.

```
fn main() {
    println!("Hello, world MLOPs!");
}
```

To run the project, you `cd` into *hello* and run `cargo run`. The output looks like the following:

```
@noahgift > /workspaces/rust-mlops-template/hello (main) $ cargo run
   Compiling hello v0.1.0 (/workspaces/rust-mlops-template/hello)
    Finished dev [unoptimized + debuginfo] target(s) in 0.36s
     Running `target/debug/hello`
Hello, world MLOPs!
```

Finally, run without all the noise: `cargo run --quiet`. If you want to run the binary created, the following command executes it `./target/debug/hello`. It is important to note how awesome this is coming from Python because the binary distribution of your code comes for free. In Python, the concept of binary executable isn’t a workflow supported by native Python. Instead, the closest way to achieve the same portable executable would require packaging Python in a Docker workflow.

A big takeaway with this hello world project is that the Rust ecosystem takes care of many complex programming parts, namely linting, testing, formatting, and binary deployment within Rust [Cargo package manager](https://doc.rust-lang.org/cargo/). If the Rust code passes lint and compiles, it should work when you run it. You cannot say the same for Python since there is no compiler. In addition to being more reliable, it gives you “C-level” speed with a similar readable syntax as Python. Finally, many features of the language, like immutable variables and rational concurrency design, make the code safer because the compiler will not compile code that isn’t safe.

There is yet one more tool to make Rust programming more effective, and this is by including [GitHub Copilot](https://github.com/features/copilot), or a similar tool like [Amazon CodeWhisperer](https://aws.amazon.com/codewhisperer/) in the initial phase of code creation. The synergy of Copilot with the robust Cargo ecosystem is a recipe for productivity, as shown in [Figure 10-3](#ch10.xhtml#Figure-8-5-modern-rust-development).

![](assets/8-5-modern-rust-development.png)

Further, by including automation in the workflow via [GitHub Actions](https://github.com/features/actions), these necessary automation steps also occur as part of CI/CD. The foundation of MLOps is the automation of software engineering best practices happening at the build system. This core capability enables further automation down the line:

```
name: Rust CI/CD Pipeline
on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]
env:
  CARGO_TERM_COLOR: always
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v1
    - uses: actions-rs/toolchain@v1
      with:
          toolchain: stable
          profile: minimal
          components: clippy, rustfmt
          override: true
    - name: update linux
      run: sudo apt update
    - name: update Rust
      run: make install
    - name: Check Rust versions
      run: make rust-version
    - name: Format
      run: make format
    - name: Lint
      run: make lint
    - name: Test
      run: make test
```

One of the reasons a `Makefile` is an excellent component in a project is that the steps locally, installing, linting, formatting, and testing, run in the same way on the build system. This methodology eliminates potential errors in building software automatically.

This style of Rust development is both new and an essential advancement in developer productivity. With tools like GitHub Copilot and AWS Codewhisper, switching to a higher-performance language like Rust, C#, or Go can be more manageable. This new form of software engineering enables *prompt engineering* as a valid first phase in software development. Next, the code formatted by the Rust Cargo tool allows a “fresh look” from a service like Copilot, potentially increasing the quality of the recommendation.

Once that phase completes, the Cargo lint tool Clippy further digs into the quality of the code and enhances it. Finally, running tests ensure the business logic works, and then a last step of compilation makes optimal and safe code.

Next, let’s go beyond just a simple hello-world tool and get into PyTorch and Hugging Face.

# Using PyTorch and Hugging Face with Rust

[Hugging Face](https://huggingface.co/) is an emerging platform for building solutions with large language Models (LLMs), and it works with Rust and PyTorch. For this example, I put together a reasonably realistic demo of the type of tool that would be useful using a pre-trained summarization model; the example [project lives here](https://github.com/noahgift/rust-mlops-template/tree/main/sqlite-hf). The general structure of this demo is as follows. The lyrics for “En El Muelle De San Blas” a song by Maná, are in the *lyrics.txt* file.

```
├── Cargo.toml
├── Makefile
├── README.md
├── lyrics.txt
└── src
  ├── lib.rs
  └── main.rs

2 directories, 6 files
```

You can see the general architecture in [Figure 10-4](#ch10.xhtml#Figure-8.9-sqlite-hugging-face-pytorch). The idea is the performance of Rust alongside an SQLite database, and Hugging Face is a repeatable pattern to analyze song lyrics using a pre-trained LLM.

![](assets/8.9-sqlite-hugging-face-pytorch.png)

A common problem in natural language processing (NLP) is building solutions that analyze text. The following example shows how to make a reproducible, efficient, and secure solution to analyze song lyrics.

```
use rust_bert::pipelines::sequence_classification::Label;
use rust_bert::pipelines::zero_shot_classification::ZeroShotClassificationModel;
use std::fs::File;
use std::io::BufRead;
use std::io::BufReader;

fn create_db() -> sqlite::Connection {
    let db = sqlite::open(":memory:").unwrap();
    db.execute(
        "CREATE TABLE zeroshotcandidates (id INTEGER PRIMARY KEY, label TEXT)",
    )
    .unwrap();
    ["rock", "pop", "hip hop", "country", "latin"].iter().for_each(|&x| {
        db.execute(&format!("INSERT INTO zeroshotcandidates (label) VALUES ('{}')", x))
            .unwrap();
    });
    db
}

pub fn get_all_zeroshotcandidates() -> Vec<String> {
    let db = create_db();
    let mut candidates: Vec<String> = Vec::new();
    db.iterate("SELECT label FROM zeroshotcandidates", |pairs| {
        for &(_column, value) in pairs.iter() {
            candidates.push(value.unwrap().to_string());
        }
        true
    })
    .unwrap();
    candidates
}

pub fn read_lyrics(file: &str) -> Vec<String> {
    let mut lyrics: Vec<String> = Vec::new();
    let file = File::open(file).expect("Unable to open file");
    let reader = BufReader::new(file);
    for line in reader.lines() {
        lyrics.push(line.unwrap());
    }
    lyrics
}

pub fn classify_lyrics(lyrics: Vec<String>) -> Vec<Vec<Label>> {
    let temp_candidates = get_all_zeroshotcandidates();
    let candidate_labels: Vec<&str> = temp_candidates.iter().map(|s| s.as_str()).collect();
    let lyrics: String = lyrics.join(" ");
    let lyrics: &str = lyrics.as_ref();
    let zero_shot_model = ZeroShotClassificationModel::new(Default::default()).unwrap();
    zero_shot_model.predict_multilabel([lyrics], candidate_labels, None, 128)
}
```

To run everything, you see the different commands exposed via `cargo`:

```
cargo run -- candidates
    Finished dev [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/sqlitehf candidates`
1
rock
2
pop
3
hip hop
4
country
5
latin
```

```
cargo run -- lyrics
   Compiling sqlitehf v0.1.0 (/Users/noahgift/src/rust-mlops-template/sqlite-hf)
    Finished dev [unoptimized + debuginfo] target(s) in 0.76s
     Running `target/debug/sqlitehf lyrics`
Lyrics lyrics.txt
Uh-uh-uh-uh, uh-uh
Ella despidió a su amor
```

```
@noahgift > /workspaces/rust-mlops-template/sqlite-hf (main) $ cargo run -- classify
   Compiling sqlitehf v0.1.0 (/workspaces/rust-mlops-template/sqlite-hf)
    Finished dev [unoptimized + debuginfo] target(s) in 8.76s
     Running `target/debug/sqlitehf classify`
Classify lyrics.txt
rock: 0.06948944181203842
pop: 0.27735018730163574
hip hop: 0.034089818596839905
country: 0.7835917472839355
latin: 0.6906086802482605
```

Yet another example of Rust for MLOps is using PyTorch to load pre-trained models and create image predictions as shown in the *main.rs* [file](https://raw.githubusercontent.com/noahgift/rust-mlops-template/main/pytorch-rust-example/src/main.rs). The PyTorch bindings are straightforward to turn into a pre-trained model tool:

```
/*
Hello world Rust pytorch
Download pre-trained model here:
https://github.com/LaurentMazare/tch-rs/releases/download/mw/resnet18.ot
*/

use anyhow::{bail, Result};
use tch::nn::ModuleT;
use tch::vision::{resnet, imagenet};

pub fn main() -> Result<()> {
    let args: Vec<_> = std::env::args().collect();
    let (weights, image) = match args.as_slice() {
        [_, w, i] => (std::path::Path::new(w), i.to_owned()),
        _ => bail!("usage: main resnet18.ot image.jpg"),
    };
    // Load the image file and resize it to the usual imagenet dimension
    // of 224x224.
    let image = imagenet::load_image_and_resize224(image)?;

    // Create the model and load the weights from the file.
    let mut vs = tch::nn::VarStore::new(tch::Device::Cpu);
    let net: Box<dyn ModuleT> = match
        weights.file_name().unwrap().to_str().unwrap() {
        "resnet18.ot" => Box::new(resnet::resnet18(
            &vs.root(), imagenet::CLASS_COUNT)),
        _ => bail!("unknown model, use a weight file named e.g. resnet18.ot"),
    };
    vs.load(weights)?;

    // Apply the forward pass of the model to get the logits.
    let output = net.forward_t(
        &image.unsqueeze(0), /* train= */ false
    ).softmax(-1, tch::Kind::Float); // Convert to probability.

    // Print the top 5 categories for this image.
    for (probability, class) in imagenet::top(&output, 5).iter() {
        println!("{:50} {:5.2}%", class, 100.0 * probability)
    }
    Ok(())
}
```

To run this example, do the following:

Pre-trained model: `cd` into *pytorch-rust-example* and then run: `cargo run -- resnet18.ot Walking_tiger_female.jpg`. You can see the results in [Figure 10-5](#ch10.xhtml#Figure-8-10-pytorch-invocation).

![](assets/8-10-pytorch-invocation.png)

# Using Rust to Build Tools for MLOps

With some solid ideas on using Rust for deep learning, let’s look at the ecosystem around Rust as it relates to MLOPs.

## Building Containerized Rust Command-Line Tools

Another capability of Rust is containerizing command-line tools. Let’s look at a regular containerized command-line tool and a Rust command-line tool with PyTorch.

The repo for the project is [here](https://github.com/noahgift/rust-docker-cli). The *lib.rs* file holds a function that returns the string “Polo” if the string “Marco” passes in.

```
/* A Marco Polo game. */

/* Accepts a string with a name.
If the name is "Marco", returns "Polo".
If the name is "any other value", it returns "Marco".
*/
pub fn marco_polo(name: &str) -> String {
    if name == "Marco" {
        "Polo".to_string()
    } else {
        "Marco".to_string()
    }
}
```

Next, to invoke the command-line tool, the same pattern as most examples works in this chapter where the library contains the logic, and the main maps the functions to subcommands, in this case, the subcommand `Play`:

```
//A command-line tool to play Marco Polo
use clap::Parser;

#[derive(Parser)]
#[clap(version = "1.0", author = "Noah Gift", about = "A Marco Polo game")]
struct Cli {
    #[clap(subcommand)]
    command: Option<Commands>,
}

#[derive(Parser)]
enum Commands {
    #[clap(version = "1.0", author = "Noah Gift")]
    Play {
        #[clap(short, long)]
        name: String,
    },
}

fn main() {
    let args = Cli::parse();
    match args.command {
        Some(Commands::Play { name }) => {
            let result = containerized_marco_polo_cli::marco_polo(&name);
            println!("{}", result);
        }
        None => println!("No subcommand was used"),
    }
}
```

Finally, the Dockerfile is [tiny](https://github.com/noahgift/rust-docker-cli/blob/main/Dockerfile) to turn this project into a containerized tool. The first section of the Dockerfile builds the project, and then a smaller container image, `debian:buster-slim` allows for a reduced footprint:

```
FROM Rust:latest as builder
ENV APP containerized_marco_polo_cli
WORKDIR /usr/src/$APP
COPY . .
RUN cargo install --path .

FROM debian:buster-slim
RUN apt-get update && rm -rf /var/lib/apt/lists/*
COPY --from=builder /usr/local/cargo/bin/$APP /usr/local/bin/$APP
ENTRYPOINT [ "/usr/local/bin/containerized_marco_polo_cli" ]
```

Building and running the container is straightforward now.

```
docker build -t marco-polo .
docker run --rm -it marco-polo --help
docker run --rm -it marco-polo play --name Marco
Polo
```

With this knowledge in our toolkit, let’s package our previous PyTorch Rust pre-trained model into a container. The complete example project lives [here](https://github.com/noahgift/rust-mlops-template/tree/main/pytorch-rust-docker).

The main [relevant addition](https://github.com/noahgift/rust-mlops-template/blob/main/pytorch-rust-docker/Dockerfile) is the Dockerfile which leverages the existing Cargo ecosystem to install PyTorch.

```
FROM Rust:latest as builder
ENV APP pytorch-rust-docker
WORKDIR /usr/src/$APP
COPY . .
RUN apt-get update && rm -rf /var/lib/apt/lists/*
RUN cargo install --path .
RUN cargo build -j 6
```

The steps to invoke this container involve running the following commands to build and run it:

```
docker build -t pytorch-rust-docker .
docker run -it pytorch-rust-docker
#runs inside of container
cargo run -- resnet18.ot Walking_tiger_female.jpg
```

## GPU PyTorch Workflows

The synergy of performance gains from Rust really shines when a GPU comes into the mix. This [following repository](https://github.com/nogibjj/rust-pytorch-gpu-template) has many repeatable GPU examples thanks to the excellent toolchain from GitHub Codespaces.

As long as the environmental variable `export TORCH_CUDA_VERSION=cu117` exists, `cargo` builds an NVidia Cuda project version. The following MNIST (Modified National Institute of Standards and Technology database) project shows a very brief [training snippet](https://github.com/nogibjj/rust-pytorch-gpu-template/blob/main/pytorch-mnist/src/mnist_conv.rs):

```
// CNN model. This should reach 99.1% accuracy.

use anyhow::Result;
use tch::{nn, nn::ModuleT, nn::OptimizerConfig, Device, Tensor};

#[derive(Debug)]
struct Net {
    conv1: nn::Conv2D,
    conv2: nn::Conv2D,
    fc1: nn::Linear,
    fc2: nn::Linear,
}

impl Net {
    fn new(vs: &nn::Path) -> Net {
        let conv1 = nn::conv2d(vs, 1, 32, 5, Default::default());
        let conv2 = nn::conv2d(vs, 32, 64, 5, Default::default());
        let fc1 = nn::linear(vs, 1024, 1024, Default::default());
        let fc2 = nn::linear(vs, 1024, 10, Default::default());
        Net { conv1, conv2, fc1, fc2 }
    }
}

impl nn::ModuleT for Net {
    fn forward_t(&self, xs: &Tensor, train: bool) -> Tensor {
        xs.view([-1, 1, 28, 28])
            .apply(&self.conv1)
            .max_pool2d_default(2)
            .apply(&self.conv2)
            .max_pool2d_default(2)
            .view([-1, 1024])
            .apply(&self.fc1)
            .relu()
            .dropout(0.5, train)
            .apply(&self.fc2)
    }
}

pub fn run() -> Result<()> {
    let m = tch::vision::mnist::load_dir("data")?;
    let vs = nn::VarStore::new(Device::cuda_if_available());
    let net = Net::new(&vs.root());
    let mut opt = nn::Adam::default().build(&vs, 1e-4)?;
    for epoch in 1..100 {
        for (bimages, blabels) in m.train_iter(256)
            .shuffle()
            .to_device(vs.device()) {
            let loss = net.forward_t(&bimages, true)
                .cross_entropy_for_logits(&blabels);
            opt.backward_step(&loss);
        }
        let test_accuracy = net
            .batch_accuracy_for_logits(
                &m.test_images, &m.test_labels, vs.device(), 1024);
        println!("epoch: {:4} test acc: {:5.2}%",
            epoch, 100. * test_accuracy,);
    }
    Ok(())
}
```

To run, you `cd` into the *pytorch-mnist* directory and run the following code: `cargo run -- conv`. The result shows a lightening fast training of a model as shown in [Figure 10-6](#ch10.xhtml#Figure-8-13-mnist-pytorch). The `nvidia-smi -l 1` command enabled GPU monitoring, verifying that the GPU is doing the heavy lifting.

![](assets/8-13-mnist-pytorch.png)

Yet another effective Rust GPU workflow is to run the latest version of Stable Diffusion:

*   Clone this [repo](https://github.com/LaurentMazare/diffusers-rs).
    
*   Follow these [setup instructions](https://github.com/LaurentMazare/diffusers-rs#clip-encoding-weights):
    

After completing the previous steps, the download of the model weights and then run:

`cargo run --example stable-diffusion --features clap -- --prompt "A very rusty robot holding a fire torch to notebooks"`

Yet again, you see the power of Rust and GPU as the GPU gets completely saturated, as shown in [Figure 10-7](#ch10.xhtml#Figure-8-14-stable-diffusion-gpu).

![](assets/8-14-stable-diffusion-gpu.png)

The result in [Figure 10-8](#ch10.xhtml#Figure-8-15-flame-robot) is a robot picture that can easily integrate into a complex pipeline due to the high-performance nature of Rust.

![](assets/8-15-flame-robot.png)

## Using Tensorflow Rust

You don’t need only to use PyTorch. To run Tensorflow with Rust, look at this example in this [repo](https://github.com/noahgift/rust-mlops-template/tree/main/tf-rust-example).

First, look at the *Cargo.toml* [file](https://github.com/noahgift/rust-mlops-template/blob/main/tf-rust-example/Cargo.toml). Notice that the `tensorflow` crate is all needed to get started:

```
[package]
name = "tf-rust-cli"
version = "0.1.0"
edition = "2021"
[dependencies]
tensorflow = "0.19.1"
```

Next, in the *main.rs* [file](https://github.com/noahgift/rust-mlops-template/blob/main/tf-rust-example/src/main.rs), a simple example involves only a few lines of code. To see more ideas, visit the official crate [documentation page](https://crates.io/crates/tensorflow), including enabling GPU support (something easily accomplished with GitHub Codespaces with GPU):

```
/*Rust Tensorflow Hello World */

extern crate tensorflow;
use tensorflow::Tensor;

fn main() {
    let mut x = Tensor::new(&[1]);
    x[0] = 2i32;
    //print the value of x
    println!("{:?}", x[0]);
    //print the shape of x
    println!("{:?}", x.shape());
    //create a multidimensional tensor
    let mut y = Tensor::new(&[2, 2]);
    y[0] = 1i32;
    y[1] = 2i32;
    y[2] = 3i32;
    y[3] = 4i32;
    //print the value of y
    println!("{:?}", y[0]);
    //print the shape of y
    println!("{:?}", y.shape());
}
```

## Doing KMeans Clustering with Rust

There are also high-performance scientific libraries for machine learning in Rust. An excellent example of a library is [linfa](https://crates.io/crates/linfa). There are many benchmarked examples.

Simple examples show a similar amount of code as machine learning in Python using [scikit-learn](https://scikit-learn.org/stable/). A key and critical point from the library author is the statement, “No need to have a second language for performance reasons,” as in you don’t need C and Python; everything is fast with just Rust.

The following example from their website shows just how simple and clean the code looks:

```
let (train, valid) = linfa_datasets::diabetes()
    .split_with_ratio(0.9);

// train pure LASSO model with 0.1 penalty
let model = ElasticNet::params()
    .penalty(0.1)
    .l1_ratio(1.0)
    .fit(&train)?;

println!("z score: {:?}", model.z_score());

// validate
let y_est = model.predict(&valid);
println!("predicted variance: {}", y_est.r2(&valid)?);
```

There is a blog post of their k-means example [which shows 25 times faster inference](https://github.com/LukeMathWalker/clustering-benchmarks) than scikit-learn. You can see a command-line tool example Noah created in the following [repository](https://github.com/noahgift/rust-mlops-template/tree/main/linfa-kmeans).

# Final Notes on Rust

Let’s discuss a few additional notes on Rust next.

## Ruff Linter

A new rust-based linter called [ruff](https://github.com/charliermarsh/ruff) can significantly decrease the time it takes to lint a codebase. You can see in [Figure 10-9](#ch10.xhtml#Figure-5-6-ruff) that ruff gives speed improvements anywhere from 12-120x regular python.

![](assets/5-6-ruff.png)

According to the author, the added speed in linting makes “instant” possible for millions of lines of code. In a [blog post about the tool](https://notes.crmarsh.com/python-tooling-could-be-much-much-faster), he wisely says, “Ultimately, my goal with Ruff is to get the Python ecosystem to question the status quo. How long should it take to lint a million lines of code? In my opinion: it should be instant. And if your developer tools were instant, what would that unlock?”

## rust-new-project-template

For beginners, a good idea is for Rust projects to follow this pattern:

1.  Create a new repo using Rust New Project Template: [*https://github.com/noahgift/rust-new-project-template*](https://github.com/noahgift/rust-new-project-template)
    
2.  Create a new Codespace and use it
    
3.  Use `main.rs` to call the handle CLI and `lib.rs` to handle logic and import `clap` in `Cargo.toml` as shown in this project.
    
4.  Use `cargo init --name 'hello'` or whatever you want to call your project.
    
5.  Put your “ideas” in as comments in Rust to seed GitHub Copilot, i.e //build anadd function
    
6.  Run `make format` i.e. `cargo format`
    
7.  Run `make lint` i.e. `cargo clippy --quiet`
    
8.  Run project: `cargo run -- --help`
    
9.  Push your changes to allow GitHub Actions to: `format` check, `lint` check, and other actions like binary deploy.
    

This pattern is a new emerging pattern and is ideal for systems programming in Rust.

Repo example here: [*https://github.com/nogibjj/hello-rust*](https://github.com/nogibjj/hello-rust)

With the previous style in place, you can then do the following:

To run: `cargo run -- marco --name "Marco"` Be careful to use the NAME of the project in the `Cargo.toml` to call `lib.rs` as in:

```
[package]
name = "hello"
```

For example, see the name `hello` invoked alongside `marco_polo`, which is in `lib.rs`.

`lib.rs` code:

```
/* A Marco Polo game. */

/* Accepts a string with a name.
If the name is "Marco", returns "Polo".
If the name is "any other value", it returns "Marco".
*/
pub fn marco_polo(name: &str) -> String {
    if name == "Marco" {
        "Polo".to_string()
    } else {
        "Marco".to_string()
    }
}
```

`main.rs` code:

```
fn main() {
    let args = Cli::parse();
    match args.command {
        Some(Commands::Marco { name }) => {
            println!("{}", hello::marco_polo(&name));
        }
        None => println!("No command was used"),
    }
}
```

# Summary

The general premise of this chapter has been to critically examine the role of Python as the only option for MLOps. As Dr. Patterson illuminates, we need new languages, new hardware, and new ideas. The practice of MLOps is different from data science. It orients toward software engineering, especially software engineering of distributed computing systems.

As for MLOps with LLMs, deployment of Rust is a big deal. For example, once you use [whisper.py](https://github.com/openai/whisper) from [OpenAI](https://github.com/openai) and see how helpful it is at building accurate transcriptions of audio and video files, then the next logical question is for a user to try to make it go 25X faster. Suddenly, Rust makes a ton of sense; this *is* 25X more performant. The “secret sauce” is GitHub Copilot; it makes Rust syntax a breeze (we cannot overstate this). After all, you don’t want to be the person telling a client or boss, sorry, we cannot go faster, when you can!

Additionally, the idea that one language, Python, is a panacea for all software engineering problems, while tremendously popular and valuable, is magical thinking. Bolting more and more non-native components onto Python is a suboptimal strategy vs. choosing a new language when appropriate. Additionally, the old paradigm of suggesting people mix C with Python needs to be reavaluated if a developer can replace both with Rust and use one language.

We shouldn’t treat software languages like our favorite sports teams in a soccer match. The pragmatic practitioner looks for tools that efficiently solve problems. Languages like Go and Rust have emerged as solutions for high-performance computing, and Rust, in particular, shines at cybersecurity safety, a weakness of most languages like C and Python. A fair question is if learning a new language that is perhaps similarly or slightly more complex is worth the improvements in performance, energy efficiency, package deployment, and cybersecurity. In the case of Rust, there is a compelling case to be made that there is.

In distributed computing, performance matters, as does cybersecurity, energy usage, and binary software distribution. Rust has a lot of compelling use cases for MLOps, and additional examples are in the [Rust MLOp repo](https://github.com/noahgift/rust-mlops-template) as well as a tutorial on Rust in the appendix.

Rust has a lot to offer for MLOps. It combines a low-level language’s performance with a high-level language’s readability. Its emphasis on memory safety, type safety, and error checking can lead to more robust and secure MLOps pipelines. Furthermore, its interoperability with C and its growing ecosystem make it a strong contender for MLOps tasks that need to handle large volumes of data quickly and securely. As the MLOps field evolves, it will be interesting to see how Rust’s role develops.

Remember, the best tool for the job depends on the job at hand. Python has its strengths and will continue wide adoption in MLOps. But if you’re looking for a language that offers more performance, security, and robustness, Rust is worth a look.

# Exercises

*   Build a command-line tool in Rust that uses a PyTorch pre-trained model. You can use [this project](https://github.com/noahgift/rust-mlops-template/tree/main/pytorch-rust-example) as a reference.
    
*   Using the [linfa crate](https://crates.io/crates/linfa), build a K-means clustering command-line tool that analyzes a well-known public dataset. You can use [this example](https://github.com/noahgift/rust-mlops-template/tree/main/linfa-kmeans) as a starting point
    
*   Build a command-line summarization tool using the [rust-bert Hugging Face bindings](https://crates.io/crates/rust-bert). You can use this [project as a starting point](https://github.com/noahgift/rust-mlops-template/tree/main/hfdemo).
    
*   Deploy a PyTorch pre-trained model to AWS Lambda using Rust.
    
*   Run Stable Diffusion in Rust, [using this project](https://github.com/LaurentMazare/tch-rs) as the starting point.
    

# Critical Thinking Discussion Questions

*   What are some of the issues Rust solves as a systems programming language that is an advantage over Python?
    
*   How could inference performance impact choosing Rust as a core component in an MLOps pipeline?
    
*   What advantages are in play in using pre-trained models, such as LLMs, vs. training these models yourself exclusively? Additionally, how could Rust be an advantage in deploying solutions with LLM vs. Python?
    
*   Two of the most popular deep learning frameworks are TensorFlow and PyTorch. What are the pros and cons of these frameworks from a sustainability, i.e., energy efficiency standpoint? What about Rust vs. Python?
    
*   How could deploying a statically linked binary solution be advantageous over a scripting language solution?