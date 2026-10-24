
weight-loss program, and most likely have higher motivation to improve their diet and exercise more often than nonparticipants. It’s surprising how often we fool ourselves and forget to consider how voluntary participation skews program effectiveness, whether the subject is weight-loss clinics, social services, or school choice programs.⁹

![](img/216890131d46798dfe1cd5f886cc608c_98_0.png)

**Figure 5-3.** To evaluate program effectiveness, don’t compare program nonparticipants (A) with those who apply or volunteer to participate (B). Instead, randomly split all participants into two subgroups (C and D).

How can we reduce self-selection bias in program evaluation data? As you learned in “Question Your Data”, it’s important to look below the surface level to fully comprehend how terms have been defined, and how data was collected and recorded. By contrast, a well-designed program evaluation will reduce self-selection bias by randomly dividing all volunteer participants (group B) into two subgroups: half will be assigned to participate in one weight-loss program (group C) and the other half will be assigned to a different weight-loss program (group D), as shown in Figure 5-3. Because subgroups C and D were selected by chance from the same larger group of volunteers, we can be more confident when comparing their progress because there’s no reason to suspect any difference in motivation or other hard-to-see factors. Of course, there are many more research design details that are beyond the scope of this book, such as ensuring that sample sizes are sufficiently large, comparing participants before, during, and after the weight-loss activity, and so forth. But the logic of avoiding selection bias is simple: randomly divide the people who apply or volunteer to participate into subgroups, to better compare program effectiveness among people with similar motivations and other hard-to-see characteristics.

Bias warnings appear in several chapters of this book because we continually need to be aware of different types that negatively influence our work at various stages of the data visualization process. In “Recognize and Reduce Data Bias”, you’ll learn how to recognize and reduce other types of biases when working with data, such as cognitive biases, algorithmic biases, intergroup biases, and mapping biases.

## Summary

Although we don’t claim to teach you statistical data analysis in this book, in this chapter we discussed several common-sense strategies to make meaningful comparisons while analyzing your data. You learned how to use two words more precisely for comparing data, why and how to normalize data, and we gave you advice on watching out for biased comparisons. In Part I, you built up your skills on refining your data story, working with spreadsheets, finding and questioning data, and cleaning up messy data. Now you can combine all of this knowledge and begin to create interactive charts and maps in Part II.

> 1  Edward R. Tufte, *Envisioning Information* (Cheshire, CT: Graphics Press, 1990), p. 67.
> 
> 2  For a reader-friendly introduction to statistical logic and its limits, see Charles Wheelan, *Naked Statistics: Stripping the Dread from the Data* (W.W. Norton & Company, 2013); David Spiegelhalter, *The Art of Statistics: How to Learn from Data* (Basic Books, 2019).
> 
> 3  Andrew Adam Newman, “Selling Gum with Health Claims,” *The New York Times: Business*, July 27, 2009, https://oreil.ly/BN9HT.
> 
> 4  Alberto Cairo, *The Truthful Art: Data, Charts, and Maps for Communication* (Pearson Education, 2016), pp. 71–74.

5 Cairo, p. 95

6 “What’s Real About Wages?” Federal Reserve Bank of St. Louis (The FRED Blog, February 8, 2018), https://oreil.ly/yljrl.

7 Carl T. Bergstrom and Jevin D. West, Calling Bullshit: The Art of Skepticism in a Data-Driven World (Random House, 2020), https://oreil.ly/kpD_S, pp. 79, 104–133.

8 Jonathan Rothbaum and Adam Bee, “Coronavirus Infects Surveys, Too: Nonresponse Bias During the Pandemic in the CPS ASEC” (US Census Bureau), accessed December 8, 2020, https://oreil.ly/auhUm.

9 If you’re interested in more on self-selection bias in school choice programs, Richard D. Kahlenberg and Halley Potter’s A Smarter Charter (Teachers College Press), 54 is a good place to start.

## Part II. Building Visualizations

## Chapter 6. Chart Your Data

Charts pull readers deeper into your story. Images such as the slope of a line chart, or clusterings of dots on a scatter chart, can communicate your evidence to readers’ eyes more effectively than text or tables. But creating meaningful charts that draw our attention to key insights in your data requires clear thinking about design choices.

In this chapter, we’ll learn to identify good charts from bad ones in “Chart Design Principles”. You’ll review important rules that apply to all charts, and also some aesthetic guidelines to follow when customizing your own designs. While many tools allow you to download charts as *static* images, our book also demonstrates how to construct *interactive* charts that invite readers to explore the data in their web browsers. Later you’ll learn how to embed interactive charts on your website in Chapter 9.

Learn about different types of charts you can create in this book in Table 6-1. Decisions about chart types are based on two main factors: the format of your data and the kind of story you wish to tell. For example, line charts work best to show a series of continuous data points (such as change over time), while range charts are better suited to emphasize the distance between data categories (such as inequality gaps). After selecting your chart type, follow our tool recommendations and step-by-step tutorials. This chapter features *easy tools* with drag-and-drop menus in “Google Sheets Charts”, “Datawrapper Charts”, and “Tableau Public Charts”. But the table also points you to *power tools* that give you more control to customize and host your visualizations, such as Chart.js and Highcharts code templates in Chapter 11. These advanced tools require prior knowledge of how to edit and host code templates with GitHub, which is in Chapter 10.

> **NOTE**
> 
> We jointly refer to *bar and column charts* because they’re essentially the same, except that bars are oriented horizontally and columns vertically. The main difference is the length of your data labels. Use bar charts to display longer labels (such as “Mocha Frappuccino 24-ounce” and “Double Quarter Pounder with Cheese”) because they require more horizontal reading space. You can use either bar or column charts for shorter labels that do not require as much room (such as “Starbucks” and “McDonald’s”). You’ll also notice that all of the examples in this chapter focus on food (because we were really hungry when writing it) and healthy eating (because we also need to lose weight).

## Table 6-1. Basic chart types, best uses, and tutorialsᵃ

| Chart | Best use and tutorials in this book |
|---|---|
| Grouped bar or column chart | |
| Split bar or column chart | |

Grouped bar or column chart

![](img/216890131d46798dfe1cd5f886cc608c_102_0.png)

Best to compare categories side-by-side. If labels are long, use horizontal bars instead of vertical columns.

- Easy tool: "Bar and Column Charts" or "Datawrapper Charts"
- Power tool: Chart.js and Highcharts templates in Chapter 11

Split bar or column chart

![](img/216890131d46798dfe1cd5f886cc608c_102_1.png)

Best to compare categories in separate clusters. If labels are long, use horizontal bars instead of vertical columns.

- Easy tool: "Bar and Column Charts" or "Datawrapper Charts"
- Power tool: Chart.js and Highcharts templates in Chapter 11

## Chart

### Stacked bar or column chart

![](img/216890131d46798dfe1cd5f886cc608c_103_0.png)

Best to compare subcategories, or parts of a whole. If labels are long, use horizontal bars instead of vertical columns.

- Easy tool: “Bar and Column Charts” or “Datawrapper Charts”
- Power tool: Chart.js and Highcharts templates in Chapter 11

### Error bars in bar or column chart

![](img/216890131d46798dfe1cd5f886cc608c_103_1.png)

Best to show margin of error bars when comparing categories side by side. If labels are long, use horizontal bars instead of vertical columns.

- Easy tool: Google Sheets Charts has limited support for error bars (“Bar and Column Charts”)
- Power tool: Chart.js and Highcharts templates in Chapter 11

### Histogram
