
+   22. Rare Commons, OMIM-Information, https://www.rarecommons.org/en/recurso/omim, 2020.
+   23. National Library of Medicine, Unified Medical Language System (UMLS), https://www.nlm.nih.gov/research/umls/index.html, 2020.
+   24. SNOMED International, http://www.snomed.org/snomed-ct/five-step-briefing, 2020.
+   25. Dragusin, R., Petcu, P., Lioma, C., Larsen, B., Jørgensen, H. L., Cox, I. J., ... Winter, O., FindZebra: a search engine for rare diseases. *Int. J. Med. Inf.*, 82, 6, 528–538, 2013.
+   26. FindZebra Rare Disease Search, https://www.findzebra.com/, 2020.
+   27. Karami, M., and Rahimi, A., Semantic web technologies for sharing clinical information in healthcare systems. *Acta Informatica Medica*, 27, 1, 4–7, 2019.

## Atypical Point of View on Semantic
Computing in Healthcare

L. Mayuri¹* and K. M. Mehata²†

¹Health Informatist, Chennai, India
²Director (Sponsored Research), VIT-AP University, Andhra Pradesh, India

## Abstract

Medical information systems are built using good engineering concepts. It outperforms its competitors when it comes to recording detailed and sophisticated data. This chapter examines recent improvements in medical terminology tools and its application methodologies for semantic reasoning and healthcare interoperability.

Healthcare information is multifaceted, sensitive, and siloed. Medical terminology systems are designed to promote semantic interoperability across different healthcare information systems. Due to data granularity requirements, current reference terminology mapping solutions that enable semantic interoperability of data between health systems are heavyweight. They are difficult to use and maintain for health professionals. They are not fit for usage by non-clinical or non-experts. Patients who were once passive are now active consumers. Understanding and integrating consumer health languages can help healthcare consumers and professionals better communicate.

The rapidly growing interdisciplinary field of healthcare analytics attempts to provide semantically standardized language to help clinical decision support systems and related technology make positive advances. Despite recent advances, a comprehensive terminology system that accommodates all clinical and administrative data according to local needs will remain a distant dream for the foreseeable future. Different stakeholders will need to work together to examine present fragmented upstream, midstream, and downstream care protocols.

**Keywords:** Healthcare, health data, linked data, clinical data, semantic interoperability, clinical text, data integration, semantic data

### 13.1 Introduction

Usage of paper to record detailed facts and figures about illnesses began in the early twentieth century. As medical practices evolved, so did the pile of paperwork. Swollen case files of individual patients became difficult for medical practitioners to process mentally and for the medical record department to store and maintain in medical record libraries. By the mid-twentieth century, myriads of other problems clouded the inherent efficiencies of paper-based medical records and founded the use of computers to address the “information explosion” in medical practices. Computers came in use to complement conventional paper records by capturing and organizing key data in medical records [1–3].

In its nascent stages, the pace of adoption of information technology (IT) within health for supporting care protocols was decades behind other fields such as finance, owing to uncertainty in general, and high costs of procurement and operations in specific. Improvements in IT in the late twentieth century pushed forward the adoption of computers to some extent. The Institute of Medicine in the United States (U.S.) reported that electronic patient records is essential standard for all medical records. The U.S. General Accounting Office reported that medical records offer great potential to bring improvements in patient care, by increasing efficiency, and reducing costs. The report also called for development of standards to ensure uniform electronic recording and transmission of medical information [4–6].

By the end of the twentieth century, healthcare systems of many countries, such as Canada and Australia, revolved around the electronic health records (EHRs). As IT systems started generating and aggregating health data, methods to analyze data, and infer knowledge lead to defining systems to aid decision support in clinical settings. The full spectrum of health data used for medical knowledge representation comes from varied sources like problem and medication lists, procedure and treatment notes, social determinants of health, and genomics and proteomics data. Improved networking technologies allowed connecting computers to share patient information for continuity of care, sprouting the need for data exchange standards and interface terminologies [7–10].

By the dawn of the twenty-first century, the practice of medicine had undergone many economic transformations on the backbone of IT. Medical information systems use sound engineering principles for their construction. It is second to none of its peer systems in recording detailed and complex data. In parallel, advancements in IT came to be available in scales and costs unreachable in the last century. In the current IT era, there is no dearth of tools and techniques for mining undecipherable knowledge from the piles of EHR data aggregated in medical record system libraries. Yet, health data remains a double-edged sword because of its inherent, unique, and multi-faceted characteristics. In theory, EHR systems are able to collect, store, analyze, and exchange near-infinite amounts of health information. In reality, IT system barriers such as lack of interoperability and incomplete data standards hinder health data collection, sharing, and usability across the care continuum.

Healthcare delivery system’s fragmented legacy structure needs major transformation. The quest to find the delicate and elusive balance between healthcare delivery cost and quality remains unconquered. Disparities among the different health data source repositories pose bizarre challenges to exploit the structured and unstructured data for its true and complete value [11, 12]. In a special publication [13], the National Academy of Medicine reports on the current and near-term AI solutions; highlighting the challenges, limitations, and best practices for AI development, adoption, and maintenance. The report further emphasizes the need for thoughtful, inclusive healthcare AI while balancing the further exacerbation of existing health- and technology-driven disparities.

### 13.2 Mind the Language

Ever-evolving medical practices make it challenging to baseline a clinical technology landscape. Health data is primarily heterogeneous, highly sensitive, and usually siloed. Sourcing and handling such transactional data is not a straightforward task. Medical terminology systems aim to provide semantic interoperability among disparate healthcare information systems. Common terminology standards used in health information and technology are SNOMED-CT, RxNORM, LOINC, ICD-x-CM, and UMLS. These standard vocabularies and code sets have specific usage. Current reference terminology mapping solutions that enable semantic interoperability of data between health systems are heavyweight because

of data granularity constraints. Health professionals find them difficult to use and costly to maintain. They are unsuited for non-clinical or non-expert use. Passive patients of yesteryears are active consumers of health information today. Understanding and integrating consumer health vocabularies will improve health dialog between healthcare consumers and health professionals.

This section summarizes the problem of semantics in healthcare and general solution trends adopted in use.

#### 13.2.1 Why Words Matter

From time immemorial, physicians have used free text narrative formats to document findings from health assessments and outcomes from care protocols. Using natural language for narratives should enable understanding the meaning of words (semantics) and the meaning of the intent of words (pragmatics), the two essential elements in communication. In fact, free text narratives lack structure, and heavily depend on the user’s communication style, making them ambiguous to other users. With computers, it became possible to enter data in a highly structured format. However, this took away the expressiveness and flexibility of narrative format from physicians. Also, interpretation and reconstruction of the (semantic) meaning from structured data is usually difficult because of the loss of contextual (pragmatic) information [14–17].

#### 13.2.2 What Words Matter

Clinical information systems capture multimodal data. In contrast, medical terminology systems, such as vocabularies, taxonomies, and ontologies, support converting human-readable clinical terms to computer-processable formats. Thus, while medical terminologies define context-independent meanings of domain terms, clinical information models define the situational contexts using domain terms, and together, they provide the standards for the exchange of meaningful clinical information. In practice, multiple domain or subject-specific terminologies and overlapping information models at different levels of abstraction and detail represent clinical data [18]. Various industry-specific associations, standards organizations, and vendors build individual terminologies or ontologies for the systematic recording of clinical data for specific use cases [19] as there is no single universally accepted standard for representing clinical concepts [20–22].
