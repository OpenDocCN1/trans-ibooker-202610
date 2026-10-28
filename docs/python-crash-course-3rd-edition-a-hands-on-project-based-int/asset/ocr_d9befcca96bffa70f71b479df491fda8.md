![](bbox=[0.125, 0.09, 0.877, 0.475])

Figure 20-2: The login page styled with Bootstrap

# Styling the Topics Page

Let’s make sure the pages for viewing information are styled appropriately as well, starting with the topics page:

# topics.xhtml

```
{% extends 'learning_logs/base.xhtml' %}

{% block page_header %}
<h1>Topics</h1>
{% endblock page_header %}

{% block content %}
<ul class="list-group border-bottom pb-2 mb-4">
{% for topic in topics %}
<li class="list-group-item border-0">
<a href="{% url 'learning_logs:topic' topic.id %}">
```