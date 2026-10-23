# Chapter 7
Scalable Knowledge Graph Processing Using SANSA

Hajira Jabeen¹, Damien Graux³, and Gezim Sejdiu²,⁴

¹ CEPLAS, Botanical Institute, University of Cologne, Cologne, Germany
hajira.jabeen@uni-koeln.de
² Smart Data Analytics, University of Bonn, Bonn, Germany
³ ADAPT SFI Research Centre,
Trinity College, Dublin, Ireland
⁴ Deutsche Post DHL Group, Bonn, Germany

Abstract. The size and number of knowledge graphs have increased tremendously in recent years. In the meantime, the distributed data processing technologies have also advanced to deal with big data and large scale knowledge graphs. This chapter introduces Scalable Semantic Analytics Stack (SANSA), that addresses the challenge of dealing with large scale RDF data and provides a unified framework for applications like link prediction, knowledge base completion, querying, and reasoning. We discuss the motivation, background and the architecture of SANSA. SANSA is built using general-purpose processing engines Apache Spark and Apache Flink. After reading this chapter, the reader should have an understanding of the different layers and corresponding APIs available to handle Knowledge Graphs at scale using SANSA.

# 1 Introduction

Over the past decade, vast amounts of machine-readable structured information have become available through the increasing popularity of semantic knowledge graphs using semantic technologies in a variety of application domains including general knowledge [25, 448], life sciences [468], scholarly data [129], open source projects [266], patents [261] or even food recommendations [189]. These knowledge bases are becoming more prevalent and this trend can be expected to continue in future.

The size of knowledge graphs has reached the scale where centralised analytical approaches have become either infeasible or too expensive. Recent technological progress has enabled powerful distributed in-memory analytics that have been shown to work well on simple data structures. However, the application of such distributed analytics approaches on semantic knowledge graphs is lagging behind significantly. To advance both the scalability and accuracy of large-scale knowledge graph analytics to a new level, fundamental research on