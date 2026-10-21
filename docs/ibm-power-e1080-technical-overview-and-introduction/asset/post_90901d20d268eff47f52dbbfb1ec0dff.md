
## Trademarks

IBM, the IBM logo, and ibm.com are trademarks or registered trademarks of International Business Machines Corporation, registered in many jurisdictions worldwide. Other product and service names might be trademarks of IBM or other companies. A current list of IBM trademarks is available on the web at “Copyright and trademark information” at http://www.ibm.com/legal/copytrade.shtml

The following terms are trademarks or registered trademarks of International Business Machines Corporation, and might also be trademarks or registered trademarks in other countries.

- Redbooks (logo)®
- AIX®
- C3®
- Db2®
- Easy Tier®
- IBM®
- IBM Cloud®
- IBM FlashCore®
- IBM FlashSystem®
- IBM Spectrum®
- IBM Z®
- Interconnect®
- Micro-Partitioning®
- POWER®
- Power Architecture®
- Power10™
- POWER8®
- POWER9™
- PowerHA®
- PowerVM®
- Redbooks®

The following terms are trademarks of other companies:

The registered trademark Linux® is used pursuant to a sublicense from the Linux Foundation, the exclusive licensee of Linus Torvalds, owner of the mark on a worldwide basis.

LTO, Ultrium, the LTO Logo and the Ultrium logo are trademarks of HP, IBM Corp. and Quantum in the U.S. and other countries.

Ansible, OpenShift, Red Hat, are trademarks or registered trademarks of Red Hat, Inc. or its subsidiaries in the United States and other countries.

VMware, and the VMware logo are registered trademarks or trademarks of VMware, Inc. or its subsidiaries in the United States and/or other jurisdictions.

Other company, product, or service names may be trademarks or service marks of others.

## Preface

This IBM® Redpaper® publication provides a broad understanding of a new architecture of the IBM Power® E1080 (also known as the Power E1080) server that supports IBM AIX®, IBM i, and selected distributions of Linux operating systems. The objective of this paper is to introduce the Power E1080, the most powerful and scalable server of the IBM Power portfolio, and its offerings and relevant functions:

- ► Designed to support up to four system nodes and up to 240 IBM Power10™ processor cores

The Power E1080 can be ordered as a one, two, three, or four node configuration with each node providing up to 60 Power10 processor cores to a maximum 240 Power10 processor cores in a four node configuration.

- ► Designed to supports up to 64 TB memory

The Power E1080 can be initially ordered with the total memory RAM capacity up to 8 TB and supports up to 64 TB in a full combined four system nodes server.

- ► Designed to support up to 32 Peripheral Component Interconnect® (PCIe) Gen 5 slots in a full combined four system nodes server and up to 192 PCIe Gen 3 slots with expansion I/O drawers

- ► Up to over 4,000 directly attached serial-attached SCSI (SAS) disks or solid-state drives (SSDs)

- ► Up to 1,000 virtual machines (VMs) with logical partitions (LPARs) per system

- ► System control unit, providing redundant system master Flexible Service Processor (FSP)

- ► Supports IBM Power System Private Cloud Solution with Dynamic Capacity

This publication is for professionals who want to acquire a better understanding of Power servers. The intended audience includes the following roles:

- ► Customers
- ► Sales and marketing professionals
- ► Technical support professionals
- ► IBM Business Partners
- ► Independent software vendors (ISVs)

This paper does not replace the current marketing materials and configuration tools. It is intended as an extra source of information that, together with existing sources, can be used to enhance your knowledge of IBM server solutions.

## Authors

This paper was produced by a team of specialists from around the world working at IBM Redbooks, Austin Center.

Giuliano Anselmi is an IBM Power Digital Sales Technical Advisor in IBM Digital Sales Dublin. He joined IBM and focused on Power processor-based technology. For almost 20 years, he covered several technical roles. He is an important resource for the mission of his group and serves a reference for Business Partners and customers.

Manish Arora is an IBM Power Technical Specialist working in IBM Techline team in India, holding a bachelor degree in Computer science from IETE, India. He has 15 years of experience in IBM Power systems, AIX, and Linux technologies. He started his career in IBM in 2007 as SSR, managing X series, P series servers, and midrange storage products. He joined IBM in 2016 as Technical specialist. His areas of expertise include Enterprise Power systems, Enterprise Pools, AIX, HMC, and PowerVC.

Ivaylo Bozhinov is a Power Systems subject matter expert (SME) at IBM Bulgaria. His main area of expertise is solving complex hardware and software issues on IBM Power Systems products, IBM AIX, VIOS, HMC, IBM i, PowerVM®, and Linux on Power Systems servers. He has been with IBM since 2015 providing reactive break-fix, proactive, preventative, and cognitive support.

Dinil Das works as Solution Architect in Power Systems Lab Services team for IBM in India. He holds a degree in Technology & Communication from the MG University. Dinil has over 15 years experience working with IBM Power Systems, IBM System Storage, and high performance computing (HPC). Area of expertise includes System performance, availability, and HPC.

Turgut Genc is a senior consultant of IBM Systems Lab Services in the UK, holding an MSc degree in Computer Science Engineering from Yildiz Technical University in Istanbul. He is a versatilist with an 18 years of expertise on Power Systems and AIX. He is the EMEA Power to Cloud Rewards teams leader of IBM PowerVS, Enterprise Pools, Performance, Migration workshops, and Lab Services tools.

Bartlomiej Grabowski is an IBM Champion and a Principal Systems Specialist in DHL IT Services supporting IBM Power Systems around the world. He has over 17 years of experience in enterprise solutions. He holds a bachelor’s degree in computer science from the Academy of Computer Science and Management in Bielsko-Biala, Poland. His areas of expertise include IBM Power Systems, IBM i, IBM PowerHA®, IBM PowerVM, and storage solutions. Bartlomiej also runs technical blog www.theibmi.org. He is a Platinum IBM Redbooks® author.

Madison Lee is a third-year computer science student at Columbia University, working as a Technical Solution Specialist Intern at the IBM San Francisco office this Summer. She joined the IBM Redbooks team, enabling her to work on several residencies. Over the course of her internship, Madison has co-authored two official IBM documents.

Armin Röll works as a IBM Power IT specialist in Germany. He holds a degree in Experimental Physics from the University of Hamburg, Germany. Armin has 25 years of experience in IBM Power and AIX pre-sales technical support. He has written several IBM Redbooks publications about AIX and IBM Power systems hardware and is a Platinum Redbooks author.

The project that produced this material was managed by:

Scott Vetter
PMP, IBM Austin, US

Thanks to the following people for their contributions to this project:

Ryan Achilles, Brian Allison, Ron Arroyo, John Banchy, Joanna Bartz, Bart Blaner,
Gareth Coates, Arnold Flores, Austin Fowler, George Gaylord, Douglas Gibbs, Nigel Griffiths,
Daniel Henderson, Stephanie Jensen, Kyle Keaty, Rajaram B Krishnamurthy,
Fredrik Lundholm, Charles Marino, Michael Mueller, Vincent Mulligan,
Hariganesh Muralidharan, Kaveh Naderi, Mark Nellen, Brandon Pederson,
Michael Quaranta, Ian Robinson, Todd Rosedahl, Bruno Spruth, Bill Starke,

## Now you can become a published author, too!

Here’s an opportunity to spotlight your skills, grow your career, and become a published author—all at the same time! Join an IBM Redbooks residency project and help write a book in your area of expertise, while honing your experience using leading-edge technologies. Your efforts will help to increase product acceptance and customer satisfaction, as you expand your network of technical contacts and relationships. Residencies run from two to six weeks in length, and you can participate either in person or as a remote resident working from your home base.

Find out more about the residency program, browse the residency index, and apply online at:

```
ibm.com/redbooks/residencies.html
```

## Comments welcome

Your comments are important to us!

We want our papers to be as helpful as possible. Send us your comments about this paper or other IBM Redbooks publications in one of the following ways:

- Use the online Contact us review Redbooks form found at:
- ibm.com/redbooks
- Send your comments in an email to:
- redbooks@us.ibm.com
- Mail your comments to:
- IBM Corporation, IBM Redbooks
- Dept. HYTD Mail Station P099
- 2455 South Road
- Poughkeepsie, NY 12601-5400

## Stay connected to IBM Redbooks

- Find us on LinkedIn:
- http://www.linkedin.com/groups?home=&gid=2130806
- Explore new Redbooks publications, residencies, and workshops with the IBM Redbooks weekly newsletter:
- https://www.redbooks.ibm.com/Redbooks.nsf/subscribe?OpenForm
- Stay current on recent Redbooks publications with RSS Feeds:
- http://www.redbooks.ibm.com/rss.html
