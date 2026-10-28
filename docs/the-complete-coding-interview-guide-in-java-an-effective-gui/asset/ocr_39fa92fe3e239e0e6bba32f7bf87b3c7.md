# A-MATH: A Dataset for Mathematical Research Comprehension

Modern reinforcement learning systems have demonstrated remarkable performance in theorem proving and mathematical reasoning. However, this progress is hindered in the research mathematics domain, where problems are inherently complex and multi-step. To address these challenges, we introduce A-MATH, a dataset of mathematical research problems collected from mathematical competitions and AI research papers. Each problem is paired with a detailed solution written as a proof in a formal language.

We propose a novel two-stage framework for tackling these problems. The first stage uses a neural theorem prover to generate a proof sketch, and the second stage employs a formal verifier to check the correctness of each step. Our framework achieves a new state-of-the-art accuracy of 78.4% on the benchmark.

+   1. **Task formulation**: Given a mathematical statement, predict the proof in a formal language.
+   2. **Dataset**: Contains 1,200 problems from olympiad-style competitions and AI research papers.
+   3. **Evaluation**: We evaluate both step-wise accuracy and full-proof correctness.

| Model | Accuracy | Step-wise |
|-------|----------|-----------|
| Baseline (GPT-4) | 62.3% | 71.5% |
| Our Framework | 78.4% | 85.2% |

![](bbox=[0.05, 0.42, 0.45, 0.6])

As shown in Figure 1, the framework first generates a proof sketch using a neural theorem prover, which is then verified by a formal verifier. This separation of generation and verification improves reliability and interpretability.

> > "The combination of neural generation and formal verification is a key step towards scalable AI mathematicians."

We release our dataset, code, and models to facilitate research in this direction.