would it be? Machine Learning is an incredibly performance-oriented activity with monumental computational workloads these days.

Languages such as Python are much more strongly-associated with machine learning than JavaScript is. If we are being honest, that is a bit of a stretch too. Python is a horrible numerics language on its own. It strikes a nice balance, however, of readability, flexible programming style (functional, OO, procedural) and wide-ranging coverage of algorithms, visualization and data-wrangling features. If you can make it run faster, then it checks an awful lot of boxes. Numpy, native libraries, clustered cloud environments and more can give it the computational horsepower to make the training process happen.

Most organizations do not run Python operationally though so chances are you will need to serialize the model into some other format so that it can be loaded into a C/C++, C#, Java or JavaScript application. This is becoming easier because of open formats such as Open Neural Network Exchange (ONNX)¹, but many frameworks such as Tensorflow naturally support this form of mixed use.

The first step is to train our models. The second step is for inference. Until a few years ago, Tensorflow required you to train in Python, but you could save the model out to disk and load it into other environments. These days you have the choice of training in Python, JavaScript or Swift with Tensorflow.

We are going to discuss Tensorflow.js which is designed as a way of providing machine learning capabilities in and out of the browser in JavaScript environments. Using JavaScript for machine learning (particularly in the browser) makes more sense than what you might think. In order to understand the relationship between machine learning and JavaScript and WebAssembly, however, we must first have a quick discussion about hardware and how it is changing our industry.

# Hardware