was proposed to alleviate identity-related variations using expression-sensitive and identity-sensitive contrastive losses. Drastic data expansion with contrastive loss occurs when constructing image pairs for training. Identity-Adaptive Generation (IA-gen) [1] was developed using six cGANs to generate six expressions from any input facial image. Then, FER is performed by comparing the six output images to the input. In De-expression Residue Learning (DeRL) [12], a cGAN synthesized a neutral, same-identity face image from a given expressive image. Person-independent expression information was extracted from the intermediate layers of the generative model. A cGAN was utilized in [22] to transform an input face image I to an expression-preserving face image Ĩ of a different identity. The generator produced an identity-invariant representation f(I), which was employed for FER. Unlike these cGAN-based models [1, 12, 22], the proposed IF-GAN is an end-to-end network that directly removes the identity information and generates a face image for identity-free FER.

# 3. METHODOLOGY

![](bbox=[0.08, 0.44, 0.48, 0.58])

Fig. 2: The IF-GAN architecture consists of: (1) a “U-Net” [23] generator (G), (2) a “PatchGAN” [24] discriminator (D) that only penalizes structures at the scale of patches, and (3) a ResNet-101 [25] expression classifier (E), pre-trained on ImageNet. In contrast to DeRL [12] and IA-gen [1], G, D, and E are jointly optimized during the IF-GAN training. Note that only G and E are employed during testing.

# 3.1. An Overview of the Proposed IF-GAN

IF-GAN transfers expression from an input image to a synthetic “average” identity and then generates an “average” expressive image for identify-free expression classification.

Let I_S,E be a face image of any real subject, with any expression, and let I_A,E be the “Average Expression” image calculated from all subjects with the same expression. The generator (G) learns to transfer expression information f_exp(I_S,E) from input I_S,E to an “average” subject and generates an image Ĩ_A,E with the same expression as I_S,E. In contrast to regular GANs and cGANs, an expression classifier (E) is employed in IF-GAN to ensure Ĩ_A,E has the same expression as the input image I_S,E.

![](bbox=[0.52, 0.08, 0.88, 0.32])

Fig. 3: U-Net-based generator with self-attention modules. Purple boxes correspond to multi-channel convolution feature maps; lilac boxes are feature maps copied from the encoding path, where the number of channels is above each box and the spatial size of feature maps are on the box’s lower left corner. Gray dashed arrows denote skip connection operations. Note the three self-attention modules: one on a skip connection and another two on the encoding and decoding paths. Best viewed in color.

As shown in Fig. 2, the IF-GAN has a generator (G), a discriminator (D), and an expression classifier (E). G takes an input subject expressive image I_S,E and generates an expressive “average” subject face image I_A,E. Then, the fake tuple {I_S,E, Ĩ_A,E, 0} and the real tuple {I_S,E, I_A,E, 1} are fed into the discriminator for fake/real classification. Finally, {I_S,E, I_A,E, l_e} and {I_S,E, Ĩ_A,E, l_e}, with the expression label l_e, are utilized to fine-tune the expression classifier.

![](bbox=[0.52, 0.62, 0.92, 0.7])

Fig. 4: Illustration of the self-attention module. Input feature maps are shown with their tensor shapes, e.g., C1×W×H for C1 channels of W×H feature maps. Green/red boxes are 1×1 convolution channel reductions resulting in C2 channels. Magenta boxes are 1×1 convolutions with C1 channel results. Softmax, performed on each row, normalizes the attention coefficients. Trainable parameter (γ) scales self-attention feature maps. Best viewed in color.

Furthermore, self-attention modules are added on top of our generator (Fig. 3) to efficiently capture spatially distant relationships and generate better expressive images. Fig. 4 shows a self-attention module computing a response at a position as a weighted sum of the features at all positions in the input feature maps.