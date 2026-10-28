```
```python
for repo_dict in repo_dicts:
    repo_names.append(repo_dict['name'])
    stars.append(repo_dict['stargazers_count'])

# Build hover texts.
owner = repo_dict['owner']['login']
description = repo_dict['description']
hover_text = f"{owner}<br />{description}"
hover_texts.append(hover_text)

# Make visualization.
title = "Most-Starred Python Projects on GitHub"
labels = {'x': 'Repository', 'y': 'Stars'}
fig = px.bar(x=repo_names, y=stars, title=title, labels=labels,
             hover_name=hover_texts)
fig.update_layout(title_font_size=28, xaxis_title_font_size=20,
                  yaxis_title_font_size=20)
fig.show()
```
```

We first define a new empty list, `hover_texts`, to hold the text we want to display for each project ❶. In the loop where we process the data, we pull the owner and the description for each project ❷. Plotly allows you to use HTML code within text elements, so we generate a string for the label with a line break (`<br />`) between the project owner’s username and the description ❸. We then append this label to the list `hover_texts`.

In the `px.bar()` call, we add the `hover_name` argument and pass it `hover_texts` ❹. This is the same approach we used to customize the label for each dot in the map of global earthquake activity. As Plotly creates each bar, it will pull labels from this list and only display them when the viewer hovers over a bar. [Figure 17-3](#) shows one of these custom tooltips.