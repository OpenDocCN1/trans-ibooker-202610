
As children begin observing their environment and processing stimuli by sensing the world around them, the billions of neurons and trillions of synapses work together to learn and store information. This results in our ability to understand and comprehend information, recognize spatial and temporal patterns, have thoughts and make mental predictions, recall information and memories, drive motor actions based on recall and prediction, and continue to learn throughout our lifetime. This is what makes humans intelligent.^([2](#app01.html#idm45909201292744))

Let’s now discuss how humans have tried to mimic this natural phenomena with machines.

# An Introduction to ANNs

ANNs are one of the primary tools used to build AI applications, and they are being used in many powerful and exciting ways, many covered throughout this book.

ANNs are statistical models directly inspired by and partially modeled on biological neural networks. They are capable of modeling and processing nonlinear relationships between inputs and outputs in parallel. The related algorithms are part of the broader field of machine learning, and we can use them in many applications such as prediction, natural language processing, and pattern recognition.

ANNs are characterized by parameters that can be tuned by a learning algorithm (parametric learning) that learns from observed data in order to build an optimized model. Some of these parameters include weights along paths between neurons and also values referred to as bias. Hyperparameters (tunable model configuration values) such as the algorithm’s `learning rate` can also be tuned for optimal performance. When using an ANN, the practitioner must choose an appropriate learning algorithm and what is known as a `loss` (or `cost`) `function`.

The loss function is what’s used to learn the optimal parameter values for the problem being solved. Learning the optimal parameter values is usually done through optimization techniques such as `gradient descent`. These optimization techniques basically try to make the ANN solution get as close as possible to the optimal solution, which when successful means that the ANN is able to solve the intended problem with high performance; or, put another way, predictive accuracy.

At a very high level, gradient descent works by trying different combinations of parameter values in an algorithmic, strategic way in order to iterate to the optimal overall parameter combination, and thus model. Although a bit oversimplified, the algorithm is able to determine when it has approached the best possible parameter combination and will stop the iteration process as a result.

Architecturally, an ANN is modeled using layers of artificial neurons, or computational units able to receive input and apply an activation function along with a threshold to determine whether messages are passed along, similarly to biological neurons and neural network information-propagating mechanisms.

In a `shallow` ANN, the first layer is the input layer, followed by one hidden layer, and finally by an output layer. Each layer can contain one or more neurons. The term “hidden” is used to indicate that the layer is between the input and output layers, and that the layer transforms its own input values into input values for the next layer. The term also refers to the fact that hidden layer output values are neither easily interpreted nor explainable relative to the human understandable input and output values of the network.

[Figure A-4](#app01.html#a04) shows an example of a simple ANN.

![](assets/aipb_aa04.png)

ANN model architectures can be modified to solve a wider and more complex set of problems by adding hidden layers and/or neurons in any given layer, for example. Note that an increased chance of `overfitting` can also occur with increased model complexity, which means that the model performs well on training data, but not on new and unseen data (e.g., test data and/or real-world data).

The chain of transformations that occur from input to output is known as the credit assignment path, or CAP. The CAP value is a proxy for the measurement or concept of “depth” in a deep learning model architecture, and is the number of hidden layers plus the output layer. [Deep learning](http://bit.ly/2ICH3kx) generally refers to a neural network architecture that has a CAP greater than two; that is, more than two nonlinear transformation (hidden) layers.

Model architecture and tuning are therefore major components of ANN techniques, in addition to the actual learning algorithms themselves. All of these characteristics of an ANN can have significant impact on the performance of the model.

Additionally, ANNs are characterized and tunable by the activation function used to convert a neuron’s weighted input to its output activation. There are many different types of transformations that can be used as the activation function.

One thing worth noting is that although ANNs are extremely powerful, they can also be very complex and are considered black-box algorithms, which means that their inner workings are very difficult to interpret, understand, and explain. Choosing whether to employ ANNs and deep learning to solve problems should therefore be considered with that in mind.

# An Introduction to Deep Learning

Deep learning, although sounding flashy, is really just a term to describe neural networks with two or more hidden layers. Deep learning is further characterized by the consumption of raw input data and by different types of architectures and associated algorithms. These deep networks process data through many layers of nonlinear transformations in order to calculate a target output in the supervised case. Deep learning is able to do things with data that humans are unable to do using other techniques, and, as of this writing, is a very hot area of AI research and development.

More generally, deep learning represents a group of techniques known as `feature learning` or `representation learning`. Feature learning algorithms are able to learn features from raw data, or, stated another way, are algorithms that learn how to learn! This characteristic of deep learning is incredibly useful when it is extremely difficult or impossible for humans to select or engineer the appropriate features for a given application. The benefits of feature learning algorithms are realized most when learning from unstructured data such as images, video, audio, and language in the form of digital text. In these cases, deep learning algorithms automatically learn features and use them for a specific task such as image classification.

For deep learning neural networks, the number of layers are greater than in learning algorithms referred to as shallow. Shallow algorithms tend to be less complex and require more upfront knowledge of optimal features to use, which typically involves feature selection and engineering. In contrast, deep learning algorithms rely more on optimal model selection and optimization through model tuning. They are better suited to solve problems where prior knowledge of features is less desired or less necessary, and where labeled data is unavailable or not required for the primary use case.

Image recognition is a good example. It would be very difficult, if not impossible, for someone to write an explicitly programmed algorithm that tests all pixels, pixel groupings of varying sizes (areas) and locations, and color variations of an image to recognize and detect a cat. The reason is that the algorithm would need to know how to look for many different features of a cat in the image; for example, whiskers, pointy ears, and cat-like eyes. These features differ in many ways such as size and shape depending on the type and age of the cat.
