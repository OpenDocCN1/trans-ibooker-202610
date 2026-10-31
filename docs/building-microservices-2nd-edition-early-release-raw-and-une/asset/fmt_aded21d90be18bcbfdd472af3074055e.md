
## 1. [Expansion Changes](#ch04.xhtml#idm46534870551128)
## 2. [Tolerant Reader](#ch04.xhtml#idm46534870548744)
## 3. [Right Technology](#ch04.xhtml#idm46534870514152)
## 4. [Explicit Interface](#ch04.xhtml#idm46534870511160)
## 5. [Catch Accidental Breaking Changes Early](#ch04.xhtml#idm46534870462296)
## 14. [Managing Breaking Changes](#ch04.xhtml#idm46534870431016)
### 1. [Lock-Step Deployment](#ch04.xhtml#idm46534870424248)
### 2. [Coexist Incompatible Microservice Versions](#ch04.xhtml#idm46534870422296)
### 3. [Emulate The Old Interface](#ch04.xhtml#idm46534870414072)
### 4. [Which Approach Do I Prefer?](#ch04.xhtml#idm46534870401512)
### 5. [The Social Contract](#ch04.xhtml#idm46534870397768)
### 6. [Tracking Usage](#ch04.xhtml#idm46534870387784)
### 7. [Extreme Measures](#ch04.xhtml#idm46534870385528)
## 15. [DRY and the Perils of Code Reuse in a Microservice World](#ch04.xhtml#a50-dry)
### 1. [Sharing Code Via Libraries](#ch04.xhtml#idm46534870377272)
## 16. [Summary](#ch04.xhtml#idm46534870360680)
## 5. [5\. Workflow](#ch05.xhtml#workflow-chapter)
### 1. [Transactions](#ch05.xhtml#ch05-transactions)
#### 1. [ACID Transactions](#ch05.xhtml#idm46534870339592)
#### 2. [Still ACID, but Lacking Atomicity?](#ch05.xhtml#idm46534870319384)
### 2. [Two-Phase Commits](#ch05.xhtml#idm46534870302616)
### 3. [Distributed Transactions—Just Say No](#ch05.xhtml#idm46534870302024)
### 4. [Sagas](#ch05.xhtml#ch05-sagas)
#### 1. [Saga Failure Modes](#ch05.xhtml#idm46534870246200)
#### 2. [Implementing Sagas](#ch05.xhtml#idm46534870206600)
#### 3. [Sagas Versus Distributed Transactions](#ch05.xhtml#idm46534870137608)
#### 4. [Summary](#ch05.xhtml#idm46534870121768)
## 6. [6\. Build](#ch06.xhtml#build-chapter)
### 1. [A Brief Introduction to Continuous Integration](#ch06.xhtml#idm46534870111272)
#### 1. [Are You Really Doing CI?](#ch06.xhtml#idm46534870104632)
#### 2. [Branching Models](#ch06.xhtml#idm46534870096296)
### 2. [Build Pipelines and Continuous Delivery](#ch06.xhtml#idm46534870095704)
#### 1. [Tooling](#ch06.xhtml#idm46534870055160)
#### 2. [Tradeoffs and Environments](#ch06.xhtml#environment-tradeoff)
#### 3. [Artifact Creation](#ch06.xhtml#idm46534870044376)
### 3. [Mapping Source Code and Builds to Microservices](#ch06.xhtml#idm46534870074952)
#### 1. [One Giant Repo, One Giant Build](#ch06.xhtml#idm46534870030376)
#### 2. [Pattern: One Repository Per Microservice (aka Multi-Repo)](#ch06.xhtml#idm46534870019560)
#### 3. [Pattern: Monorepo](#ch06.xhtml#idm46534869983784)
#### 4. [Which Approach Would I Use?](#ch06.xhtml#idm46534869983192)
### 4. [Summary](#ch06.xhtml#idm46534869919784)
## 7. [7\. Deployment](#ch07.xhtml#deployment_chapter)
### 1. [From Logical to Physical](#ch07.xhtml#idm46534869909368)
#### 1. [Multiple Instances](#ch07.xhtml#idm46534869902888)
#### 2. [The Database](#ch07.xhtml#idm46534869891464)
#### 3. [Environments](#ch07.xhtml#idm46534869883224)
### 2. [Principles Of Microservice Deployment](#ch07.xhtml#idm46534869864472)
#### 1. [Isolated Execution](#ch07.xhtml#idm46534869842504)
#### 2. [Focus On Automation](#ch07.xhtml#idm46534869834264)
#### 3. [Infrastructure As Code](#ch07.xhtml#idm46534869806152)
#### 4. [Zero-downtime Deployment](#ch07.xhtml#idm46534869799144)
#### 5. [Desired State Management](#ch07.xhtml#idm46534869792424)
### 3. [Deployment Options](#ch07.xhtml#idm46534869791832)
#### 1. [Physical Machines](#ch07.xhtml#idm46534869765288)
#### 2. [Virtual Machines](#ch07.xhtml#idm46534869751272)
#### 3. [Containers](#ch07.xhtml#idm46534869732248)
#### 4. [Application Containers](#ch07.xhtml#idm46534869692664)
#### 5. [Platform As A Service (PAAS)](#ch07.xhtml#idm46534869681704)
#### 6. [Function As A Service (FAAS)](#ch07.xhtml#idm46534869674616)
### 4. [Which Deployment Option Is Right For You?](#ch07.xhtml#idm46534869768632)
### 5. [Kubernetes & Container Orchestration](#ch07.xhtml#idm46534869618808)
#### 1. [The Case For Container Orchestration](#ch07.xhtml#idm46534869594984)
#### 2. [A Simplified View Of Kubernetes Concepts](#ch07.xhtml#idm46534869589592)
#### 3. [Multi-Tenancy and Federation](#ch07.xhtml#idm46534869573944)
#### 4. [The Cloud Native Computing Federation](#ch07.xhtml#idm46534869555096)
#### 5. [Platforms and Portability](#ch07.xhtml#idm46534869550376)
#### 6. [Helm, Operations and CRDs, oh my!](#ch07.xhtml#idm46534869546072)
#### 7. [And Knative](#ch07.xhtml#idm46534869540792)
#### 8. [The Future](#ch07.xhtml#idm46534869533000)
#### 9. [Should You Use It?](#ch07.xhtml#idm46534869530664)
### 6. [Progressive Delivery](#ch07.xhtml#idm46534869597128)
#### 1. [Separating Deployment From Release](#ch07.xhtml#idm46534869515304)
#### 2. [On To Progressive Delivery](#ch07.xhtml#idm46534869508376)
#### 3. [Feature Toggles](#ch07.xhtml#idm46534869499592)
#### 4. [Canary Release](#ch07.xhtml#idm46534869491032)
#### 5. [Parallel Run](#ch07.xhtml#idm46534869481192)
### 7. [Summary](#ch07.xhtml#idm46534869523272)

## TITLE
# Building Microservices
## AUTHOR
by Sam Newman
## COPYRIGHT
Copyright © 2021 Sam Newman. All rights reserved.
Printed in the United States of America.
## PUBLISHER
Published by O’Reilly Media, Inc., 1005 Gravenstein Highway North, Sebastopol, CA 95472.
O’Reilly books may be purchased for educational, business, or sales promotional use. Online editions are also available for most titles ([`http://oreilly.com`](http://oreilly.com)). For more information, contact our corporate/institutional sales department: 800-998-9938 or corporate@oreilly.com.
## STAFF LIST
* Acquisitions Editor: Melissa Duffiel
* Development Editor: Nicole Taché
* Production Editor: Deborah Baker
* Interior Designer: David Futato
* Cover Designer: Karen Montgomery
* Illustrator: Kate Dullea
## PRINTINGS
* July 2021: Second Edition
## REVISIONS
# Revision History for the Early Release
* 2020-05-22: First Release
* 2020-08-27: Second Release
* 2020-10-14: Third Release
## ERRATA
See [http://oreilly.com/catalog/errata.csp?isbn=9781492034025](http://oreilly.com/catalog/errata.csp?isbn=9781492034025) for release details.
## LEGAL
The O’Reilly logo is a registered trademark of O’Reilly Media, Inc. *Building Microservices*, the cover image, and related trade dress are trademarks of O’Reilly Media, Inc.
While the publisher and the author have used good faith efforts to ensure that the information and instructions contained in this work are accurate, the publisher and the author disclaim all responsibility for errors or omissions, including without limitation responsibility for damages resulting from the use of or reliance on this work. Use of the information and instructions contained in this work is at your own risk. If any code samples or other technology this work contains or describes is subject to open source licenses or the intellectual property rights of others, it is your responsibility to ensure that your use thereof complies with such licenses and/or rights.
978-1-492-03395-0
[LSI]

# Chapter 1. What Are Microservices?
Microservices have become an increasingly popular architecture choice in the five years since I wrote the first edition of this book. I can’t claim credit for the subsequent explosion in popularity, but the rush of people to make use of microservice architectures means that while many of the ideas I captured previously are now tried and tested, new ideas have also come into the mix, at the same time as earlier practices have fallen out of favour. So it’s once again time to distill down the essence of microservice architecture, highlighting the core concepts that make them work.
