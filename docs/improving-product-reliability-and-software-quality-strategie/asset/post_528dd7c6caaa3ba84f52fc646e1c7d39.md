
```
C:\tmp>cloc .
    7 text files.
    7 unique files.
github.com/AlDanial/cloc v 1.76  T=0.50 s (14.0 files/s, 3416.0 lines/s)
--------------------------------------------------------------------------------
Language    files    blank    comment    code
--------------------------------------------------------------------------------
C           3        55       174        880
C++         4        8        62         529
--------------------------------------------------------------------------------
SUM:        7        63       236        1409
--------------------------------------------------------------------------------
```

Figure 9.3 Sample line counts.

- 1. Estimating the expected number of defects for a project provides input for a defect model, which can be used to determine how close the project is to achieving its quality goals.
- 2. The estimate for the number of defects expected in the software project is used to plan the effort required to produce the software.

Generally, the expected defect density is based on historical data. Software organizations should be tracking the number of defects for each project that are found prior to release and the number of software defects reported by users after product release (escaped defects). The sum of the discovered defects and escapes divided by the size of the release provides the defect density for that release. The defect density is generally stated as the number of defects/KLOC.

Defect counts and LOC should be tracked for each project or product release. This will provide the data to accurately estimate defect density. Tracking defects and code size over time provides a means to measure improvement in software quality over time.

Defect density could also be tracked per feature, product component, or technology. This more specific data can be helpful for both improving effort estimation as well as providing insight to areas of the product or process to improve. It is very informative, when creating effort estimates, to know that certain parts of the product may require more effort due to higher than average defect rates. Similarly, differentiated defect rate data can be very good input to consider when determining which areas of the product require improvement.

Defect density is not uniform for all projects. Three key factors can impact defect density:

- 1. *The language used to code the software.* Typically higher level languages, such as Java and C#, produce a higher defect density than a low-level language such as C or assembler.
- 2. *The code complexity.* More complex code typically has a higher defect density. For example, multitasking or real-time software tends to have higher defect densities. Older legacy software often falls in this category as well.
- 3. *The type of software application software being developed.* Higher-level software (e.g. database or web applications) tends to have lower overall defect densities.

Lower-level software (e.g. dealing with hardware or device drivers) typically has a higher defect density.

In most cases, a software group can collect historic defect density metrics for overall software releases and not worry about differentiating between different defect density rates for a given project. However, if different projects delivered by the same software organization have very different characteristics, it may be worthwhile using different defect density estimates for each type of project. Also, if forced to use industry data to create initial defect density estimates, it is useful to match that up with the type of project under development.

Historic defect density data may not be available when first starting to use defect models. If this is the case, it is possible to use generic industry defect density data as a starting point. The estimated defect density metric can be refined over time as actual data is collected. If historical data from your organization is not available, then industry data can be used in its place. According to research by Capers Jones [2], the average US software industry defect removal rate is 85%. That is, the average US software company removes about 85% of the software defects before release. As noted in Section 9.4, the best-of-breed software companies have an escape rate of 1.3–2.6 defects per KLOC, depending on language. Therefore, it is reasonable to assume that the average injection rate for a very good software development group would be between approximately 9 defects per KLOC for a low-level language and 18 defects per KLOC for a high-level language. One can assume that a best-of-breed software group would also employ good defect prevention techniques. So, these referenced industry numbers are too low and should be rounded up when using them to estimate defect rates for an organization that is just starting to deploy software quality control methods. If no historical data is available, I recommend using 10 defects per KLOC for low-level languages and 20 defects per KLOC for high-level languages.

## 9.5 Defect Models

Defect models estimate the expected number of defects that will be injected and/or found during a project. Some defect models can be very sophisticated and predict how many defects will be injected and discovered during each phase in the project. A simple defect model might just estimate the total number of defects expected to be created for a specific component or project.

Defect models are best if constructed based on historical data. If historical data is not available, initial defect models can be constructed using industry data.

A caveat about managing with defect models, in the words of George Box: “All models are wrong, but some models are useful.”[3] The defect model should not be treated as gospel. It is an estimate and thus inherently inaccurate. A defect model cannot predict the exact number of defects that are in the code at any given time, just give a rough estimate. It should be used as a general guideline to approximate the quality of the project at any point in time. Defect models are a very useful tool to manage the project, so long as management does not expect any given project metrics to match the model exactly.

Defect models allow a development organization to more effectively plan the work required for a project. Using an estimate from a defect model enables managers to plan

the work so that enough time and resources are available to find and remediate enough defects to achieve the desired quality goals. Managers can schedule the work so that the defects do not pile up. The most productive approach is to keep the number of open defects low and under control as the project progresses. This approach is recommended because if the number of open defects is allowed to get too high at any point in time, it becomes more difficult to test the software and harder to isolate individual issues. When individual defects take longer to determine the root cause and repair, overall productivity slips and the project will take longer and cost more. On top of this, if the defect count gets too high, then new development is occurring on a faulty foundation, which will lead to more issues as development progresses.

## 9.6 Defect Run Chart

A defect run chart shows the state of the software release quality over time during the development and testing of a software product and at product release. It allows managers to determine if the software release has achieved its target quality goals.

A defect run chart is composed of several data series. The first is the total number of defects expected to be found and fixed during the software release. This data is determined by the defect model. The second data series is the total number of defects discovered over time during the software development and test of a release. The third data series in the chart is the number of defects open at any given time during the release. A defect run chart is shown in Figure 9.4.

![](img/50dc187750ab899846d825394ccbb93a_179_0.png)
