fundamental for all kinds of regression analysis, not just linear regression. The detailed formulas are important in their own way, but understanding the main ideas related to inference is really the main goal. We’ll revisit and rely on these ideas again and again in later chapters.

# 3.1 Random variables and the theoretical model

When we fit a line to a scatterplot, we’re really acting on the belief that the response variable Y and the predictor variable x satisfy the theoretical model

y = α + β x + ε ,

for an intercept α and a slope β whose exact values can never be known and an unpredictable term ε that manifests as predictive uncertainty. The value of Y that we observe for a given value of x is the sum of the deterministic term α + β x and the random error term ε, making the response a random variable. To emphasize this fact, we henceforth use y to denote responses that have actually been observed, and we use Y to denote the random variable corresponding to responses that have not yet been observed. The theoretical model should therefore really be phrased in the form

Y = α + β x + ε .

From now on, we will generally use capital letters to denote random variables; their lowercase counterparts will denote observations or quantities computed from data. The reasons for adopting this notation will become clear as we go.

Since the error ε is random, we can’t predict how much it will contribute to an individual observation. We need to be able to say something, though, so we’re going to make some statistical assumptions about this term. To motivate the first one, fix a value x of the predictor variable and use the theoretical model to compute the average of all of the possible corresponding values of the response variable— this calculation is itself purely theoretical, as we can’t actually observe all possible responses to a given predictor value. What we obtain is the expected value or mean response of the random variable Y for the given value x, denoted E(Y|x) and read aloud as "the expected value of Y given x". Similarly, the average over all of these