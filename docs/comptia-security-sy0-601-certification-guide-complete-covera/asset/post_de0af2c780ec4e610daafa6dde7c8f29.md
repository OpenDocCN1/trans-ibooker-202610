
# CompTIA Security+: SY0-601
# Certification Guide
# Second Edition

Complete coverage of the new CompTIA Security+ (SY0-601) exam to help you pass on the first attempt

Revised & Updated for the Latest Exams
NEW UPDATE

# CompTIA Security+:
# SY0-601 Certification
# Guide

## *Second Edition*

Complete coverage of the new CompTIA Security+ (SY0-601) exam to help you pass on the first attempt

Ian Neil

![](img/adfe46a7d545f23d50e4108d31e60de2_1_0.png)

BIRMINGHAM—MUMBAI

CompTIA Security+: SY0-601 Certification Guide
Second Edition

Copyright © 2020 Packt Publishing

All rights reserved. No part of this book may be reproduced, stored in a retrieval system, or transmitted in any form or by any means, without the prior written permission of the publisher, except in the case of brief quotations embedded in critical articles or reviews.

Every effort has been made in the preparation of this book to ensure the accuracy of the information presented. However, the information contained in this book is sold without warranty, either express or implied. Neither the author, nor Packt Publishing or its dealers and distributors, will be held liable for any damages caused or alleged to have been caused directly or indirectly by this book.

Packt Publishing has endeavored to provide trademark information about all of the companies and products mentioned in this book by the appropriate use of capitals. However, Packt Publishing cannot guarantee the accuracy of this information.

Commissioning Editor: Vijin Boricha
Acquisition Editor: Rahul Nair
Senior Editor: Arun Nadar
Content Development Editor: Pratik Andrade
Technical Editor: Yoginee Marathe
Copy Editor: Safis Editing
Project Coordinator: Neil Dmello
Proofreader: Safis Editing
Indexer: Safis Editing
Production Designer: Shantanu Zagade

First published: September 2018
Second Edition published December 2020, updated November 2021

Production reference: 2031121

Published by Packt Publishing Ltd.
Livery Place
35 Livery Street
Birmingham
B3 2PB, UK.

ISBN 978-1-80056-424-4

www.packt.com

![](img/adfe46a7d545f23d50e4108d31e60de2_3_0.png)

Packt.com

Subscribe to our online digital library for full access to over 7,000 books and videos, as well as industry leading tools to help you plan your personal development and advance your career. For more information, please visit our website.

Why subscribe?

- Spend less time learning and more time coding with practical eBooks and Videos from over 4,000 industry professionals
- Improve your learning with Skill Plans built especially for you
- Get a free eBook or video every month
- Fully searchable for easy access to vital information
- Copy and paste, print, and bookmark content

Did you know that Packt offers eBook versions of every book published, with PDF and ePub files available? You can upgrade to the eBook version at packt.com and as a print book customer, you are entitled to a discount on the eBook copy. Get in touch with us at `customercare@packtpub.com` for more details.

At www.packt.com, you can also read a collection of free technical articles, sign up for a range of free newsletters, and receive exclusive discounts and offers on Packt books and eBooks.

Contributors

About the author

**Ian Neil** is one of the world's top trainers of Security+. He is able to break down information into manageable chunks so that people with no background knowledge can gain the skills required to become certified. He has recently worked for the US Army in Europe and designed a Security+ course that catered to people from all backgrounds (not just IT professionals), with an extremely successful pass rate. He is an MCT, MCSE, A+, Network+, Security+, CASP, and RESILIA practitioner, who, over the past 23 years, has worked with high-end training providers and was one of the first technical trainers to train Microsoft internal staff when they opened their Bucharest Office in 2006.

About the reviewers

**Crystal Voiles** is an IT specialist with more than 30 years of IT experience ranging from help desk support, desktop support, system administration, and cyber security support.

For the last 10 years, she has served as a cyber security specialist, managing several cyber security tools, including **Assured Compliance Assessment Solution (ACAS)**, **Host-Based Security System (HBSS)**, Tanium, **System Center Configuration Manager (SCCM)**, and **Enterprise Mission Assurance Support Service (eMASS)**.

Currently serving as the **Information Systems Security Manager (ISSM)** for a small medical organization responsible for coordination and execution of security policies and controls, as well as assessing vulnerabilities within a medical company. She is responsible for data and network security processing, security systems management, and security violation investigations. She manages backup and security systems, employee training for approximately 900 end user accounts, security planning measures, and recovery of data in disaster testing situations.

Her certifications include **Certified Information Systems Security Professional (CISSP)**, **CompTIA Advanced Security Practitioner (CASP+)**, Security +, **Microsoft Certified Professional (MCP)**, SCCM, and ITIL Foundations.

**Rebecca Moffitt** is an experienced information security and risk consultant with 8 years of experience in the industry.

Rebecca joined QA in October of 2018, and since then has been working as a cyber security technical specialist. Her areas of training have been primarily related to cyber security, information security, information assurance, and risk management. She most recently obtained her CISM via ISACA, and her CSRM via PECB. She is a certified Information Security Management Systems Lead Implementer and is proficient in ISO 27001, 27002, 27005, and has knowledge of ISO 31000, 27035, and 19011, as well as various cyber, information, and risk frameworks.

Rebecca is passionate about her profession and has spent time working with the younger generations, raising their awareness of the field of cyber/information security and sparking enthusiasm in them about a potential career in cyber security.

Table of Contents

Preface

Objectives for the CompTIA Security+ 601 exam

Section 1:
Security Aims and Objectives

1
Understanding Security Fundamentals

Security Fundamentals 4
CIA Triad Concept 4
Least Privilege 5
Defense in Depth Model 5

Role-Based Access Control 12
Rule-Based Access Control 13
Attribute-Based Access Control 13
Group-Based Access Control 13
Linux-Based Access Control 13

Comparing Control Types 6
Managerial Controls 7
Operational Controls 7
Technical Controls 8
Deterrent Controls 8
Detective Controls 9
Corrective Controls 9
Compensating Controls 9
Preventative Controls 10
Access Controls 10
Discretionary Access Control 11
Mandatory Access Control 12

Physical Security Controls 15

Perimeter Security 15
Building Security 17
Device Protection 18

Understanding Digital Forensics 19

Five-Minute Practical 21
Collection of Evidence 21
Cloud Forensics 26

Review Questions 27

ii Table of Contents

2
Implementing Public Key Infrastructure

+ **PKI Concepts** ........................................ 30
+ Certificate Hierarchy .................................. 30
+ Certificate Trust ...................................... 33
+ Certificate Validity ................................... 34
+ Certificate Management Concepts ........................ 35
+ Types of Certificates .................................. 36

+ **Asymmetric and Symmetric Encryption** ................. 39
+ Encryption Explained ................................... 39
+ Digital Signatures Explained ........................... 42
+ Cryptography Algorithms and Their Characteristics ....... 44
+ Symmetric Algorithms ................................... 44
+ Asymmetric Algorithms .................................. 45
+ Symmetric versus Asymmetric Analogy ..................... 46
+ Lightweight Cryptography ................................ 46
+ XOR Encryption ......................................... 47

+ **Key Stretching Algorithms** .......................... 47
+ **Salting Passwords** .................................. 48
+ **Cipher Modes** ....................................... 48
+ Stream versus Block Cipher Analogy ...................... 48
+ Modes of Operation ..................................... 48

+ **Quantum Computing** .................................. 50
+ **Blockchain and the Public Ledger** .................... 50
+ **Hashing and Data Integrity** .......................... 51
+ **Comparing and Contrasting the Basic Concepts of Cryptography** ... 51
+ Asymmetric – PKI ....................................... 51
+ Symmetric Algorithm – Modes of Operation ............... 52
+ Hashing Algorithms ..................................... 53

+ Crypto Service Provider ................................ 53
+ Crypto Module .......................................... 53
+ Data Protection ........................................ 53

+ **Basic Cryptographic Terminologies** ................... 55
+ Obfuscation ............................................ 55
+ Pseudo-Random Number Generator ........................ 55
+ Nonce .................................................. 55
+ Perfect Forward Secrecy ................................ 55
+ Security through Obscurity ............................. 55
+ Collision .............................................. 56
+ Steganography .......................................... 56
+ Homomorphic Encryption ................................ 56
+ Diffusion .............................................. 56
+ Implementation Decisions ................................ 56

+ **Common Use Cases for Cryptography** ................... 56
+ Supporting Confidentiality ............................. 57
+ Supporting Integrity ................................... 57
+ Supporting Non-Repudiation ............................ 57
+ Supporting Obfuscation ................................ 57
+ Low-Power Devices ...................................... 58
+ High Resiliency ........................................ 58
+ Supporting Authentication .............................. 58
+ Resource versus Security Constraints .................... 58

+ **Practical Exercises** ................................ 58
+ Practical Exercise 1 – Building a Certificate Server ... 59
+ Practical Exercise 2 – Encrypting Data with EFS and Self-Signed Certificates ... 60
+ Practical Exercise 3 – Revoking the EFS Certificate .... 61

+ **Review Questions** ................................... 61
