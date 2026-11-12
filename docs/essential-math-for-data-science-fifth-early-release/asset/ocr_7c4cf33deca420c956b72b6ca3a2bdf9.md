w5, and w6. The third row is the third node with weights w7, w8, and w9.

The output layer only has one node, meaning its matrix only has one row with weights w10, w11, and w12.

See a pattern here? Each node is represented as a row in a matrix. If there are 3 nodes there are 3 rows. If there is 1 node there is 1 row. Each column holds a weight value for that node.

Let’s look at the biases too. Since there is one bias per node, there’s going to be 3 rows of biases for the hidden layer, and 1 row of biases for the output layer. There’s only 1 bias per node so there will only be 1 column.

\( B_{hidden} = \begin{bmatrix} 0.41379442 \\ 0.81666079 \\ 0.07511252 \end{bmatrix} \)

\( B_{output} = [0.58018555] \)

Now let’s compare these matrix values to our visualized neural network as shown in Figure 7-9.