
## Data Management at Scale

### Modern Data Architecture with Data Mesh and Data Fabric

> Compliments of Microsoft
> Microsoft

![](img/999fd6f5dfec4455873f6d26acc75ff0_0_0.png)

Piethein Strengholt

## Data Management at Scale

Data management is subject to disruption. Trends like artificial intelligence, cloudification, ecosystem connectivity, microservices, open data, software as a service, and new software delivery models are causing a paradigm shift in the way data management is practiced. Organizations need to face the fact that decentralization is inevitable. In this practical book, author Piethein Strengholt explains how to establish a future-proof and scalable data management practice. He’ll cut through new concepts like data mesh and data fabric and demonstrate what a next-gen data architecture will look like.

Executives, architects and engineers, analytics teams, and compliance and governance staff will learn how to shape data management according to their needs. Strengholt provides blueprints, principles, observations, best practices, and patterns to get you up to speed.

This guide shows you how to:

- Examine data management trends, including regulatory requirements, privacy concerns, and new developments such as data mesh and data fabric
- Go deep into building a modern data architecture, including cloud data landing zones, domain-driven design, data product design, and more
- Explore data governance and data security, master data management, self-service data marketplaces, and the importance of metadata

Piethein Strengholt is the chief data officer for Microsoft Netherlands. He acts as a counterpart to CDOs and is a driving force in the community and the product group. Piethein is also a prolific blogger and regularly speaks about the latest trends in data management, including the data mesh concept, data governance, and strategy at scale.

> “For startups, larger companies, and old industrial giants alike, this pragmatic yet visionary book is the essential guide to modern data management. It’s about scaling and staying competitive. There is no other book like this one out there. It’s a must-read.”
> —Ole Olesen-Bagneux
> author of The Enterprise Data Catalog

> “Data management is challenging enough at a small scale. This book provides a very detailed and solid foundation for data management today and in the future.”
> —Joe Reis
> coauthor of Fundamentals of Data Engineering and ‘recovering data scientist’

## Data at scale with Azure

Managing data at scale starts with choosing a platform that supports data anywhere: On-premises, hybrid, or a fully modernized environment. Azure offers a full range of deployment options from edge to cloud with the lowest total cost of ownership.

Learn more about how Azure can help you migrate and modernize your data environments with flexible solutions and pricing.

Get started with an Azure free account and a US $200 credit.

## SECOND EDITION

## Data Management at Scale

### Modern Data Architecture with Data Mesh and Data Fabric

Piethein Strengholt

## Data Management at Scale

by Piethein Strengholt

Copyright © 2023 Piethein Strengholt. All rights reserved.
Printed in the United States of America.
Published by O'Reilly Media, Inc., 1005 Gravenstein Highway North, Sebastopol, CA 95472.

O'Reilly books may be purchased for educational, business, or sales promotional use. Online editions are also available for most titles (http://oreilly.com). For more information, contact our corporate/institutional sales department: 800-998-9938 or corporate@oreilly.com.

Acquisitions Editor: Michelle Smith
Development Editor: Shira Evans
Production Editor: Katherine Tozer
Copyeditor: Rachel Head
Proofreader: Piper Editorial Consulting, LLC
Indexer: nSight, Inc.
Interior Designer: David Futato
Cover Designer: Karen Montgomery
Illustrator: Kate Dullea

April 2023: Second Edition

## Revision History for the Second Edition

2023-04-10: First Release

See https://oreilly.com/catalog/errata.csp?isbn=9781098138868 for release details.

The O'Reilly logo is a registered trademark of O'Reilly Media, Inc. Data Management at Scale, the cover image, and related trade dress are trademarks of O'Reilly Media, Inc.

The views expressed in this work are those of the author, and do not represent the publisher's views. While the publisher and the author have used good faith efforts to ensure that the information and instructions contained in this work are accurate, the publisher and the author disclaim all responsibility for errors or omissions, including without limitation responsibility for damages resulting from the use of or reliance on this work. Use of the information and instructions contained in this work is at your own risk. If any code samples or other technology this work contains or describes is subject to open source licenses or the intellectual property rights of others, it is your responsibility to ensure that your use thereof complies with such licenses and/or rights.

This work is part of a collaboration between O'Reilly and Microsoft. See our statement of editorial independence.

978-1-098-15207-9

[LSI]

## Table of Contents

- Foreword .................................................................................... xiii
- Preface ........................................................................................... xv
- 1. The Journey to Becoming Data-Driven ............................................................ 1
  - Recent Technology Developments and Industry Trends ........................................ 2
  - Data Management .................................................................................... 4
  - Analytics Is Fragmenting the Data Landscape ................................................. 8
  - The Speed of Software Delivery Is Changing .................................................. 9
  - The Cloud's Impact on Data Management Is Immeasurable ................................. 10
  - Privacy and Security Concerns Are a Top Priority .......................................... 11
  - Operational and Analytical Systems Need to Be Integrated ............................... 12
  - Organizations Operate in Collaborative Ecosystems ....................................... 13
  - Enterprises Are Saddled with Outdated Data Architectures .............................. 14
  - The Enterprise Data Warehouse: A Single Source of Truth ............................... 14
  - The Data Lake: A Centralized Repository for Structured and Unstructured Data ............................................................................ 17
  - The Pain of Centralization ....................................................................... 18
  - Defining a Data Strategy ......................................................................... 19
  - Wrapping Up .......................................................................................... 22
- 2. Organizing Data Using Data Domains............................................................. 25
  - Application Design Starting Points ............................................................. 26
  - Each Application Has a Data Store .............................................................. 26
  - Applications Are Always Unique ................................................................... 26
  - Golden Sources ......................................................................................... 26
  - The Data Integration Dilemma ....................................................................... 27
  - Application Roles ...................................................................................... 27
  - Inspirations from Software Architecture 29
  - Data Domains 32
  - Domain-Driven Design 32
  - Business Architecture 35
  - Domain Characteristics 45
  - Principles for Distributed and Domain-Oriented Data Management 50
  - Design Principles for Data Domains 51
  - Best Practices for Data Providers 53
  - Domain Ownership Responsibilities 55
  - Transitioning Toward Distributed and Domain-Oriented Data Management 56
  - Wrapping Up 57
- 3. Mapping Domains to a Technology Architecture....... 61
  - Domain Topologies: Managing Problem Spaces 62
  - Fully Federated Domain Topology 62
  - Governed Domain Topology 66
  - Partially Federated Domain Topology 69
  - Value Chain–Aligned Domain Topology 70
  - Coarse-Grained Domain Topology 71
  - Coarse-Grained and Partially Governed Domain Topology 73
  - Centralized Domain Topology 74
  - Picking the Right Topology 77
  - Landing Zone Topologies: Managing Solution Spaces 78
  - Single Data Landing Zone 80
  - Source- and Consumer-Aligned Landing Zones 87
  - Hub Data Landing Zone 88
  - Multiple Data Landing Zones 89
  - Multiple Data Management Landing Zones 92
  - Practical Landing Zones Example 93
  - Wrapping Up 95
- 4. Data Product Management....... 99
  - What Are Data Products? 99
  - Problems with Combining Code, Data, Metadata, and Infrastructure 100
  - Data Products as Logical Entities 101
  - Data Product Design Patterns 103
  - What is CQRS? 104
  - Read Replicas as Data Products 106
  - Design Principles for Data Products 107
  - Resource-Oriented Read-Optimized Design 108
  - Data Product Data Is Immutable 109
  - Using the Ubiquitous Language 109
