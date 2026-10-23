
example of the company control topology at the European level. It has one large SCC containing 88 nodes, and all the others with less than 10 nodes; there is one huge WCC, with 57% of the nodes, with the others scattered around small WCCs with 11.968 nodes on average and (apart from the largest one), none with more than 472 nodes.

## 5.2 Large-Scale Scholarly Knowledge Graphs

The complexity of scholarly data fully follows the 6 Vs of Big Data characteristics towards building Scholarly Knowledge Graphs [405]. The term Big Scholarly Data (BSD) [474] is coined to represent the vast amount of information about scholarly networks including stakeholders and artifacts such as authors, organizers, papers, citations, figures. The heterogeneity and complexity of data and their associated metadata distributed on the Web perfectly qualifies this domain for Big Data challenges towards building Scholarly KGs:

- Volume refers to the ability to ingest and store very large datasets; in the context of scholarly metadata, at least over 114 million scholarly documents [240] were recorded in 2014 as being available in PDF format. In computer science, the total number of publications of the different types is reaching 4 million [423]. Different types of publication in different formats are being published every day in other scientific disciplines.
- Velocity denotes the growth rate generating such data; the average growth rate of scientific publishing is measured as 8 to 9% [61].
- Variety indicates multiple data formats and models; the domain of scholarly communication is a complex domain [29] including many different types of entities with complex interrelationships among them.
- Value concerns the impact of high quality analytics over data; certain facts play enormously important roles in the reputation and basic life of research stakeholders. Providing precise and comprehensive statistics supports researchers with already existing success measurement tools such as number of citations. In additions, deep and mined knowledge with flexible analytics can provide new insights about artifacts and people involved in the scholarly communication domain.
- Veracity refers to the biases, ambiguities, and noise in data; this characteristic is especially applicable in the context of the scholarly communication domain due to deduplication problems [296] and the ambiguity problem for various scholarly artifacts as well as person names.
- Variability of the meaning of the metadata [474].

Discovering high quality and relevant research-related information has a certain influence on the life of researchers and other stakeholders of the communication system [109]. For examples, scholars search for quality in the meaning of fitness for use in questions such as “the venues should a researcher participate” or “the papers should be cited”. There are already attempts to assist researchers in this task, however, resulting in recommendations often being rather superficial and the underlying process neglecting the different aspects that are important for authors [439]. Providing recommendation services to researchers and a comprehensive list of criteria while they are searching for relevant information. Furthermore, having access to the networks of a paper’s authors and their organizations, and taking into account the events in which people participate, enables new indicators for measuring the quality and relevance of research that are not just based on counting citations [438]. Thus each of the Vs of Big Data needs careful management to provide such services for scholarly communities.

## 6 Conclusion

In this chapter, we introduced Knowledge Graphs in a layered perspective: Knowledge Graphs as (1) knowledge representations tools, (2) knowledge management systems, and (3) knowledge application services. We did not focus on a single definition here but presented a multitude of definitions, putting them into the context of this layered perspective. We deliberately stopped short of the chapter being an exhaustive historical overview as excellent overviews have already been written.

We also pointed toward aspects of particular concern: The different ways that particular communities see KGs (top-down or bottom-up, or even middle-layer in focus). We concluded with the practical challenges of KGs by providing typical industrial and academic applications. Throughout the chapter, we discussed the aspect of reasoning being a natural counterpart to this “bigger picture” focus section, and we shall consider reasoning in greater detail in Chap. 6.

Acknowledgements. E. Sallinger acknowledges the support of the Vienna Science and Technology (WWTF) grant VRG18-013 and the EPSRC programme grant EP/M025268/1.

Open Access This chapter is licensed under the terms of the Creative Commons Attribution 4.0 International License (http://creativecommons.org/licenses/by/4.0/), which permits use, sharing, adaptation, distribution and reproduction in any medium or format, as long as you give appropriate credit to the original author(s) and the source, provide a link to the Creative Commons license and indicate if changes were made.

The images or other third party material in this chapter are included in the chapter’s Creative Commons license, unless indicated otherwise in a credit line to the material. If material is not included in the chapter’s Creative Commons license and your intended use is not permitted by statutory regulation or exceeds the permitted use, you will need to obtain permission directly from the copyright holder.

![](img/c21ce6788ba392bc08a8057d8b20603e_42_0.png)

## Chapter 3
Big Data Outlook, Tools,
and Architectures

Hajira Jabeen(✉)

CEPLAS, Botanical Institute, University of Cologne, Cologne, Germany
hajira.jabeen@uni-koeln.de

**Abstract.** Big data is a persistent phenomena, the data is being generated and processed in a myriad of digitised scenarios. This chapter covers the history of ‘big data’ and aims to provide an overview of the existing terms and enablers related to big data. Furthermore, the chapter covers prominent technologies, tools, and architectures developed to handle this large data at scale. At the end, the chapter reviews knowledge graphs that address the challenges (e.g. heterogeneity, interoperability, variety) of big data through their specialised representation. After reading this chapter, the reader can develop an understanding of the broad spectrum of big data ranging from important terms, challenges, handling technologies, and their connection with large scale knowledge graphs.

## 1  Introduction

The digital transformation has impacted almost all aspects of modern society. The past decade has seen tremendous advancements in the areas of automation, mobility, the internet, IoT, health, and similar areas. This growth has led to enormous data-generation facilities, and data-capturing capabilities.

In the first section “Outlook”, we review the definitions and descriptions of big data and discuss the drivers behind big data generation, the characteristics exhibited by big data, the challenges offered by big data, and the handling of this data by creating data value chains. In the section “Tools and Architectures”, we cover the software solutions and architectures used to realise the big data value chains. We further cover characteristics and challenges relating to big data. The section “Harnessing the Big Data as Knowledge Graphs” connects knowledge graphs and big data, outlining the rationale and existing tools to handle large-scale knowledge graphs.

## 2  Big Data: Outlook

Today, big data is widespread across and beyond every aspect of everyday life. This trend of increasing data was first envisioned and defined years ago. Notably, the first evidence of the term big data comes from a paper [87] published in 1997, where the authors described the problem as *Big Data* when the data do not fit in the main memory (core) of a computing system, or on the local disk. According to the Oxford English Dictionary (OED), big data is defined as: “data of a very large size, typically to the extent that its manipulation and management present significant logistical challenges.” Later, when the terms velocity, variety, and volume were associated as characteristics of big data, the newer definitions of the term ‘big data’ came to cover these characteristics, as listed below:

- 1. “Big data is high volume, high velocity, and/or high variety information assets that require new forms of processing to enable enhanced decision making, insight discovery and process optimization,” [271,297].
- 2. “When the size of the data itself becomes part of the problem and traditional techniques for working with data run out of steam,” [288].
- 3. Big Data is “data whose size forces us to look beyond the tried-and-true methods that are prevalent at that time,” [217].
- 4. “Big Data technologies are a new generation of technologies and architectures designed to extract value economically from very large volumes of a wide variety of data by enabling high-velocity capture, discovery, and/or analysis,” [470].
- 5. “Big data is high-volume, high-velocity and high-variety information assets that demand cost-effective innovative forms of information processing for enhanced insight and decision making,” [271].
- 6. “Big Data is a term encompassing the use of techniques to capture, process, analyse and visualize potentially large datasets in a reasonable timeframe not accessible to standard IT technologies.” By extension, the platform, tools and software used for this purpose are collectively called “Big Data technologies,” [98].
- 7. “Big data can mean big volume, big velocity, or big variety,” [414].
- 8. “The term is used for a collection of datasets so large and complex that it becomes difficult to process using on-hand database management tools or traditional data processing applications”.
- 9. “Big data represents the information assets characterized by such a high volume, velocity and variety to require specific technology and analytical methods for its transformation into value”².

Regardless of the defining text, big data is a persistent phenomenon and is here to stay. We take a brief overview of the key enabling technologies that made big data possible in the following section.

## 2.1  Key Technologies and Business Drivers

As recently as the year 2000, digital information only constituted about one quarter of all the stored information worldwide³. Other information was mainly

¹ http://en.wikipedia.org/.
² http://en.wikipedia.org/.
³ https://www.foreignaffairs.com/articles/2013-04-03/rise-big-data.
