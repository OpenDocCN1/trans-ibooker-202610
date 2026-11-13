
In addition, different bias-handling algorithms address different parts of the machine learning life cycle; understanding how, when, and why to use each research contribution is challenging even for experts in algorithmic fairness. As a result, AI stakeholders and practitioners need clarity on how to proceed. Currently the burden is on the practitioners, who face questions such as these:

*   Should our data be debiased?
*   Should we create new classifiers that learn unbiased models?
*   Is it better to correct predictions from the model?

This report helps you to strategize on how to approach such questions for your organization’s specific use case.

# Where Does Bias Come From?

Algorithmic bias is often discussed in machine learning, but in most cases the underlying data, rather than the algorithm, is the main source of bias. Consider supervised learning, which is the most common form of machine learning. Its goal is to find a mathematical function that takes data points of numerical, ordinal, or categorical features as inputs and predicts correct labels for those data points. Models are trained on data, and often that data contains human decisions that reflect the effects of societal or historical inequities.

For example, in a credit scoring model, the features might be income, education level, and occupation of an applicant, and the label might be whether an applicant defaults three years later. The algorithm finds the desired function by training on a large set of already labeled examples. Although the function fits the training data, when it is applied to new data points, it must generalize to predict well. In its most basic form, fitting is performed to optimize a criterion such as average accuracy.

The biggest problem with machine learning models is that the training distribution does not always match the desired distribution. If the present reality puts certain individuals at a systematic disadvantage, the training data distribution is likely to reproduce that disadvantage rather than reflecting a fairer future. Biases such as those against African-Americans in the criminal justice system and women in employment can be present whenever judges and hiring managers make decisions. These decisions are reflected in the training data and subsequently baked into future machine learning model decisions. Some types of bias that can be introduced into data include the following:

Sample bias
: Sample bias occurs when one population is overrepresented or underrepresented in a training dataset. An example of this would be a recruiting tool that has been predominantly trained on white male job candidates.

Label bias
: Label bias occurs when the annotation process introduces bias during the creation of training data. For example, the people labeling the data might not represent a diverse group of locations, ethnicities, languages, ages, and genders, and can bring their implicit personal biases into their labels. This can lead to labels that are skewed in ways that yield systematic disadvantages to certain groups.

Outcome proxy bias
: Outcome proxy bias occurs when the machine learning task is not specified appropriately. For example, if one would like to predict the likelihood of a person committing a crime, using arrests as a proxy is biased because arrest rates are greater in neighborhoods with more police patrols. Also, being arrested does not imply guilt. Similarly, using the cost of a person to a health system is a biased proxy for the person’s quality of health.

# Bias and Machine Learning

Machine learning models make predictions of an outcome for a particular instance. For example, given an instance of a loan application, a model might predict whether an applicant will repay the loan. The model makes these predictions based on a training dataset for which many other instances (other loan applications) and actual outcomes (whether the borrowers repaid) are provided. Thus, a machine learning algorithm will attempt to find patterns, or *generalizations*, in the training dataset to use when a prediction for a new instance is needed. For example, one pattern might be that if a person has a salary greater than $40,000 and outstanding debt less than $5,000, they will repay the loan. In many domains, this technique, called *supervised machine learning*, has worked very well.

However, sometimes the patterns such models find are undesirable or even illegal. For example, our loan repayment model might determine that age plays a significant role in the prediction of repayment, because the training dataset happened to have better repayment rates for one age group than for another. This raises two problems. First, the training dataset might not be representative of the true population of people of all age groups. Second, even if it is representative, it is illegal to base a decision on an applicant’s age, regardless of whether this is a good prediction based on historical data.

AIF 360 addresses this problem with *fairness metrics* and *bias mitigators*. We can use fairness metrics to check for bias in machine learning workflows. We can use bias mitigators to overcome bias in the workflow to produce a fairer outcome. The loan scenario describes an intuitive example of illegal bias. However, not all undesirable bias in machine learning is illegal; bias can also manifest in subtler ways. For example, a loan company might want a diverse portfolio of customers across all income levels and thus will deem it undesirable to make more loans to high-income customers than to low-income customers. Although doing so is not illegal or unethical, it is undesirable for the company’s strategy.

# Can’t I Just Remove Protected Attributes?

When building machine learning models, many data scientists assume that they can just remove protected attributes (i.e., race, gender, age) to avoid unfair bias. However, there are many features that are too closely correlated to protected attributes, which makes it easy to reconstruct a protected attribute such as race even if you drop it from your training set.

An example of this is when Amazon rolled out its Prime Free Same-Day Delivery service for many products on orders over $35. Eleven months after rolling out the program, Amazon offered same-day service in 27 metropolitan areas. However, a [2016 Bloomberg News analysis](https://oreil.ly/6_D_s) found that six of the major cities serviced were excluding predominantly black zip codes to varying degrees. And in Atlanta, Chicago, Dallas, Washington, Boston, and New York, black citizens were about half as likely as white residents to live in neighborhoods with access to Amazon same-day delivery. How did this occur? Amazon focused its same-day service model on zip codes with a high concentration of Prime members, not on race. Yet in cities where most of those paying members are concentrated in predominantly white parts of town, looking at numbers instead of people did not prevent a data-driven calculation from reinforcing long-entrenched inequality in access to retail services.

# Conclusion

Bias mitigation is not a silver bullet. Fairness is a multifaceted, context-dependent social construct that defies simple definition. The metrics and algorithms in AIF 360 can be viewed through the lens of [distributive justice](https://oreil.ly/ZDm9O) and do not capture the full scope of fairness in all situations. The toolkit should be used in only a very limited setting: allocation or risk assessment problems with well-defined protected attributes in which one would like to have some sort of statistical or mathematical notion of sameness. Even then, the code and collateral contained in AIF 360 are only a starting point for a broader discussion among multiple stakeholders on overall decision-making workflows.
