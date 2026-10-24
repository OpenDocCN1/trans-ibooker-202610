
# Chapter 11. Chart.js and Highcharts Templates

In Chapter 6, we looked at powerful drag-and-drop tools, such as Google Sheets, Datawrapper, and Tableau Public to build interactive charts.

In this chapter, we’ll look into creating interactive charts using two popular JavaScript libraries, Chart.js and Highcharts. Since we don’t expect our readers to be proficient in JavaScript or any other programming language, we designed templates that you can copy to your own GitHub account, substitute data files, and publish them to the web without writing a single line of code. For those of you who are code-curious, we’ll show how the JavaScript code in these templates can be customized.

Why would anyone prefer JavaScript to easy-to-use Datawrapper or Tableau, you may wonder? Well, a few reasons. Although JavaScript code may seem overwhelming and intimidating at first, it allows for greater customization in terms of colors, padding, interactivity, and data handling than most third-party tools can offer. In addition, you can never be sure that third-party apps will remain free, or at least have a free tier, forever, whereas open source tools are here to stay, free of charge, as long as someone maintains the code.

> NOTE
> 
> Although both libraries are open source, Highcharts comes with a stricter license that allows it to be used for free for noncommercial projects only, such as a personal, school, or nonprofit organization website. Keeping that in mind, we primarily focus on Chart.js, which is distributed under an MIT license that lets you use the library for commercial projects as well.

Table 11-1 lists all types of charts that we’ll look at in this chapter. Both libraries include many more default chart types that you can explore in Chart.js Samples and Highcharts Demos. However, we strongly advise against using some chart types, such as three-dimensional ones, for reasons we discussed in the Chart Design Principles section of Chapter 7.

## Table 11-1. Chart code templates, best uses, and tutorials

| Chart | Best use and tutorials in this book |
|---|---|
| Bar or column chart | Best to compare categories side-by-side. If labels are long, use horizontal bars instead of vertical columns. |
| Error bars in a bar/column chart | Best to show margin of error bars when comparing categories side-by-side. If labels are long, use horizontal bars instead of vertical columns. |

Bar or column chart

![](img/216890131d46798dfe1cd5f886cc608c_310_0.png)

Best to compare categories side-by-side. If labels are long, use horizontal bars instead of vertical columns.

Power tool: “Bar or Column Chart with Chart.js”

Error bars in a bar/column chart

![](img/216890131d46798dfe1cd5f886cc608c_310_1.png)

Best to show margin of error bars when comparing categories side-by-side. If labels are long, use horizontal bars instead of vertical columns.

Power tool: “Error Bars with Chart.js”

### Chart

#### Line chart

![](img/216890131d46798dfe1cd5f886cc608c_311_0.png)

Best to show continuous data, such as change over time.

Power tool: “Line Chart with Chart.js” (see tutorial note to modify line chart into stacked area chart)

#### Annotated line chart

![](img/216890131d46798dfe1cd5f886cc608c_311_1.png)

Best to add contextual notes inside chart of continuous data, such as change over time.

Power tool: “Annotated Line Chart with Highcharts”

#### Scatter chart

### Chart

Best use and tutorials in this book

Best to show the relationship between two datasets as x and y coordinates to reveal possible correlations.

Power tool: “Scatter Chart with Chart.js”

West Haven
Median Household Income, USD: $52,353
Grade, Relative to Average: -0.6

#### Bubble chart

Best to show the relationship between three or four sets of data, with x and y coordinates, bubble size, and color.

Power tool: “Bubble Chart with Chart.js”

East Hartford
Median Household Income, USD: $50,400
Grade, Relative to Average: -1.4
Enrollment: 7,053 students

## Bar or Column Chart with Chart.js

In this section, we’ll show you how to create bar or column charts using Chart.js. We’ll use a Chart.js code template that pulls data from a CSV file, as shown in Figure 11-1. This column chart shows how many students in five school districts in Connecticut were English-language learners in the 2018–2019 academic year.

### English Learners by Select School Districts in Connecticut, 2018-19

![](img/216890131d46798dfe1cd5f886cc608c_313_0.png)

Source: CT Department of Education
Download data powering this visualization.

Figure 11-1. Bar chart with Chart.js: explore the interactive version.

To create your own bar or column chart with CSV data using our Chart.js template:

- 1. Go to our GitHub repo that contains the code for the chart in Figure 11-1, log into your GitHub account, and click “Use this template” to create a copy that you can edit.

## TEMPLATE FILE STRUCTURE

If you don’t remember how to use GitHub, we recommend you revisit **Chapter 10**. The repo contains three files that are directly related to the chart:

*index.html*

Contains HTML (markdown) and CSS (stylesheets) that tell the browser how to style the document that contains the chart and what libraries to load.

*script.js*

Contains the JavaScript code that reads data from the CSV file and constructs the interactive chart.

*data.csv*

Is the comma-separated file that keeps all the data in the chart and can be edited in a text editor, or Google Sheets/Excel, etc.

The two remaining files are a *README.md* that describes the contents of the repo and *bar.png* that is an image that you can see in the README. All other GitHub templates in this chapter will be similarly structured.

- 2. Prepare your data in CSV format and upload into a *data.csv* file. Place labels that will appear along the axis in the first column, and each data series in its own column. Your CSV must contain at least two columns (labels and one data series). You can add as many data series columns as you wish.

| district | nonlearner | learner |
|---|---:|---:|
| Hartford | 15656 | 4111 |
| New Haven | 17730 | 3534 |

- 3. In *script.js*, customize the values of variables. Because you may not be familiar with JavaScript, let’s take a look at the code snippet that describes a single variable in the file:

```
// `false` for vertical column chart, `true` for horizontal bar chart
var HORIZONTAL = false;
```

The first line starts with `//` and is a comment to help you understand what the variable in the next line is responsible for. It doesn’t affect the code. As you can see, if the variable HORIZONTAL is set to *false*, the chart would have vertical bars (also known as columns). If set to *true*, the chart will contain horizontal bars. The second line contains the variable declaration itself. The equal sign (=) assigns the value that you see on the right (*false*) to the variable (var) called HORIZONTAL to the left. This line ends with the semicolon (;).

Here are some of the variables available for you to customize in *script.js*:

```
// `false` for vertical column chart, `true` for horizontal bar chart
var HORIZONTAL = false;

// `false` for individual bars, `true` for stacked bars
var STACKED = false;

// Which column defines 'bucket' names?
var LABELS = 'district';

// For each column representing a data series, define its name and color
var SERIES = [
  {
    column: 'nonlearner',
    name: 'Non-learners',
    color: 'gray'
  },
  {
    column: 'learner',
    name: 'Learners',
    color: 'blue'
  }
];

// x-axis label and label in tool tip
var X_AXIS = 'School Districts';

// y-axis label, label in tool tip
var Y_AXIS = 'Number of Enrolled Students';

// `true` to show the grid, `false` to hide
var SHOW_GRID = true;
```

```
// `true` to show the legend, `false` to hide
var SHOW_LEGEND = true;
```

These basic variables should be enough to get you started. It’s natural that you’ll want to move the legend, edit the appearance of the tool tip, or change the colors of the grid lines. We recommend you look at the official Chart.js documentation to get help with that.

## Error Bars with Chart.js

If your data comes with uncertainty (margins of error), we recommend you show it in your visualizations with the use of error bars. The bar chart template shown in Figure 11-2 shows median and mean (average) income for different-sized geographies: the US state of Colorado, Boulder County, Boulder city, and a census tract in the city.
