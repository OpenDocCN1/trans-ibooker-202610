![](bbox=[0.14, 0.08, 0.85, 0.56])

Figure 9-1. IGV visualization of the HG00103 at chr7:117149189 in the CFTR gene

It is now possible to manually inspect these nine reads, or process them through a custom aligner, for example, and check whether the reported pathogenic variant is a false positive.

Say we’re running a clinical lab that is performing such carrier screening as a service to clinicians. Archiving the raw data using a cloud storage system such as AWS S3 ensures that the data stays relatively warm (compared with, say, tape archive). In addition to having a reliable system for actually performing the data processing, we can easily access all of the past data for quality control (QC) or for cases where there need to be manual interventions, like the CFTR example presented earlier. In addition to the rapid access to the totality of the data, the centrality also makes it easy to