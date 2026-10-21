

# O'REILLY

# Cloud Native
Infrastructure
with Azure

Building and Managing
Cloud Native Applications

Early
Release
RAW &
UNEDITED

Nishant Singh & Michael Kehoe

![](img/8d47d1b5dbca55e784ae602145a894c7_0_0.png)

# Cloud Native Infrastructure with Azure

Building and Managing Cloud Native Applications

With Early Release ebooks, you get books in their earliest form—the authors’ raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

Nishant Singh and Michael Kehoe

# Cloud Native Infrastructure with Azure

by Nishant Singh and Michael Kehoe

Copyright © 2022 Nishant Singh and Michael Kehoe. All rights reserved.

Printed in the United States of America.

Published by O’Reilly Media, Inc., 1005 Gravenstein Highway North, Sebastopol, CA 95472.

O’Reilly books may be purchased for educational, business, or sales promotional use. Online editions are also available for most titles (http://oreilly.com). For more information, contact our corporate/institutional sales department: 800-998-9938 or corporate@oreilly.com.

Acquisitions Editor: Jennifer Pollock

Development Editor: Rita Fernando

Production Editor: Christopher Faucher

Copyeditor: Audrey Doyle

Proofreader: Kim Cofer

Indexer: Ellen Troutman-Zaig

Interior Designer: David Futato

Cover Designer: Karen Montgomery

Illustrator: Kate Dullea

February 2022: First Edition

# Revision History for the Early Release

- 2021-12-07: First Release
- 2022-01-20: Second Release

See http://oreilly.com/catalog/errata.csp?isbn=9781492090960 for release details.

The O’Reilly logo is a registered trademark of O’Reilly Media, Inc. *Cloud Native Infrastructure with Azure*, the cover image, and related trade dress are trademarks of O’Reilly Media, Inc.

The views expressed in this work are those of the authors and do not represent the publisher’s views. While the publisher and the authors have used good faith efforts to ensure that the information and instructions contained in this work are accurate, the publisher and the authors disclaim all responsibility for errors or omissions, including without limitation responsibility for damages resulting from the use of or reliance on this work. Use of the information and instructions contained in this work is at your own risk. If any code samples or other technology this work contains or describes is subject to open source licenses or the intellectual property rights of others, it is your responsibility to ensure that your use thereof complies with such licenses and/or rights.

978-1-492-09089-2

## Preface

## A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the Preface of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at rfernando@oreilly.com.

Cloud computing has been widely adopted as a model of next-generation digital business transformation that drives growth and innovation. Nowadays, customers look for an ecosystem and experience that is fast and that seamlessly integrates with their existing services. From an enterprise perspective, the cloud delivers services for consumers and businesses in a way that is scalable, highly reliable, and available. From an end-user perspective, the cloud provides a simple model for acquiring computing services without needing to fully understand the underlying infrastructure and technology.

To take full advantage of the speed and agility of cloud services, many existing applications have been transformed into cloud native applications, and new solutions are being built for the cloud first. Cloud native applications are built from the ground up to embrace rapid change, large scale, and resilience. By default, the underlying infrastructure for cloud native applications plays a critical role in efficiently serving a business’s needs. If the underlying infrastructure is not architected with the correct practices, even the best cloud native applications will fail in production environments.

This book explores how modern cloud native infrastructures on Azure can be built and managed in a production environment, along with various requirements and design considerations of cloud native applications.

### Who Should Read This Book

This book provides a simple yet comprehensive introduction to the cloud native landscape and all the major technologies that engineers use to build such reliable environments. The book is for site reliability engineers (SREs), DevOps engineers, Solution architects, Azure enthusiasts, and anyone who is involved in building, migrating, deploying, and managing the day-to-day operations of a cloud native workload.

The book assumes that you possess basic knowledge of the cloud and DevOps culture in general. But even if you don’t, and you want to better understand the buzz around cloud native and other fancy technologies, this book is still the right place for you to get started.

### Goals of This Book

By the end of this book, you will be able to follow and build your own infrastructure on Microsoft Azure. We present a sequential introduction to the major components of the cloud native world and how to use, deploy, and maintain them over Azure. Additionally, you will be introduced to the need for cloud native technologies in today’s brave new world, the problems they solve, and hands-on best practices.

In this book, you will:

- Learn how to build cloud native infrastructure over Azure following the path of the Cloud Native Computing Foundation’s (CNCF) cloud native landscape
- Ascertain which technologies to use at different stages of design
- Discover how to address the problems you may face while managing and operating cloud native infrastructure, as well as the technologies that help solve them

### Navigating This Book

This book is organized as follows:

- Chapter 1 provides a basic introduction to the cloud and the need for cloud native technology and its adaptations.
- Chapter 2 covers the fundamentals of infrastructure as code (IaC) with Terraform and Packer, and introduces Azure and Ansible as provisioners/configuration managers.
- Chapter 3 introduces containers, and container runtimes such as containerd, Docker, and CRI-O. Different types of container registries are also discussed.
- Chapter 4 discusses Kubernetes and includes the details necessary to use the infrastructure in upcoming chapters.
- Chapter 5 deals specifically with the Azure Kubernetes Service and the Helm package manager.
- Chapter 6 focuses on how modern cloud native infrastructure can be made observable.
- Chapter 7 is all about service discovery and service mesh. We introduce the CoreDNS DNS server and the Istio service mesh in this chapter.
- Chapter 8 covers network and policy management, including the Calico, Flannel, Cilium, Azure Policy, and Open Policy Agent container networking interfaces.
- Chapter 9 discusses how persistent storage systems are deployed on cloud native infrastructure, with a focus on Azure Storage, Vitess, Rook, TiKV, and etcd.
- Chapter 10 is primarily focused on messaging and streaming platforms such as NATS and Azure messaging services.
- Chapter 11 is a simple introduction to the serverless side of things in the cloud native landscape.
- Chapter 12 serves as a wrap-up of everything we discussed in the preceding chapters.

### Acknowledgments

We would like to thank Rita Fernando, Nicole Taché, and Jennifer Pollock, our editors at O’Reilly who were supportive and awesome as they guided us through the writing process. We would also like to thank our tech reviewers for their valuable contributions to the book. This book would not have been possible without the thorough and amazing reviews and feedback from Matt Franz, Peter Jausovec, Liudmila Sinkevich, Steve Machacz, and Alexander Kabonov.

In addition, we would like to highlight Matt Franz for his thorough reviews, suggestions, and contributions to improve the quality of the book. Matt gave extensive feedback on chapters and contributed Chapter 10.

Nishant would like to thank his mom, his dad, and his wife, Mahek, for being so patient and supportive while he was working on the book. He would also like to thank the O’Reilly team and his peers at LinkedIn who supported the idea from the very beginning.

Michael would like to thank his family, who encouraged him to follow his dreams and never give up. He would also like to thank his girlfriend, Jennifer, for her support and patience while he was working on this book, the O’Reilly staff for helping to bring the book from idea to reality, and all of the reviewers for their time.

