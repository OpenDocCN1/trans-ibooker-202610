
85\. Afolabi-Balogun, N., Inuwa, H., Ishiyaku, M., Bakare-Odunoola, M., Nok, A., Isolation and characterization of a mannose-binding insecticidal lectin gene from Allium sativum (garlic) and its putative role in insect resistance using bioinformatics tools. *Infect. Genet. Evol*., *12*, 7, 1508–1512, 2012.

86\. Vassilev, D.; Leunissen, J.; Atanassov, A.; Nenov, A.; Dimov, G., Application of Bioinformatics in Plant Breeding. Biotechnology & Biotechnological Equipment, 19 (sup 3), 139–152, 2005. [https://www.tandfonline.com/doi/abs/10.1080/13102818.2005.10817293](http://www.tandfonline.com/doi/abs/10.1080/13102818.2005.10817293).

87\. Zeli, W., Hengyue, Z., Xianxi, Y., Xiuling, J., Xinzheng, L., Anatomical studies on the drought resistant varieties in maize. *Acta Bot. Boreal.-Occid. Sin*., *18*, 4, 581–583, 1998.

88\. Gao Y. *et al*., Identification of Rice Drought-Resistant Gene Based on Gene Expression Profiles and Network Analysis Algorithm. In: Huang DS., Jo KH. (eds.) Intelligent Computing Theories and Application. ICIC 2020. Lecture Notes in Computer Science, vol. 12464. Springer, Cham, 2020. [https://doi.org/10.1007/978-3-030-60802-6\_26](http://doi.org/10.1007/978-3-030-60802-6_26).

1.  [\*](#c07.xhtml#rfns)*Corresponding author*: [nikhil.08oct@gmail.com](mailto:nikhil.08oct@gmail.com)

# 8 DNA Microarray Analysis: From Affymetrix CEL Files to Comparative Gene Expression

**Sandeep Kumar^([1](#c08.xhtml#fn1))[\*](#c08.xhtml#fns), Shruti Shandilya^([2](#c08.xhtml#fn2)), Suman Kapila^([3](#c08.xhtml#fn3)), Mohit Sharma^([4](#c08.xhtml#fn4),[5](#c08.xhtml#fn5)) and Nikhil Agrawal^([6](#c08.xhtml#fn6)[†](#c08.xhtml#fnd))**

^(*[1](#c08.xhtml#rfn1)*)*International Institute of Veterinary Education & Research, Haryana, Rohtak, India*
^(*[2](#c08.xhtml#rfn2)*)*Department of Applied Physics, Aalto University, Espoo, Finland*
^(*[3](#c08.xhtml#rfn3)*)*ICAR-National Dairy Research Institute, Haryana, India*
^(*[4](#c08.xhtml#rfn4)*)*Post Graduate School of Molecular Medicine, Medical University of Warsaw, Warsaw, Poland*
^(*[5](#c08.xhtml#rfn5)*)*Malopolskie Centre of Biotechnology, Jagiellonian University, Krakow, Poland*
^(*[6](#c08.xhtml#rfn6)*)*College of Health Sciences, University of KwaZulu-Natal, Westville, Durban, South Africa*

> ***Abstract***
> 
> Microarray technique is widely used for the rapid detection and comparison of expression of thousands of genes at the same time. For the microarray data analysis, robust and reproducible protocols are being generated day by day. Bioconductor provides several free microarray data analysis packages. The analysis includes the following processes: quality control, normalization, and calculation of differential gene expression. This chapter demonstrates a protocol to find out the differentially expressed genes from the raw data (.CEL files) using the Bioconductor tools.
> 
> ***Keywords**:* Microarray, bioconductor, genes

## 8.1 Introduction

DNA microarray, also known as DNA chip, is a molecular technique used to quantify mRNA expression in the cells/tissues \[1\]. In this technique, an array of hundreds of known DNA fragments called as probes is immobilized on a chip in defined positions. This chip can be made up of either silicon or nylon. Using the microarray technique, thousands of genes can be studied simultaneously to compare different conditions \[2\]. Spotted microarray and oligonucleotide chips are the most popular types of microarray among the users for expression study, and the difference between these is the way to immobilize the probe on the slide. In spotted or DNA microarray, the probes are synthesized separately and immobilized mechanically to the chip. However, in oligonucleotide chips, the probes are directly synthesized on the slide. A probe set is used in the oligonucleotide chip instead of a single probe \[3\]. Microarray data analysis includes image analysis, quality control, normalization, and differential gene expression. In this chapter, the pipeline for the analysis of microarray data is developed and run in RStudio, an integrated development environment (IDE) for R. The packages are downloaded from Bioconductor. It provides advance tools for microarray data analysis. Bioconductor is built on an open-source platform, R programming language. Bioconductor software constituents are available as R packages. Here, we will use affy, affyQCReport, limma, gdata, and gplots packages for the analysis. This chapter will provide a detailed methodology for the analysis of Affymetrix microarray data \[5\].

## 8.2 Data Processing

### 8.2.1 Installation of Workflow

The packages are installed via the BiocManager using the following commands in R studio.

```
if (!requireNamespace("BiocManager", quietly = TRUE))
install.packages("BiocManager")
BiocManager::install("affy")
BiocManager::install("affyQCReport")
BiocManager::install("limma")
BiocManager::install("gdata")
BiocManager::install("gplot")
```

After the successful installation of packages, all packages are loaded in order. To call the packages, `library()` function is used. This will also load all the other packages necessary to run the workflow.

```
library(affy)
library(affyQCReport)
library(limma)
library(gdata)
library(gplots)
```

If several packages with the same functionality are loaded, e.g., oligo and affy, then R will not be able to identify the package. Affy and oligo packages have functions with the similar name, e.g., `intensity()`, `MAplot()`, and `rma()`. If a function `intensity(data)` is called, then the package name should be specified before calling the function, e.g.,

```
oligo::intensity(data)
```

Otherwise, it chooses affy over oligo by default.

### 8.2.2 Importing the Raw Data for Processing

The working directory is specified using the `setwd` function, where all .CEL files are downloaded.

```
setwd("Path to the working directory")
```

To see the path of the working directory, the `getwd` function is used.

```
getwd()
```

The next step is to import .CEL files that contain raw probe-level data into `AffyBatch` object. `ReadAffy` function can be employed to import the data

```
data <- ReadAffy()
```

To see the loaded data, type `data` in the console

```
data
```

This function will give the information about the array size, affy IDs, annotation, and number of samples and genes.

```
AffyBatch object

size of arrays=1164x1164 features (20 kb)

cdf=HG-U133_Plus_2 (54675 affyids)

number of samples=6

number of genes=54675

annotation=hgu133plus2
```

### 8.2.3 Retrieving Sample Annotation of the Data

Microarray data sets not only include expression data but also comprise information of samples hybridized to the arrays, e.g., sample labels. `AffyBatch` objects contain many slots, and `phenoData` is one of these. It contains samples labels.

```
ph = data@phenoData
ph
```

This function will give information about phenotypic data and meta-data of the experiment.

An object of class ‘`AnnotatedDataFrame`’

`sampleNames`: Control 1.CEL Control 2.CEL ... Treated 3.CEL (6 total)

`varLabels`: sample

`varMetadata`: labelDescription

`ph` is a data frame and a column of a data frame can be selected using the $ sign. To see the number of samples in the experiment, use

```
ph$sample
```

[1] 1 2 3 4 5 6

And to observe the data in the data frame, use

```
ph@data
```

It will show the name and number of samples used in the experiment

sample

Control 1.CEL 1

Control 2.CEL 2

Control 3.CEL 3

Treated 1.CEL 4

Treated 2.CEL 5

Treated 3.CEL 6

To retrieve the probe IDs (Affymetrix IDs), `featureNames` function is used

```
featureNames(data)
```

[1] "1007_s_at" "1053_at" "117_at"

[4] "121_at" "1255_g_at" "1294_at"

[7] "1316_at" "1320_at" "1405_i_at"

[10] "1431_at" "1438_at" "1487_at"

[13] "1494_f_at" "1552256_a_at" "1552257_a_at"
