![](bbox=[0.12, 0.09, 0.88, 0.48])

Figure 5-5. Neural network example

Developing a neural network’s structure is a difficult and expensive design process. To minimize this expense in time and effort, organizations often tailor existing neural networks for a specific application by starting with the structural information and regenerating weighting information from different training data. This process, called transfer learning, is the most approachable way for many organizations to use pretrained neural networks and leverage the extensive training previously executed for similar applications.

As previously mentioned, models are updated in one of several ways, depending on the learning technique. In some use cases, it is not desirable to reload all of the training data that were initially used to create a model to update a model. In this situation, an online learning technique can be used to allow training data to be incrementally added to the model without having to retrain from scratch.

An even more extreme version of this is distributed/federated learning. Recent developments in so-called “edge” technologies, such as widely used