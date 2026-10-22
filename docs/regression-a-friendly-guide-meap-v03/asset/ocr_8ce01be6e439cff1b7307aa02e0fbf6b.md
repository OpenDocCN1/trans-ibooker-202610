better classifier for such an important problem. More on this in later chapters!

# 4.5 Exercises

+   1. For the NAMCS logistic model with ungrouped responses:
+      1. Compute the likelihood ratio and conduct the likelihood ratio test.
+      2. Compute and compare $R_M^2$, $R_S^2$, and $R_D^2$.
+      3. Look at the histograms related to the coefficient of discrimination.
+   2. For the simple logistic model for chasmogamous bud development:
+      construct a binary classifier, compute its error and success rates, plot the associated ROC curve, and determine its AUC.
+   3. Use the violet data to build and assess a logistic model that relates the probability of *cleistogamous* bud formation to photoperiod length.
+   4. For Collett’s throat data:
+      1. Construct and analyze a binary classifier.
+      2. Construct a model that excludes the surgery that lasted 135 minutes, and compare it to the model that includes this outlier.
+   5. Sources for some interesting exercises:
+      1. NHANES
+      2. heart failure example from UCI-ML repository
+      3. geohazard risk example (e.g., sinkholes in Florida, landslides in Mexico)

# 4.6 Summary

+   - Simple logistic regression uses the logistic curve to relate the probability of success (outcome=1) to the value of the predictor variable.
+   - Logistic models can be expressed in terms of probabilities, odds, or the logarithm of the odds; all three formulations are useful.
+   - To see if a logistic model is appropriate for a small dataset, plot the raw binary responses and look for a natural separation between successes (1s) and failures (0s).