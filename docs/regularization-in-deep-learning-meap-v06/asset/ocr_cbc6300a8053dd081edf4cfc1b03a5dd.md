In the next section, we will entertain a more detailed analysis of the convergence property of SGD based on a univariate single-example linear regression model.

# 8.2 More on SGD convergence

Theoretical analysis on SGD requires each mini-batch to be taken with replacement; that is, a sample *can* be repeated in the next mini-batch. Let us use *t* to denote the *t*th iteration and *i(t)* as the index of the random sample used for SGD update of the *k*th iteration, using a single sample in each mini-batch. In each iteration, *i(t)*= {1, 2, ..., *N*} could be any sample within the training set. Our SGD update rule using a single observation at iteration *t* thus becomes:

**w**<sub>*t*+1</sub>=**w**<sub>*t*</sub> - \(\eta_t\) \(\nabla_w Q(\mathbf{w}_t, D_{i(t)})\)

Where *D*<sub>(*t*)</sub> represents the single observation currently sampled and *η<sub>t</sub>_ is the learning rate as a function of the time step. Processing a single observation makes the update *N_ times faster, where *N_ could easily be millions in modern applications. This idea, where a single random observation is used in each round of gradient update instead of the whole training set, was initially proposed by Robbins and Monro in their 1951 paper titled "A Stochastic Approximation Method" and remains the mainstream optimization method in modern neural networks today.

An interesting phenomenon observed in single-example SGD updates is that the descent is typically fast and stable at the initial stage, as indicated by a relatively smooth trajectory, followed by random oscillations within a specific region. Such fluctuations are a result of the stochasticity in the random mini-batches (single example) and a high level of sensitivity to the learning rate.

Figure 8.10 illustrates this behavior. On the left panel, we plot a typical trajectory using single-observation SGD. Note that the first four iterates, as indicated by the number 1-4, have relatively fast progress towards the local optimum on the right area, which confines the follow-up fluctuating updates. Suvrit Sra, who discussed this phenomenon in his YouTube tutorial on SGD, called the first region the far-out zone and the second region the confusion, indicating that the optimizer is now suffering from random fluctuations. We use the same name here.