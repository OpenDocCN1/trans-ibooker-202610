
## Semantic Web for Effective Healthcare

- 51. McClure, R.J. and Mack, K., Injury surveillance as a distributed system of systems. *Inj. Prev.*, 22 Suppl 1, i1–i2, 2016.
- 52. Denecke, K., An ethical assessment model for digital disease detection technologies. *Life Sci. Soc. Policy*, 13, 1, 16, 2017. Published 2017 Sep 20.
- 53. Ioppolo, G., Vazquez, F., Hennerici, M., Andrés, E., Medicine 4.0: New Technologies as Tools for a Society 5.0. *J. Clin. Med.*, 9, 2198, 2020.
- 54. Tang, C., Plasek, J.M., Bates, D.W., Rethinking Data Sharing at the Dawn of a Health Data Economy: A Viewpoint. *J. Med. Int. Res.*, 20, 11, e11519, 2018. Published 2018 Nov 22.
- 55. Gleim, L.C., Karim, M.R., Zimmermann, L. *et al.*, Enabling ad-hoc reuse of private data repositories through schema extraction. *J. Biomed. Semant.*, 11, 1, 6, 2020. Published 2020 Jul 8.

## Using Artificial Intelligence to Help COVID-19 Patients

Ayush Hans

Department of Computer Engineering, Indian Institute of Information Technology, Sonepat, Kurukshetra, Haryana, India

**Abstract**
COVID-19 is an infectious disease caused by the SARS-CoV-2 virus. SARS-CoV-2 infection is spreading similar to wildfire across the globe. In March 2020, it was declared a global pandemic by the World Health Organization (WHO). At the time of writing, more than 14 million people have tested positive. Infections have been growing rapidly across the globe and tremendous efforts are being made to fight this disease. The major challenge that the COVID-19 outbreak has brought is for effective management of the healthcare industry. The supervision of COVID-19 patients in an effective way so as to reduce the threat of further spread has to be the most challenging job for the healthcare industry. This study has its main focus on discovering new and building upon the existing AI-based model so as to improve the healthcare facilities for better recovery of the COVID-19 patients.

After selective studying through the resources, many AI related solutions for the healthcare industry have been proposed. This includes a three-stage model of AI application in critical care which includes input, process and output stage. The Input stage includes data for pre-processing. The Process stage includes various Artificial Intelligence techniques, Machine Learning algorithms, Deep Learning Algorithms for creating a model. The output stage includes the decision making part. This involves treatment of patients, diagnosis of disease, risk analysis, estimation, and management. All the efforts put in defeating COVID-19 by the healthcare industry could be supported by AI-based solutions, which would enhance the process of curing COVID-19 patients, especially those with critical symptoms in ICUs.

**Keywords:** Artificial intelligence (AI), COVID-19, critical care, pandemic, healthcare

### 14.1 Introduction

Since the beginning of mankind, humans have faced a large number of pandemics. Pandemic is the term which is used for an epidemic which occurs worldwide or has its effect on a greater part of the world population. According to the World Health Organization, a pandemic is the one which involves the worldwide spread of a new disease whereas an epidemic is the one which remains limited to one region, city or country. A pandemic is said to spread beyond national borders and possibly worldwide. The main causes of such pandemics can be the introduction of some new bacteria or virus strain or its subtype which easily transmits into the human body and even between them. Some of the deadliest pandemics in the history of mankind have been caused by a single bacterium which is known as Yersinia pestis, and commonly called Plague.

We have recorded many pandemics in the past. Some of the known ones include:

- Plague of Justinian (540 AD): estimated to have killed over 40 million (approximately half of the world’s population) [1].
- Black Death (1347 AD): the same Plague resurfaced after around 800 years killing around 200 million in a span of 3–4 years [2].
- Smallpox (15th century): it was a constant source of error for almost 3 centuries and was believed to be the cause of death of every third person at its peak. It wiped out 90% of the indigenous population of North American countries like USA and Mexico. Later in 1980, the World Health Organization announced the complete eradication of smallpox [3].
- Cholera (early 19th century): It is a waterborne disease, which is believed to have originated in India and traveled across the world killing countless people over the century [4].
- Influenza (early to mid-19th century): This appeared nearly simultaneously across the globe and was the cause of over 70 million deaths.

All these pandemics had a huge impact on world population due to the lack of adequate healthcare facilities. The late diagnosis was one of the main reasons that these diseases were very lethal and also became pandemics. But with the advent of new technologies and better healthcare facilities over the years, it seemed like mankind had proclaimed the victory over microbes as seen in Graph 14.1. This was in sight of the vaccines, medicines, antibodies, better treatments, and advanced technology, which were developed over the past few decades.

This was a very dangerous complacency, as the microbes never went away, they were just out of sight and have now resurfaced again. Since the start of 21st century, we have witnessed many such pathogens which have had an impact on the public health. Some notable ones include the Ebola virus, HIV, Zika virus, and many others, which were either new and less harmful viruses or were just the ones which we have already encountered in the past.

In 2019 an old virus known by the name coronavirus resurfaced. This is a type of virus which impacts the respiratory tracts in humans. Coronaviruses are known to cause different diseases some of which include the SARS (severe acute respiratory syndrome), MERS (Middle East respiratory syndrome), and the most recent, coronavirus disease 2019 (COVID-19).

COVID-19 (shorthand of Coronavirus) was first identified in Wuhan, China in December 2019. It was later declared a global pandemic by the World Health Organization (WHO) on 11 March 2020. At the time of writing, there are 21,450,473 confirmed cases, 805,587 deaths, and 12,34,808 recovered cases. The distribution of these cases across the world can be seen in Figure 14.1. The mortality rate (MR) and the number of cases of this virus is mainly dependent on a few important factors [5]. But the most important of them is the care of the critically affected patients. As the research suggests that the nasal cavity is the initial entry point of COVID virus.

![](img/cc49610e64f7c15534c72aab7adc7c74_332_0.png)

Graph 14.1 Increase in life expectancy with the advancement in healthcare sector.

Therefore, one of the most prominent challenges of this virus is the proper treatment of COVID-19 patients who need acute or critical respiratory care and have had a number of medical records in the past [6, 7].

These types of treatments have become possible because of the advancement in technology which has occurred in this century. The advent of computers was a major boost in the healthcare sector. The next huge boost that started a new phase in the healthcare sector was the introduction of the World Wide Web. It was due to the WWW that a high level of expertise reached even the smallest of areas, and now computers or electronic devices have become essential parts of the healthcare industry.

But even with this type of advancement, healthcare facilities have been overburdened during these times due to the rapid increase in the number of cases as depicted in Figure 14.2. So the main question that arises is that how can the critical care of the COVID-19 patients using AI be managed by the healthcare institutes around the world?

Over the past few years, AI (Artificial Intelligence) has become a necessity in healthcare. This is due to the benefits it brings to the table like automated clinical workflows, reduction in diagnostic errors, early detection of a disease and many more [8]. But in these situations where doctors need to work long shifts and also with immense precision, the role of AI needs to be an innovative one. A slight disturbance in the process of patients’ critical care and clinical decision making of these healthcare institutes would make all the people involved vulnerable to COVID [9].

The main aim of this paper was to evaluate the role of Machine Learning (ML) and Artificial Intelligence (AI) techniques in the treatment and

![](img/cc49610e64f7c15534c72aab7adc7c74_333_0.png)

Figure 14.1 Distribution of COVID cases across the world.
