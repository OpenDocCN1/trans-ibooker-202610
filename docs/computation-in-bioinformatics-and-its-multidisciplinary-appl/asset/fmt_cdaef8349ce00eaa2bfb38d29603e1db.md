
The past couple of years will be remembered for the COVID-19 pandemic and ensuing lockdowns, wherein almost every country saw a series of lockdowns and consequent suffering. To overcome the pandemic, the discovery of vaccines and other alternative therapies were attempted worldwide. Currently, the discovery of new solutions to the pandemic can be facilitated through the use of promising bioinformatics techniques and integrated approaches. Hence, this book provides readers with an understanding of the use of bioinformatics tools in new drug design and the discoveries which are inevitable in the present situation. The book covers a broad spectrum of the bioinformatics field, starting with the basic principles, concepts, and application areas under multidisciplinary sections. Also covered is the role of bioinformatics in drug design and discovery, including aspects of molecular modeling. Some of the chapters provide detailed information on bioinformatics-related topics, such as silico design, protein modeling, DNA microarray analysis, DNA-RNA barcoding, and gene sequencing, all of which are currently needed in the industry. Also included are specialized topics, such as bioinformatics in cancer detection, genomics and proteomics, which are highly relevant to the present scenario. Moreover, a few chapters explain highly advanced topics, like machine learning and covalent approaches to drug design and discovery, all of which are significant in pharma and biotech research and development. Therefore, the contents of this book will be useful for students, scientists, researchers, and professionals working in the field of bioinformatics, drug design, pharmacoinformatics and medicinal chemistry under the umbrella of academia and industry.

Our sincere gratitude especially goes to all the contributors for their useful insights concerning bioinformatics and its multidisciplinary applications. We sincerely thank Scrivener Publishing for their assistance, constant support, and patience in finalizing this book.

**S. Balamurugan**  
*Coimbatore, India*

**Krishnan Anand**  
*Bloemfontein, South Africa*

**Dinesh Goyal**  
*Jaipur, India*

**Balakumar Chandrasekaran**  
*Amman, Jordan*

**Boomi Pandi**  
*Karaikudi, India*

# 1 Bioinfomatics as a Tool in Drug Designing

**Rene Barbie Browne, Shiny C. Thomas and Jayanti Datta Roy[\*](#c01.xhtml#fn-1)**  
*Department of BioSciences, Assam Don Bosco University, Sonapur, Assam, India*

> ***Abstract***
> 
> Drug discovery is the method of identifying and validating a disease target and discovering and developing a chemical compound which can interact with its specific target. This process is very complex and time consuming, requiring multidisciplinary expertise and innovative approaches. To overcome the difficulties and complexity, *in silico* approach is used that reduces the time and expenditure. This chapter addresses the importance of bioinformatics in drug designing. It focuses on bioinformatics tools like `AutoDock`, `LigPlot`, `FlexX`, and many other softwares which play an important role in rational designing of drug. Thus, the main goal of this chapter is to provide an overview of the importance of bioinformatics tools in designing a drug.
> 
> ***Keywords**:* `AutoDock`, `LigPlot`, `FleX`, `GenBank`, `SWISS-PROT`, `PDB`

## 1.1 Introduction

Bioinformatics is a multidisciplinary field of life sciences merging biology, computer science, and information technology into a single discipline \[1\]. A wide range of subject areas is included in this field. These subject areas are structural biology, gene expression studies, and genomics. Computational techniques play an important role analyzing information that are associated with biomolecules on a large scale \[2\].
The main goal of bioinformatics aims toward better understanding of living cells and how it functions at the molecular level. Besides being essential for basic genomic and molecular biology research, bioinformatics plays a pivotal role on many areas of biotechnology and biomedical sciences \[3\]. In this aspect, bioinformatics play a vital role in designing of novel drugs. The interactions between protein and ligand investigated computationally provide rational basis for rapidly identifying novel synthetic drugs \[4\]. Information available regarding the 3D structure of proteins makes it easier to design molecule in such a way that they are capable of binding to the receptor site of a target protein with great affinity and specificity. Consequently, it significantly reduces time and cost necessary to develop drugs with higher potency, fewer side effects, and less toxicity than using the traditional trial-and-error approach.
This field of computational study has also reduced the sacrifice of animals in research. Nowadays, the number of potential drug candidate molecules is increasing with the use of computational simulation and informatics methods. These methods help in reducing the number of animals sacrificed in drug discovery process \[5\]. By efficient use of existing knowledge, computational studies have also helped in reducing the number of animal experiments which is required in basic biological sciences \[6\].
Bioinformatics tools are now appreciably used for developing novel drugs, leading to a new variety of research. Discovery and development of a new drug is generally very complex process consuming a whole lot of time and resources. So, bioinformatics techniques in designing tools are now broadly used so as to growth the efficiency of designing and developing a novel synthetic drug \[4\]. Drug discovery is the method of identifying, validating a disease target, followed by designing a chemical compound which can interact with that target resulting in inhibition of biological response which increases the rate of the disease. All these processes can be supported by various computational tools and methodology. Some of the factors which need to be observed during identification of the drug target are sequences of protein and nucleotide, mapping information, functional prediction, and data of protein and gene expression. Bioinformatics tools have helped in collecting the information of all these factors leading to the development of primary and secondary databases of nucleic acid sequences, protein sequences, and structures. Some of the commonly used databases include `GenBank`, `SWISS-PROT`, `PDB`, `PIR`, `SCOP`, and `CATH`. These databases have become indispensable tools to accumulate information regarding disease target. Databases like `PubChem` and `ChemFaces` provide structural and biological information of known drug like compounds which helps to identify the drug target for designing drug in research field \[7\]. These databases help in saving time, money, and efforts of the researchers.
Designing of drugs using bioinformatics tools can be broadly classified into two main categories, *viz*.,

1. a) Structure-based drug design (`SBDD`)
2. b) Ligand-based drug design (`LBDD`)

**a) Structure-Based Drug Design (`SBDD`):** Designing of drugs using `SBDD` method utilizes the 3D structure of the biological target which can be acquired via X-ray crystallography or NMR spectroscopy techniques \[8\]. Candidate drugs can be predicted on the basis of its binding affinity to the target using the structural information of the biological target. If the structure of the biological target/receptor is unavailable, then in that case, the structure can be predicted using homology modeling. It usually requires the amino acid sequence of the target protein, which when submitted constructs models that can be compared with the 3D structure of similar homologous protein (template). In order to know the interactions or bio-affinity for all tested compounds, molecular docking of each compound is performed into the binding site of the target, predicting the electrostatic fit between them.
