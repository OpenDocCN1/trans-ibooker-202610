be surprisingly efficient once trained: even with the full GPT-3 175B, generating 100 pages of content from a trained model can cost on the order of 0.4 kW/hr, or only a few cents in energy costs. Additionally, because GPT-3 exhibits few-shot generalization, it doesn’t need to be retrained for every new task like smaller models do. A 2019 Green AI paper in the journal CACM notes “that the trend of releasing pre-trained models publicly is a green success,” and the authors encouraged organizations “to continue to release their models in order to save others the costs of retraining them.”

A few more strategies have emerged to reduce LLMs’ impact on the planet. As Patterson et al. point out, “Remarkably, the choice of DNN, datacenter, and processor can reduce the carbon footprint up to ~100-1000X”. Algorithmic techniques can also improve energy efficiency. Some work by achieving the same accuracy with less overall computation. Other techniques use a large, already-trained model as a starting point to yield a lighter-weight, more computationally efficient model with almost the same accuracy. Additional techniques include:

# Distillation

This technique transfers the knowledge from large models into smaller, more computationally efficient models

# Pruning, quantization, and efficient coding

These techniques were found to improve the energy efficiency of DNNs 3X-7X.

# Fine-tuning and transfer learning

They both reuse already-trained representations.

# Sparsely activated mixture-of-expert-style models

These techniques provide more than 10X reductions in computation requirements and energy costs.