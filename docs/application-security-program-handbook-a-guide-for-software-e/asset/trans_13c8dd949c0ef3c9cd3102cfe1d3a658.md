

A guide for software engineers and team leaders

# APPLICATION SECURITY PROGRAM

# Handbook

Derek Fisher

![](img/cc84747f93d290964598894b4f5c153b_0_0.png)

![](img/cc84747f93d290964598894b4f5c153b_0_1.png)

MANNING

![](img/cc84747f93d290964598894b4f5c153b_1_0.png)

# MEAP Edition
Manning Early Access Program
Application Security Program Handbook
A guide for software engineers and team leaders

Version 2

Copyright 2021 Manning Publications

For more information on this and other Manning titles go to [manning.com](https://manning.com)

# welcome

Thank you for your interest in this book *Application Security Program Handbook*. This book is intended for developers, architects, and leaders that are looking to understand the purpose and benefit of building security into the development lifecycle. Although you don’t need to be a developer, having an understanding of how code is developed and delivered to production will make the content easier to understand.

I have been working in application security for nearly a decade and have seen the successes and failures in organizations that are fighting the good fight of delivering secure software. Although the fundamentals of application security haven’t changed much in that time, the tools, processes, and technology have. The threats have changed, the attacker’s techniques are getting more robust, and the risks to organizations have grown. Organizations need to change with them and develop the framework that enables them to build security in from the start.

My desire for this book is that it will show how application security is applied in organizations and where it can provide the most amount of assistance in delivering secure software. The book is made up of three sections that take the reader through an effective application security program. The first section is dedicated to what are common application security practices and technology that you find in an organization. The second section dives more into how application security is evolving to meet the current demands of rapid development. This includes how application security should be developed as a service and not as the gated exercises. The last section will focus on delivering application security in an organization and measuring success.

I appreciate you picking up this book and my hope is that it brings you an appreciation for what it takes to deliver secure software in today’s complex systems. Please be sure to post any questions, comments, or suggestions you have about the book in the liveBook discussion forum. Your feedback is essential in developing the best book possible.

-Derek Fisher

# brief content

## PART 1 DEFINING APPLICATION SECURITY

- 1 Why do we need application security?
- 2 Defining the problem
- 3 Components of application security

## PART 2 DEVELOPING THE APPLICATION SECURITY PROGRAM

- 4 Releasing secure code
- 5 Security belongs to everyone
- 6 Service oriented application security

## PART 3 DELIVER AND MEASURE

- 7 Building a roadmap
- 8 Measuring success
- 9 Continuous improvement

# 1

# Why do we need application security?

This chapter covers:

- Current state of application security
- Going right or going left
- Breaches as a result of insecure applications
- The cost of inaction

Every company uses software to function. Whether they are a Fortune 500 technology company or a sole proprietor landscaping company, software is integral to businesses large and small. Software provides a means to track employees, customers, inventory, and scheduling. Data moves from a myriad of systems, networks, and software providing insights to businesses looking to stay competitive. Some of that software used is built within the organization or it is purchased and integrated. What this means is that every organization, regardless of size and industry, has a software need. It enables organizations to move quickly and stay ahead of their competition. In the United States, the growth of software in various industries including finance, sales, human resource, and supply chain has seen a steady increase with that trend continuing. Over the next decade these industries will see a steady increase of market size. For instance, in 2020 the market size of nearly 390 billion USD in the global business software and services. This is expected to have an annual growth of 11% from 2021-2028. This expansion is based on the increasing need for automation and processing solutions in nearly every sector on the economy.

The shift to software driven organizations can be seen in several prominent examples. Movies were rented at a physical store before Netflix began streaming them in 2007 directly to your television. Books were purchased in a shop before Amazon upended not just the book buying experience but the overall retail market for any item you can think of. It used to take a phone call to order food, now a mobile app allows you to not only order your food but track it from the restaurant to your front door. Moreover, software is used in all parts of the supply chain for every organization. Leaders in these organizations can get real-time updates on product as it moves around the world and to their customers hands. This isn’t a nice to have any more for organizations but is critical to any organization’s ability to compete.

> "In short, software is eating the world" Marc Andreessen.

Each new piece of software brings new capabilities but also new challenges, especially when that software is available over a network where it is open to a wide audience. More capabilities mean more software. More software means more data. This data is intended to assist an organization and its customers to gain insight into the services the organization offers. However, the additional software and data means more opportunity for bad actors to leverage weaknesses that might exist. Data is a lucrative target for attackers and difficult for organizations to protect especially when the organization collects a large amount of it. By the beginning of 2020, the amount of data created worldwide had reached 44 zettabytes and this is expected to reach 463 exabytes by 2025.

The increasing software landscape and the repository of data growing every second offers a target rich environment for malicious actors looking for a way in and access to the data. There is constant background noise of malicious activity that pervades the internet. The moment that any software becomes available online, it is immediately probed and prodded. Some of that activity is from automated tools that will then alert when it finds software that can be compromised. Some of it is from bad actors waiting for the latest vulnerability to become public so they can hunt the internet for applications that are vulnerable to it and create exploitation software. Other activity comes from well financed bad actors that are simply looking for any weaknesses so that they can gain a foothold in an organization in order to come back later for exploitation. If ever there was a more fitting metaphor for software on the internet, it would be the Wild West of the early United States where lawlessness was rampant, and sheriffs were often overwhelmed and outgunned by the range of activity.

Software, data, attackers, oh my! When organizations don’t take software security seriously, they run the risk of not only putting their client’s data at risk, but the organizations future as well. Most organizations that are not in the software building industry will often say that they are not a prime target, or that their data is not interesting to an attacker. They would be wrong. Almost all data that is collected and processed by an organization can be used and misused by an attacker. Nothing is too unimportant. And although many large organizations can weather a data breach, smaller companies often cannot while remaining in business.

It is often said that data is the new oil. Mining, processing, and selling data is a lucrative business both for the organizations that do so openly and the malicious actors who are looking for ways to profit off an organization’s missteps. Building security into the software from the start is the first, and primary, step to ensuring that organizations have the means to protect their software, their data, and their livelihood.

