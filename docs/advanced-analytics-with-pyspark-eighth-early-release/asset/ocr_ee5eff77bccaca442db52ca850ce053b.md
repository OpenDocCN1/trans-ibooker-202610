![](bbox=[0.145, 0.105, 0.845, 0.605])

*Figure 4-3. Robot's next decision tree*

Now, seven of nine examples are correct. Of course, decision rules could be added until all nine were correctly predicted. The logic embodied in the resulting decision tree would probably sound implausible when translated into common speech: “If the animal’s weight is less than 100kg, and its color is brown instead of green, and it has fewer than 10 legs, then yes it is a suitable pet.” While perfectly fitting the given examples, a decision tree like this would fail to predict that a small, brown, four-legged wolverine is not a suitable pet. Some balance is needed to avoid this phenomenon, known as overfitting.

Decision trees generalize into a more powerful algorithm, called random forests. Random forests combine many decision trees in order to reduce the risk