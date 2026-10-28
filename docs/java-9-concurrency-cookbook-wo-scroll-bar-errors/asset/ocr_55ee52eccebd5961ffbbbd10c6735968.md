The *combiner* function receives two objects of the return type to calculate a new object of the return type. The *identity* value is the *identity* value of the return type, and it has to verify the following expression:

combiner (u, accumulator(identity, t)) == accumulator(u, t)

Here, u is an object of the return type and t an element of the stream.

The following screenshot shows the output of an execution of the example:

![](bbox=[0.26, 0.34, 0.73, 0.83])