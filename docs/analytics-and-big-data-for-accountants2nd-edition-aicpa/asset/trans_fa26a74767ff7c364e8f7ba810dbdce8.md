
# Application of Data Analytics at the SEC: From Classic Statistical Modeling to Machine Learning

## Why These Perspectives Matter?

I have worked at the Commission for nearly a decade, during which I participated in research on a wide range of policy issues. The economic analyses supporting these policy decisions were primarily built upon theory-driven research by social scientists. They rely on carefully constructed analyses to address causal inference problems, which is critical for understanding the potential impact of new regulations.

## Recent Changes

But over the past few years, I have witnessed the arrival of increasingly complex data and new analytical methods for analyzing such data. Some of these analytical methods have made previously impenetrable information sets (e.g., unstructured data, such as free text) amenable to analysis. This is particularly important for the SEC, as registrant filings are often presented in the form of narrative disclosures. Consequently, we have launched a series of new initiatives utilizing machine learning methods for behavioral prediction, particularly in the area of market risk assessment, including identifying potential fraud and misconduct.

## Adopting New Methodologies While Maintaining Core Principles

Today, the SEC, like many other organizations, is adopting these new methodologies at a very rapid pace. Of course, this does not mean we are abandoning classic statistical modeling. As I want to emphasize now, regardless of whether our analytical projects are built on classic statistical modeling or machine learning, **neither can replace human judgment**, and human judgment remains crucial to making the output of our analytical models and methods actionable. To illustrate why, let me give a few examples.

## Classic Statistical Modeling: Corporate Issuer Risk Assessment (CIRA)

Let's start with the Corporate Issuer Risk Assessment (CIRA) project, which relies on classic statistical modeling developed by DERA economists and accountants in collaboration with expert staff from the SEC's Division of Enforcement. The project originated from an initiative initially known as the "Accounting Quality Model" (AQM), which itself was rooted in academic research. Specifically, the AQM focused on estimating earnings quality and identifying signs of management's improper discretion in the use of accruals. As former DERA Director and SEC Chief Economist Craig Lewis noted: "The finance and accounting academic community has long studied the information contained in financial statements to better understand the discretionary accounting choices made when reporting financial information to shareholders."[^iv]

Today, the CIRA project provides these earnings quality modeling metrics as part of more than 230 custom metrics available to SEC staff. These metrics include earnings smoothing measures, auditor activity measures, tax treatment measures, key financial ratios, and management behavior indicators. Importantly, they are easily accessible to SEC staff through intuitive, customized dashboards. Citing the collaboration between DERA and the Enforcement Division's Fraud Group, Enforcement Director Andrew Ceresney noted earlier this year: "CIRA provides us with a comprehensive overview of the financial reporting environment of the Commission's registrants and assists our staff in detecting anomalous patterns in financial statements that may warrant further investigation."[^v]

## Clarifying the "Robocop" Misconception

However, the media initially coined the term "Robocop" to describe the initiative when reporting on it—as if a machine were making the critical decisions to identify potential market risks. As our current DERA Director and Chief Economist Mark Flannery recently pointed out: "The implied view is, at best, inaccurate and, at worst, misleading. While these activities use quantitative analysis to help prioritize limited agency resources, the tools developed in DERA cannot—and do not—operate independently."[^vi]

## Machine Learning and Text Analysis: The Most Exciting Developments

But at the same time, some of the most exciting developments at the Commission are focused on machine learning and text analysis. While machine learning methods have existed since the 1950s[^vii], the advent of big data and high-performance computing environments has advanced their application. At the Commission, this has taken several forms.

At the most basic level, consistent with approaches now common in academic research, we extract words and phrases from tabular and narrative disclosures in filings. For example, by applying a programming technique that uses human-written rules to define document patterns—known as "regular expressions"[^viii]—we have been able to systematically measure and assess how emerging growth companies utilize JOBS Act provisions through disclosures in their registration statements.

## Topic Modeling and Sentiment Analysis

More recently, we have adopted topic modeling[^ix] methods to analyze the tens of thousands of narrative disclosures contained in registrant filings. For those unfamiliar with topic modeling, when applied to a document corpus, it identifies groups of words and phrases associated with different concepts ("topics") across all documents, and simultaneously generates the distribution of topics found in each specific document. We also use natural language processing techniques to perform sentiment analysis to assess the tone of each filing[^x] (e.g., identifying filings with negative tone or vague tone). We then use machine learning algorithms to map these topic and tone "signals" to known risk measures—such as examination findings or past enforcement actions. Once trained, the final model can be applied to new documents filed by registrants, assigning risk scores based on historical findings across all filers. This process can be applied to different types of disclosures or unique registrant categories, and the results are then used to help us determine where investigative and examination staff should prioritize their attention.

## Limitations of Machine Learning: The Human Element Remains Indispensable

However, while this machine learning text analysis approach provides new and exciting ways to detect potential market misconduct, just like classic modeling methods, **it cannot operate independently**. In particular, while a model may classify a filing as high risk, that classification does not provide a definitive indicator of underlying misconduct. Rather, many machine learning methods typically do not point to specific actions or behaviors indicative of fraud or other violations. **The human element remains an essential part of the equation.**[^11]

## References

The Roman numeral references included in the speech are as follows:

[^iv]: Craig Lewis, Chief Economist and Director, Division of Risk, Strategy, and Financial Innovation, U.S. Securities & Exchange Commission, Financial Executives International Committee on Finance and Information Technology, Dec. 13, 2012.

[^v]: Andrew Ceresney, Director, Division of Enforcement, U.S. Securities & Exchange Commission, Directors Forum 2016 Keynote Address.

[^vi]: Mark Flannery, Director, Division of Economic and Risk Analysis, U.S. Securities & Exchange Commission, Global Association of Risk Professionals Risk Assessment Keynote Address.

[^vii]: See Arthur L. Samuel, Some Studies in Machine Learning Using the Game of Checkers, IBM Journal, Vol. 3, No. 3, July 1959.

[^viii]: Thompson, K. (1968). "Programming Techniques: Regular expression search algorithm." Communications of the ACM. 11 (6): 419-422. doi:10.1145/363347.363387.

[^ix]: See, for example, David Blei, "Probabilistic Topic Models," Communications of the ACM. 55, April 2012.

[^x]: See, e.g., Tim Loughran and Bill McDonald, 2011, "When is a Liability not a Liability? Textual Analysis, Dictionaries, and 10-Ks," Journal of Finance, 66:1, 35-65.

[^11]: Footnote cited from original text

