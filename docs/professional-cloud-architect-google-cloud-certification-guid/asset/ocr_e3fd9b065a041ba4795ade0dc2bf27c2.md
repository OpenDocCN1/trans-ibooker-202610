# Cloud Vertex AI

Vertex AI is a Google Cloud AI platform service that allows you to build, deploy and scale ML models with a pretrained or custom tool without worrying about the underlying infrastructure. It is an evolution of the AI Platform service and introduced integration with AutoML.

> **Exam Tip**
> Vertex AI was announced at the Google I/O conference in 2021. Lots of Vertex AI features are still in preview at the time of writing this book. We don’t anticipate that you will get a question on this in the PCA exam. We still feel this topic is very exciting. If you want to learn more about Vertex AI, check the documentation: https://cloud.google.com/vertex-ai/docs/start.

You can accelerate the learning process, since a range of CPU, GPU, and **Tensor Processing Unit (TPU)** nodes are supported. It works with multiple frameworks, but the most popular is TensorFlow. As TensorFlow is open source, it allows for portability. Models can be trained locally on limited data and then sent to GCP to train at scale. Vertex AI integrates with other GCP services, such as Cloud Storage for data storage and Cloud Dataflow for data processing.

# Using Vertex AI

The following diagram should give you an idea of where Vertex AI fits into the process of developing, deploying, and monitoring your ML model:

![](bbox=[0.12, 0.62, 0.88, 0.78])

Figure 14.7 – Vertex AI workflow