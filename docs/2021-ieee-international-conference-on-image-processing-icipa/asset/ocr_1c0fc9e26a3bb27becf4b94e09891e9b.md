# IDENTITY-FREE FACIAL EXPRESSION RECOGNITION USING CONDITIONAL GENERATIVE ADVERSARIAL NETWORK

Jie Cai1,2, Zibo Meng1,2, Ahmed Shehab Khan3,5, James O’Reilly3, Zhiyuan Li3, Shizhong Han4, Yan Tong3

1 InnoPeak Technology, 2 OPPO US Research Center, 3 University of South Carolina, 4 Qualcomm AI Research, 5 Facebook

# ABSTRACT

A novel Identity-Free conditional Generative Adversarial Network (IF-GAN) was proposed for Facial Expression Recognition (FER) to explicitly reduce high inter-subject variations caused by identity-related facial attributes, e.g., age, race, and gender. As part of an end-to-end system, a cGAN was designed to transform a given input facial expression to an “average” identity face with the same expression as the input. Then, identity-free FER is possible since the generated images have the same synthetic “average” identity and differ only in their displayed expressions. Experiments on four facial expression datasets, one with spontaneous expressions, show that IF-GAN outperforms the baseline CNN and achieves state-of-the-art performance for FER.

Index Terms— Facial Expression Recognition, Facial Expression Generation, Convolutional Neural Network, Generative Adversarial Network

# 1. INTRODUCTION

![](bbox=[0.07, 0.6, 0.58, 0.78])

Fig. 1: IF-GAN identity-free expressive face generation.

FER systems play an important role in human-computer interaction, e.g., animation and interactive games. CNNs have recently achieved good results on FER but the high inter-subject facial attribute variations were not explicitly considered and, as discussed in [1], the learned features may capture more identity-related information which is unrelated to the FER task. This motivates reducing identity-related variations by explicitly removing the identity information from the images. If f(I) is a CNN extracted facial representation of an input image I and is the result of a nonlinear function g(f_id(I), f_exp(I)) → f(I), where f_id(I) are identity-related features and f_exp(I) are expression-related features, then one may focus on f_exp(I) and minimize f_id(I) when finding f(I).

Fig. 1 illustrates the proposed IF-GAN removing identity features by transferring the expression features from an input image to a synthetic “average” face calculated from all subjects in a training dataset. The generated images have the same synthetic “average” identity, differing only in their displayed expressions, and thus can be used for identity-free expression classification.

In summary, our major contributions are:

+   - Identity-free FER using IF-GAN by transferring an arbitrary subject’s expression to an “average” identity;
+   - Developing an end-to-end system to perform expression synthesis and recognition simultaneously.

IF-GAN surpasses baseline CNN and state-of-the-art FER methods on four facial expression datasets, including the spontaneous facial expression dataset RAF-DB.

# 2. RELATED WORK

Recent surveys [2, 3] detail the research into FER over past decades. Deep CNNs have recently achieved good FER results [4, 5, 6, 7, 8, 9, 10, 11, 12, 1, 13, 14, 15, 16, 17, 18, 19, 20], but they may also learn identity-related features that are irrelevant to expression and suffer from high intra-class variations and inter-class similarities, leading to a drop in FER performance on unseen subjects. Wen et al. [21] introduced a center loss for face recognition to reduce intra-class variations, without explicitly considering inter-class similarity. Extensions of center loss [5, 7] were proposed to reduce the intra-class variations while increasing the inter-class differences. These approaches, however, are trained on distinct subjects without disentangling identity-related and expression-related features.

Identity-free facial expression recognition: A few recent approaches attempted to explicitly improve person-independent FER. An Identity-Aware CNN (IACNN) [4]