
![](img/ec1933a597ce63afa3bcf7496364f412_0_0.png)

1ST EDITION

## Data Wrangling on AWS

Clean and organize complex data for analysis

NAVNIT SHUKLA
SANKAR M | SAM PALANI

![](img/ec1933a597ce63afa3bcf7496364f412_1_0.png)

## Data Wrangling on AWS

Copyright © 2023 Packt Publishing

*All rights reserved.* No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the authors, nor Packt Publishing or its dealers and distributors, will be held liable for any damages caused or alleged to have been caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

Publishing Product Manager: Heramb Bhavsar

Book Project Manager: Kirti Pisat

Content Development Editor: Joseph Sunil

Technical Editor: Sweety Pagaria

Copy Editor: Safis Editing

Proofreader: Safis Editing

Indexer: Sejal Dsilva

Production Designer: Shyam Sundar Korumilli

DevRel Marketing Coordinator: Nivedita Singh

First published: July 2023

Production reference: 1280723

Published by Packt Publishing Ltd.

Grosvenor House

11 St Paul’s Square

Birmingham

B3 1RB, UK.

ISBN 978-1-80181-090-6

www.packtpub.com

I am grateful to my grandmother, Radhika Shukla, my mother, Asha Shukla, my grandfather, I.D. Shukla, and my father, N.K. Shukla, for their unwavering support and the sacrifices they have made. They have been a constant source of inspiration and have shown me the true power of determination. I would also like to express my heartfelt gratitude to my loving wife, Anchal Dubey, and my son, Anav Shukla, for being my steadfast companions on our shared life journey. Their love and encouragement have fueled my ambitions and brought immense joy to my life.

- Navnit Shukla

I am thankful to my wife, Krithiha Kumar, for her unwavering support and motivation in all my endeavors. I would thank my son and daughter for their love, curiosity, creativity, and boundless energy, which has inspired me every day to explore further and dream big.

## Contributors

## About the authors

**Navnit Shukla** is a passionate individual with a love for technology. He pursued engineering studies in electronics and communication at Dr. A.P.J. Abdul Kalam Technical University in Lucknow (formerly U.P.T.U), setting the stage for his future endeavors. At the age of 25, he ventured across the oceans to the United States, where he now resides in the beautiful Orange County, California, alongside his loving wife and son. Immersed in the world of IT since 2009, he has gained extensive experience and knowledge within the industry. For the past four years, he has worked with **Amazon Web Services (AWS)** as a specialist solution architect – analytics. In this role, he is entrusted with the responsibility of assisting organizations of all sizes in architecting, building, and seamlessly operating solutions on the dynamic AWS Cloud platform. Beyond his professional pursuits, he finds immense joy in spending quality time with his family and friends, exploring new places, creating precious memories, and embarking on exciting adventures.

> *I am deeply grateful to those who have been by my side, offering unwavering support throughout my journey. I extend a heartfelt thank you to my beloved wife, Anchal Dubey, and my devoted parents, whose encouragement and belief in me have been invaluable.*

**Sankar M** has been working in the IT industry since 2007, specializing in databases, data warehouses, and the analytics space for many years. As a specialized data architect, he helps customers build and modernize data architectures and helps them build secure, scalable, and performant data lake, database, and data warehouse solutions. Prior to joining AWS, he worked with multiple customers in implementing complex data architectures.

**Sam Palani** has over 18+ years as a developer, data engineer, data scientist, startup co-founder, and IT leader. He holds a master's in Business Administration with a dual specialization in Information Technology. His professional career spans 5 countries across financial services, management consulting, and the technology industries. He is currently Sr Leader for Machine Learning and AI at Amazon Web Services, where he is responsible for multiple lines of the business, product strategy, and thought leadership. Sam is also a practicing data scientist, a writer with multiple publications, a speaker at key industry conferences, and an active open-source contributor. Outside work, he loves hiking, photography, experimenting with food, and reading.

## About the reviewer

**Naresh Rohra** is an accomplished data modeler and lead analyst with a passion for harnessing the potential of cutting-edge technologies to unlock valuable insights from complex datasets. With over a decade of experience in the field of data modeling, he has successfully navigated the dynamic landscape of data wrangling and analysis, earning him a reputation as a leading expert. He has expertly designed various data models related to OLTP and OLAP systems. He has worked with renowned organizations like TCS, Cognizant, and Tech Mahindra.

*I extend heartfelt thanks to my colleagues and family for their constant encouragement throughout my book review journey. To my beloved daughter, Ginisha, you are the luminescent star that adorns my night sky.*

**Roja Boina** is an engineering senior advisor. She provides data-driven solutions to enterprises. She takes pride in her get-it-done attitude. She enjoys defining requirements, designing, developing, testing, and delivering backend applications, and communicating data through visualizations. She is very passionate about being a **Women in Tech (WIT)** advocate and being a part of WIT communities. Outside of her 9-5 job, she loves to volunteer for non-profits and mentor fellow women in STEM. She believes in having a growth mindset.

## Table of Contents

- Preface

## Part 1: Unleashing Data Wrangling with AWS

## 1

## Getting Started with Data Wrangling

## Introducing data wrangling

## The 80-20 rule of data analysis

## Advantages of data wrangling

## The steps involved in data wrangling

## Data discovery

## Data structuring

## Data cleaning

## Data enrichment

## Data validation

## Data publishing

## Best practices for data wrangling

## Identifying the business use case

## Identifying the data source and bringing the right data

## Identifying your audience

## Options available for data wrangling on AWS

## AWS Glue DataBrew

## SageMaker Data Wrangler

## AWS SDK for pandas

## Summary

## Part 2: Data Wrangling with AWS Tools

## 2

## Introduction to AWS Glue DataBrew

## Why AWS Glue DataBrew?

## AWS Glue DataBrew’s basic building blocks

## Getting started with AWS Glue DataBrew

## Understanding the pricing of AWS Glue DataBrew

## Using AWS Glue DataBrew for data wrangling

## Identifying the dataset

## Downloading the sample dataset

## Data discovery – creating an AWS Glue DataBrew profile for a dataset

## Data cleaning and enrichment – AWS Glue DataBrew transforms

## Data validation – performing data quality checks using AWS Glue DataBrew

## Data publication – fixing data quality issues

## Event-driven data quality check using Glue DataBrew

## Data protection with AWS Glue DataBrew

## Encryption at rest

## Encryption in transit

## Identifying and handling PII

## Data lineage and data publication

## Summary

## 3

## Introducing AWS SDK for pandas
