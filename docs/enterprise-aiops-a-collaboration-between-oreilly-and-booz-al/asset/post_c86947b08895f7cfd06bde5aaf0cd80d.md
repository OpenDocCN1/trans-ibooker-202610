
# O'REILLY®

# Enterprise
AIOps

A Framework for Enabling Artificial Intelligence

Justin Neroda,
Steve Escaravage
& Aaron Peters

REPORT

![](img/53a28dd44bbae66341b0fe693bd59916_1_0.png)

Booz | Allen | Hamilton®

# ENGINEERING
YOUR AI FUTURE

Enterprise artificial intelligence (AI) requires a proven playbook. As the leader in providing AI services for the federal government, we enable any organization to design, engineer, and operationalize purpose-built AI solutions. From core analytics to the full potential of AI, we solve your hardest data problems. Don’t go it alone. Partner with Booz Allen.

See our ideas in action at boozallen.com/ai.

# Enterprise AIOps

A Framework for Enabling Artificial Intelligence

Justin Neroda, Steve Escaravage, and Aaron Peters

# Enterprise AIOps

by Justin Neroda, Steve Escaravage, and Aaron Peters

Copyright © 2021 O’Reilly Media Inc. All rights reserved.

Printed in the United States of America.

Published by O’Reilly Media, Inc., 1005 Gravenstein Highway North, Sebastopol, CA 95472.

O’Reilly books may be purchased for educational, business, or sales promotional use. Online editions are also available for most titles (http://oreilly.com). For more information, contact our corporate/institutional sales department: 800-998-9938 or corporate@oreilly.com.

Acquisitions Editor: Rebecca Novack

Development Editor: Virginia Wilson

Production Editor: Christopher Faucher

Copyeditor: nSight, Inc.

Proofreader: Piper Editorial Consulting, LLC

Interior Designer: David Futato

Cover Designer: Randy Comer

Illustrator: Kate Dullea

August 2021: First Edition

# Revision History for the First Edition

+   - 2021-08-16: First Release

The O’Reilly logo is a registered trademark of O’Reilly Media, Inc.
*Enterprise AIOps*, the cover image, and related trade dress are trademarks of O’Reilly Media, Inc.

The views expressed in this work are those of the authors, and do not represent the publisher’s views. While the publisher and the authors have used good faith efforts to ensure that the information and instructions contained in this work are accurate, the publisher and the authors disclaim all responsibility for errors or omissions, including without limitation responsibility for damages resulting from the use of or reliance on this work. Use of the information and instructions contained in this work is at your own risk. If any code samples or other technology this work contains or describes is subject to open source licenses or the intellectual property rights of others, it is your responsibility to ensure that your use thereof complies with such licenses and/or rights.

This work is part of a collaboration between O’Reilly and Booz Allen Hamilton. See our [statement of editorial independence](statement of editorial independence).

978-1-098-10726-0

[LSI]

# Preface

A significant transformation is underway in the marketplace.

Mountains of data are being generated on a daily basis. Business operations, online transactions, vehicles and smart homes, our cell phone, and the increasing prevalence of the Internet of Things (IoT) means that data is continuously being generated and stored.

This growth in data vastly outstrips growth in the supply of technical analysts as demand for their services proliferates across industries. Companies, governments, and organizations are rightly asking how they can possibly provide value from these data volumes with their existing analytical capabilities and staff while determining where they need to invest to meet this growth.

When deployed effectively, Artificial Intelligence (AI) provides the necessary force multiplier, allowing organizations to generate insights not possible even a few years ago.¹ AI provides organizations with the ability to process vast amounts of data by training algorithms to automate current processes, generate new insights, and then present these insights so that decision makers can act.

> *Artificial Intelligence (AI)*
> 
> The ability for machines to solve problems and perform tasks that would normally require human capabilities and intelligence.

Rapid gains in computing power, inexpensive storage solutions for large volumes of data, and new research in algorithm development make AI increasingly more accessible. These three keys—processing power, data,
and algorithms—bring us to a new reality, one where AI can now meet or exceed human capability across a variety of diverse tasks.

Similar to past industrial revolutions, AI’s adoption will have sweeping effects on our daily lives and work. Traditional business analytics (think Excel or basic statistical analyses) provided a historically competitive advantage to organizations in many industries. Now, companies are seeking the next wave of competitive advantage and strive for better performance in their analyses—a result that effective AI deployments can deliver. As such, artificial intelligence will change how we conduct business, introducing new efficiencies—and complexities—that we are just beginning to understand. Just as importantly, AI will fundamentally alter how you manage your teams, services, and operations.

The ability of strategic leaders to anticipate and guide these long-term changes will be essential to ensuring the success of this transition and allowing their organization to remain competitive. While there is growing consensus around AI’s importance, there is a growing chasm regarding how to successfully deploy and apply artificial intelligence at an enterprise-wide scale. Many organizations are struggling with expanding, evolving, and integrating their early AI development efforts into mature, sustainable, enterprise-wide capabilities. This gap is due to the drastic increase in scope and complexity required to operationalize artificial intelligence, particularly in terms of integrating AI solutions within the larger organization.

This report is for business leaders who desire to transition AI from small, pilot projects to an enterprise-wide reality. We will introduce an artificial intelligence operations (AIOps) engineering framework to assist you in overcoming these post-pilot challenges through responsibly developing AI tools, the important role of data management, team roles and responsibilities, and large-scale implementation. The insights and strategies we share come from the lessons we’ve learned working at Booz Allen Hamilton on a portfolio of over 120 AI projects across the federal government.

By the end of the report, you’ll learn how to unlock the incredible potential that lies within your organization’s exponentially growing data, deriving
insights not available just a few years ago.

To extract the most benefit from this report, you should understand AI’s fundamentals, its purpose, and what challenges can be solved when deploying AI-based solutions. If you’d like a quick refresher or a deeper introduction to AI, our AI primer is a great place to start.²

# Acknowledgments

A huge thank-you to all the individuals who collaborated with us on this report. To the following colleagues: John Larson, Kathleen Featheringham, Elizabeth Cardosa, Alex Walter-Higgins, Caleb Wharton, Sheshadri Mudiyanur, Byron Gaskin, Jeffrey Gross, Chuck Audet, Drew Farris, Drew Leety, Geoff Schaefer, David McCleary, Susan Johnston, Katrina Jacobs, Catherine Quinn, and Catherine Ordun—your insight and contributions strengthened this piece considerably.

1 While there are many, often competing, technical definitions for AI, we wanted to provide a broad, high-level definition for this report. Our definition of AI is extracted from the National Security Commission on Artificial Intelligence. You can view their 2021 report at their website, where they define AI on page 20.

2 Booz Allen Hamilton, *The Artificial Intelligence Primer*, accessed July 13, 2021.

# Chapter 1. Demystifying AI

A majority of analytics exist to take operational data (e.g., past/present stock prices) and provide focused insights (e.g., predicted stock prices) that inform decision making. This essential objective is the same for conventional business analytics and AI analytics and includes a range of functions (e.g., automation, augmentation, conversational AI for consumers, etc.).

The key difference is how developers create the code that transforms operational data into insights. For conventional business analytics, this is a static process where the developer manually defines each logical operation the computer must take.

AI analytics, via machine learning (ML), attempts to derive the necessary operations directly from the data, reducing the onus on the developer to create and update the model over time (but not eliminating the developer) and making it possible to address otherwise prohibitively sophisticated use cases (e.g., computer vision).¹

# Machine Learning (ML)

A subset of AI, machine learning is the study of computer algorithms that improve automatically through experience and by use of data. ML algorithms build a model based on sample data, known as “training data” (defined in Chapter 5), to make predictions or decisions without being explicitly programmed to do so.

Beyond the initial challenge of the ML algorithm teaching an AI analytic to complete a basic task, we must ensure that it does not learn additional, undesirable behaviors that may impact its long-term sustainability (reliability, security, etc.). The ability to holistically understand the learned
