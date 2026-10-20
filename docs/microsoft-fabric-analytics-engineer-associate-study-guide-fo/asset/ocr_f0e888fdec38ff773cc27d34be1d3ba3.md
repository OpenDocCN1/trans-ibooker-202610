![](bbox=[0.14, 0.08, 0.86, 0.31])

Figure 5-20. Creating a field parameter in Power BI by clicking Modeling and then “New parameter”

Then, a pop-up called Parameters will appear. This is where we set things up. First, Power BI asks what we want to create a parameter for. In our case, we want to switch fields, so the default selection is fine. Then, we give our parameter a name, and below that, we select the fields we want to include. At the bottom, there’s an option to automatically add a slicer to the page. This is selected by default, and we keep it that way because we use this slicer to navigate among the different dimensions in the final visual. Figure 5-21 shows what the pop-up looks like with three dimensions added: Product, Category, and Country.

Then, we click on Create to create the field parameter. It is shown in the Data pane as a table, together with the tables of our model. To use the parameter, we can simply create a bar chart like the one we created before, with the sales on the x-axis and the parameter on the y-axis (see Figure 5-22).