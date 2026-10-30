
# KONRAD CLAPA | BRIAN GERRARD

# PROFESSIONAL
CLOUD ARCHITECT
GOOGLE CLOUD
CERTIFICATION GUIDE

# Second Edition

Build a solid foundation in Google Cloud Platform
to achieve the most lucrative IT certification

Foreword by Yujin Liang,
Fully Certified on GCP, Cloud Certification King on LinkedIn

![](img/de2c97ab017f36b3750a5bade111edd7_0_0.png)

# Professional Cloud Architect – Google Cloud Certification Guide

# *Second Edition*

Copyright © 2022 Packt Publishing

*All rights reserved.* No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the authors, nor Packt Publishing or its dealers and distributors, will be held liable for any damages caused or alleged to have been caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

**Group Product Manager:** Rahul Nair

**Publishing Product Manager:** Rahul Nair

**Senior Editor:** Sangeeta Purkayastha

**Content Development Editor:** Nihar Kapadia

**Technical Editor:** Nithik Cheruvakodan

**Copy Editor:** Safis Editing

**Project Coordinator:** Shagun Saini

**Proofreader:** Safis Editing

**Indexer:** Hemangini Bari

**Production Designer:** Aparna Bhagat

**Marketing Coordinator:** Nimisha Dua and Sanjana Gupta

First published: October 2019

Second edition: January 2022

Production reference: 1101221

Published by Packt Publishing Ltd.
Livery Place
35 Livery Street
Birmingham
B3 2PB, UK.

ISBN 978-1-80181-229-0

www.packt.com

To my wife and parents, for their constant support.

- Konrad Clapa

To Linda, for her constant support and understanding.

- Brian Gerrard

# Foreword

I came to know Konrad Clapa on LinkedIn. We both have an interest in Google Cloud and have both become fully certified on Google Cloud. Then we met at Google Cloud Next '19, where Konrad invited me to co-author a book to teach people how to use Google Cloud – this book.

I had had a failed writing experience before and I knew it takes a lot of effort to write a book, so I didn't agree to co-author it, but I did write the foreword for the first edition of the book, published in 2019.

Those 2 years passed quickly and so many new services have been released on Google Cloud since then. Google Cloud Training and Certification released an upgraded version of the exam for Google Cloud Certified Professional Cloud Architect in March 2021. Even though my certification won't expire until September 2021, I took the beta exam and passed.

As expected, a lot of new content has been added to the exam, and new services such as Google Kubernetes Engine and Anthos are tested in the new exam. These changes made the first edition of the book irrelevant.

I am glad Konrad and Brian have spent time refreshing their book to reflect the changes in the exam syllabus. This will be the first book ever published on the topic since the new exam was released and it will provide much-needed guidance for people to learn about the most up-to-date services from Google in order to pass the exam and advance their careers.

A better job, a better life. I achieved so much more after I became certified on a number of cloud vendors. I encourage people to learn the cloud and plan for a better career. This book gives you what you need to pass one of the most sought-after certifications, and also the highest-paid certification, according to some sources online.

We didn't get a chance to meet again after Google Cloud Next '19. Google Cloud Next '20 and '21 were online only due to COVID-19. But we kept in touch on LinkedIn and I am aware that Konrad has become a Google Cloud Certified Fellow, the most prestigious certification from Google, and one of my own career goals. I am happy about his achievement.

Even though I don’t know Brian Gerrard in person, since Konrad and Brian are colleagues and they co-authored the first edition of the book, we connected on LinkedIn. Brian has decade-long experience on VMware and he claimed to have many VMware-related certifications. Brian just moved to the public cloud in recent years. His transition from data center-centric technology to the public cloud is an inspiration for people facing the same dilemma – stop doubting the public cloud; rather, embrace it to further your career advancement. There are many similar success stories on LinkedIn for people to use as a reference and as inspiration for their own career moves. LinkedIn is a professional social media platform where you can connect with people with a common interest, learn from each other, and grow together to reach your own professional goals.

I feel honored to be given this opportunity again to write the foreword to this second edition. And I understand why Konrad wanted me to write it. For the past 5 years, I have inspired thousands of people to learn the cloud and find their purpose. I am the right person to introduce this book to you, which will help you learn Google Cloud and become certified. And great things will happen after becoming certified in Google Cloud.

Yujin Liang

Fully Certified on GCP

Cloud Certification King on LinkedIn

# Contributors

# About the authors

Konrad Cłapa is a Certified Google Trainer and Lead Cloud Architect working for Atos R&D. He has over 12 years' experience in the IT industry. He holds over 40 IT certification. This includes all 12/12 active Google Cloud Platform certifications making him the first in the world to achieve it. He is also listed among 40 individuals who hold Google Cloud Certified Fellow certificate. Sharing knowledge has always been important to him, so he contributes to the community by acting as a leader for a local Google Cloud Developer group. Funny fact about Konrad is that till now he never failed a GCP exam attempt including multiple beta exams that he has taken.

> First of all, I would like to thank my lovely wife Kinga for the patience and understanding of my passion. It is difficult to be a wife of a geek. I would also like to thank my parents that always supported me in any decisions I have taken in my life. Last but not the least I thank Brian for being a fellow for multiple journeys and above all being a great friend!

Brian Gerrard is currently a DevOps Engineer from Scotland with over 15 years' experience in the IT industry. As well as holding the Google Certified Professional Architect and Google Certified Associate Engineer certifications, he is certified in Azure, AWS, Terraform and VMware. Brian is a firm believer in lifelong learning, and you will regularly find him contributing to his local user groups.

> I would like to thank my wife and children for their support. Also, thanks to Konrad for constantly pushing the Google community to achieve more!

# About the reviewers

**Dr. Artem Nikulchenko** is a chief software architect at Teamwork Commerce, a Google Cloud Platform Partner that specializes in the development of retail management systems. Artem spearheaded the usage of Google Cloud Platform in teamwork products from the very first publicly available version of the platform.

Artem has a Ph.D. in computer science and holds an associate professor position at National Technical University Kharkiv Polytechnic Institute. He teaches courses on databases, software architecture, and public clouds.

As a big fan of Google Cloud, Artem leads his local Google Developer Group and delivers talks about Google Cloud Platform at specialized conferences around the world.

> *Having worked with GCP for over 10 years, I still found this book very useful while preparing for my Google Cloud Architect certification exam. Then, I was invited to become a reviewer of that same book’s newest edition. It’s a great honor for me and I’d like to thank the authors, as well as Packt Publishing, for this exceptional opportunity.*

**Artur Zeifer** has been working in the IT industry for more than 20 years, ranging across application support, database administration, automation, and architect roles. This has given him a broad base of skills and the ability to work with a diverse range of clients. He holds a master's degree in computer science and several industry-recognized certifications from Oracle, Google, The Open Group, and so on. He currently works for Atos R&D as a database architect focusing on automating daily operations. Artur has a passion for learning and information that he continues to pursue daily.

Above all the previously stated, he is a father, husband, son, brother, and friend.

> *I’d like to thank my wife, Kamila, and our son, Franek, for their daily support and patience. To my parents, siblings, relatives, friends, and mentors (you know who you are), thank you for guiding and supporting me.*
