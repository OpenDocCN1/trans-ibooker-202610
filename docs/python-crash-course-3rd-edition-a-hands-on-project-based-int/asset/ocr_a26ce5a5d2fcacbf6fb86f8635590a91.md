We first import the Entry model. We then get the entry object that the user wants to edit ① and the topic associated with this entry. In the if block, which runs for a GET request, we make an instance of EntryForm with the argument instance=entry ②. This argument tells Django to create the form, prefilled with information from the existing entry object. The user will see their existing data and be able to edit that data.

When processing a POST request, we pass both the instance=entry and the data=request.POST arguments ③. These arguments tell Django to create a form instance based on the information associated with the existing entry object, updated with any relevant data from request.POST. We then check whether the form is valid; if it is, we call save() with no arguments because the entry is already associated with the correct topic ④. We then redirect to the topic page, where the user should see the updated version of the entry they edited ⑤.

If we’re showing an initial form for editing the entry or if the submitted form is invalid, we create the context dictionary and render the page using the edit_entry.xhtml template.

# The edit_entry Template

Next, we create an edit_entry.xhtml template, which is similar to new_entry.xhtml:

# edit_entry.xhtml

```
{% extends "learning_logs/base.xhtml" %}

{% block content %}
    <p><a href="{% url 'learning_logs:topic' topic.id %}">{{ topic }}</a></p>
    <p>Edit entry:</p>
```