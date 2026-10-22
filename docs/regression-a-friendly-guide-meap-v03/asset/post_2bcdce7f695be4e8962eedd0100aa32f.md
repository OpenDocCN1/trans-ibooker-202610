
- How does the number of traffic fatalities vary from week to week?
- What is the likely number of bug reports to be submitted tomorrow?

As you’ll see in the upcoming chapters, regression methods can be used to answer these questions and many others. While the kind of data we need and the analysis we have to do will depend on the specifics of the problem at hand, these questions are all fundamentally the same. In each case, we want to know how one quantity, the *response variable*, depends on another, an *explanatory variable* known as a *predictor*. A good regression model clarifies the relationship between these variables and allows forecasting the responses to certain values of the predictor. The trick, of course, is obtaining a *good* regression model that performs these jobs well!

The first step in building a regression model is identifying the appropriate method for the question of interest. When the response variable is *quantitative*— a test score, a literacy rate, or some other measurement— *linear regression* is the basic tool. When the response variable is *binary*— often interpreted as either success or failure— *logistic regression* is the go-to method; logistic models yield probabilities that can either be used directly or processed for classification. When the response variable is a *count*— a non-negative integer like the number of fatalities or bug reports— then some form of *count regression* is the place to start. Here is how all of this fits together for the questions above:

## Table 1.1. Response variables, predictors, and regression methods

| Relationship | Predictor | Response | Regression method |
|---|---|---|---|
| Adult literacy and infant mortality | National adult literacy rate | National infant mortality rate | Linear regression |
| College enrollments and birth trends | Annual number of births | Undergraduate enrollment 20 years later | Linear regression |
| Chronic conditions and age | Age in years | Probability of having a chronic condition | Logistic regression |
| Extinction risk and body size | Species body mass in kilograms | Chance of being endangered | Logistic regression |
| Traffic fatalities by week | Week of the year | Number of traffic fatalities | Count regression |
| Bug submissions by day | Day of the week | Number of bug reports | Count regression |

The structural similarity of these methods goes even deeper, providing a unity that can be obfuscated by messy formulas. Prediction is a common application of regression, and, thanks to experience and common sense, we all know an excellent prediction strategy: guess the average value, based on the information available. Suppose, for example, that you’re walking around a busy city by yourself. How tall will the next adult you see be? Guess the average height of an adult. How tall will the next adult woman you see be? Guess the average height of an adult woman. How tall will the next elderly woman you see be? Guess the average height of an elderly woman. And so on. While the accuracy of a prediction depends on the quality of the information used to make it, the underlying principle remains the same: the average value is often the best possible guess. All of the regression models in this book are implementations of this basic idea— keep this in mind as we go!

To make this explicit, here is the first linear regression model in the book, presented in detail at the start of the next chapter:

```
Average height of sons = 33.8866 + 0.5141 * Father's height.
```

For now, just know that this gives us the average height, in inches, of all of the sons of fathers of a given height; if a man is 70.21 inches tall, for instance, this model predicts his son’s height to be

```
Predicted height of son = 33.8866+0.5141*70.21 = 69.98 inches.
```

His son probably won’t be exactly this tall, but this is the best guess we can make based on the data used to build the model; as we’ll see later, the model also provides information on how far off our guess is likely to be. In any case, this example clearly illustrates the motif that runs throughout regression modeling, namely, *directly relating the average response and the predictor*. It will take a little more work to explain how this manifests in logistic regression and in regression models for counts, but we’ll get there in due time.

Now that you have some idea of what regression is, you may well be wondering something else: shouldn’t a useful model include more than one explanatory variable? This is another good question, and the answer is: *maybe*. In lots of problems, there are many possible predictors, and determining which ones to include is part of the modeling process— a tricky part, in fact! It therefore makes sense to start with *simple regression models*, those involving just one predictor, in Part 1 of the book, before moving on to *multiple regression models*, those involving two or more predictors, in Part 3. Understanding simple and multiple regression will pave the way for the more advanced material in Part 4.

## 1.2 Do you need a regression model?

Regression models can be crucial to clarifying relationships in data and to answering certain questions. They are by no means the right tools for every job, however, so understanding when regression models are likely to be helpful is important. Roughly, there are two situations in which you should reach for regression: when you want to summarize some overall trends to get to know your data better, or when you have to rely on a statistical model to draw insight from a limited amount of data— and if you need to do both of these, you definitely need regression!

When there’s an abundance of data related to the problems being studied, approaches other than regression will likely perform better and should be tried first. To explain why, let’s consider a couple of Big Data situations. First, suppose that we have a nice, clean dataset with thousands and thousands of rows and just a few columns. With all of this data at our disposal, we don’t need to figure out an appropriate statistical model and fit it to the data— we can work directly with summary statistics and carefully explore how they depend on the available predictors. We might have an extensive database of information about software developers and their salaries, for example; we could organize the developers by experience and industry and then compute the average salary for each combination, thereby discovering how the average response depends on the predictors. We wouldn’t need to assume a particular functional form for the relationship between the response and the predictors, nor would we be restricted to working with the usual average— it might make more sense to investigate how median salaries depend on experience and industry, and working with the median would be no harder than working with the mean. If you can get plenty of data, start with a simple exploratory approach like this before doing something complicated.

Having lots of data can be great, but it’s possible to have way more data than you can handle— this is what happens in a true Big Data problem, when there are, say, millions and millions of rows and hundreds or thousands of possible explanatory variables. Regression might not help much in this situation, since there’s too much data for people to process in anything but a haphazard way. This is when machine learning methods like dimension reduction and deep learning shine, as algorithms can wrestle signals from the noise systematically, and models have access to the generous training sets that they need.

Big Data gets a lot of attention these days, for some very good reasons: the technical problems are interesting and challenging, and the people solving these problems get paid handsomely by some of the most influential tech companies in the world. Most of the people engaged in data analysis, however, are not swamped with terabytes of data that can only be digested by machine learning algorithms: social scientists routinely conduct surveys of at most a few thousand people, trials of new medical procedures typically involve hundreds or thousands of patients, and many successful businesses
