The first way to customize a plot with Plotly is to use some optional parameters in the initial call that generates the plot, in this case, px.bar(). Here’s how to add an overall title and a label for each axis:

```
die_visual.py

--snip--
# Visualize the results.
① title = "Results of Rolling One D6 1,000 Times"
② labels = {'x': 'Result', 'y': 'Frequency of Result'}
fig = px.bar(x=poss_results, y=frequencies, title=title, labels=labels)
fig.show()
```

We first define the title that we want, here assigned to title ①. To define axis labels, we write a dictionary ②. The keys in the dictionary refer to the labels we want to customize, and the values are the custom labels we want to use. Here we give the x-axis the label Result and the y-axis the label Frequency of Result. The call to px.bar() now includes the optional arguments title and labels.

Now when the plot is generated it includes an appropriate title and a label for each axis, as shown in Figure 15-13.