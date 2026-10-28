Estimation can be thought of as a procedure made up of three primary parts:

+   - Criterion function
+   - Models
+   - Algorithm.

The criterion function can take many forms and can also be classified as deterministic or stochastic. Models represent a broad class of information, formalizing the a priori knowledge about the process generating the signal, measurement instrumentation, noise characterization, and underlying probabilistic structure. Finally, the algorithm or technique chosen to minimize (or maximize) the criterion can take many different forms depending on (i) the models, (ii) the criterion, and (iii) the choice of solution. For example, one may choose to solve the well-known least-squares problem recursively or with a numerical-optimization algorithm. Another important aspect of most estimation algorithms is that they provide a “measure of quality” of the estimator. Usually, what this means is that the estimator also predicts vital statistical information about how well it is performing.

Intuitively, we can think of the estimation procedure as follows:

+   - The specification of a criterion
+   - The selection of models from a priori knowledge
+   - The development and implementation of an algorithm.

Criterion functions are usually selected on the basis of information that is meaningful about the process or the ease with which an estimator can be developed. Criterion functions that are useful in estimation can be classified as deterministic and probabilistic. Some typical functions are as follows:

+   - *Deterministic:*
+     - Squared error
+     - Absolute error
+     - Integral absolute error
+     - Integral squared error
+   - *Probabilistic:*
+     - Maximum likelihood
+     - Maximum a posteriori (Bayesian)
+     - Maximum entropy
+     - Minimum (error) variance.

Models can also be deterministic as well as probabilistic; however, here we prefer to limit their basis to knowledge of the process phenomenology (physics) and the underlying probability density functions as well as the necessary statistics to describe the functions. Phenomenological models fall into the usual