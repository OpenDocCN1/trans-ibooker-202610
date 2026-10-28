that you should be able to see your visualization as quickly as possible before refining its appearance. Here we use the px.bar() function to create a bar chart. We pass the list repo_names as the x argument and stars as the y argument.

Figure 17-1 shows the resulting chart. We can see that the first few projects are significantly more popular than the rest, but all of them are important projects in the Python ecosystem.

![](bbox=[0.12, 0.3, 0.88, 0.71])

Figure 17-1: The most-starred Python projects on GitHub

# Styling the Chart

Plotly supports a number of ways to style and customize the plots, once you know the information in the plot is correct. We’ll make some changes in the initial px.bar() call and then