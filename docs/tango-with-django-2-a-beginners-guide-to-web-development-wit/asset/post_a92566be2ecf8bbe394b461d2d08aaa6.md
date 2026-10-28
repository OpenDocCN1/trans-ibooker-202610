
```python
import json
import requests

# Add your Microsoft Account Key to a file called bing.key
def read_bing_key():
    """
    reads the BING API key from a file called 'bing.key'
    returns: a string which is either None, i.e. no key found, or with a key
    remember to put bing.key in your .gitignore file to avoid committing it.

    See Python Anti-Patterns - it is an awesome resource to improve your python code
    Here we using "with" when opening documents
    http://bit.ly/twd-antipattern-open-files
    """
    bing_api_key = None
    try:
        with open('bing.key','r') as f:
            bing_api_key = f.readline().strip()
    except:
        try:
            with open('../bing.key') as f:
                bing_api_key = f.readline().strip()
        except:
            raise IOError('bing.key file not found')

    if not bing_api_key:
        raise KeyError('Bing key not found')

    return bing_api_key

def run_query(search_terms):
    """
    See the Microsoft's documentation on other parameters that you can set.
    http://bit.ly/twd-bing-api
    """
    bing_key = read_bing_key()
    search_url = 'https://api.cognitive.microsoft.com/bing/v7.0/search'
    headers = {'Ocp-Apim-Subscription-Key': bing_key}
    params = {'q': search_terms, 'textDecorations': True, 'textFormat': 'HTML'}

    # Issue the request, given the details above.
    response = requests.get(search_url, headers=headers, params=params)
```

```html
{% extends 'rango/base.html' %}
{% load staticfiles %}

{% block title_block %}
    Search
{% endblock %}

{% block body_block %}
<div class="jumbotron p-4">
    <div class="container">
        <h1 class="jumbotron-heading">Search with Rango</h1>
    </div>
</div>
<div>
    <form class="form-inline"
          id="user-form"
          method="post"
          action="{% url 'rango:search' %}">
        {% csrf_token %}

        <div class="form-group">
            <input class="form-control"
                   type="text"
                   size="50"
                   name="query"
                   id="query" />
        </div>

        <button class="btn btn-primary"
                type="submit"
                name="submit">Search</button>
    </form>
</div>
<div>
    {% if result_list %}
        <h2>Results</h2>

        <div class="list-group">
            {% for result in result_list %}
                <div class="list-group-item">
                    <h3 class="list-group-item-heading">
                        <a href="{{ result.link }}">
                            {{ result.title|safe|escape }}
                        </a>
                    </h3>
```

Once you have completed the preceding three steps, you should now be able to navigate to your Rango app in your browser and search! The result should look something like [what you see below](#).

![](img/75169a27287f7c945bf2cbf7dede5251_245_0.png)

**The completed Bing search page, showing results for the query Tango with Django.**

Well done! You have successfully added search to Rango and thus learnt how to integrate your app with an external API. Integrating other APIs is broadly the same – you may find that some provide their Python wrappers so you can literally import a module and query the API using Python commands.

### Search Box Exercise

You may have noticed that when you issue a query, the query disappears when the results are shown. This is not very user-friendly, because research has shown that people like to *reformulate their queries* (or in different words, *tweak their original query*). Your exercise here is to figure out how to update the view and template so that the user’s query is displayed within the search box when results are returned.

## 14. Making Rango Tango Exercises

So far, we have added in several key pieces of functionality to Rango. We’ve been building the app up in a manner that hopefully gets you familiar with using the Django framework and to learn how to construct the various parts of a modern web application.

However, Rango at the moment is not very cohesive or interactive. In this chapter, we challenge you to improve the app and its user experience further by bringing together some of the functionality we have implemented along with some new features. To make Rango more coherent, integrated and interactive, let’s look at implementing the following functionality.

In Rango’s Category and Page models, we’ve included a views field. However, we haven’t used them yet, so why not implement the functionality to track views to both types of content? Specifically, we’ll be looking at:

- counting the number of times that a category is viewed; and
- counting the number of times a page is clicked from a Rango category view.

We’ll also be wanting to collect likes for different categories, as this is also something that we have not yet implemented. We’ll be looking at implementing this with AJAX (see [AJAX in Django](#)). We’ll also make use of AJAX to allow users to filter the categories that they see on the left-hand categories bar.

In addition to these new features, we’ll be working to provide further services to registered users of Rango. Specifically, we’ll be working to:

- allow users registering to the site to again specify a profile image and website (if you worked through the [chapter on django-registration-redux](#), this functionality will have been lost);
- let users view and edit their profile; and
- let users who are logged in view a list of other users and their profiles.

### Note

As we have alluded to above, we won’t be working through all of these tasks in this chapter. The AJAX-related tasks (as well as a definition of what AJAX is!) will be left to the [AJAX in Django] later on, while others will be left to you to complete as additional exercises.

Before we start to add this additional functionality, we will make a *todo list* to plan our workflow for each task. Breaking tasks down into smaller sub-tasks will greatly simplify the implementation process. We’ll then be attacking each main activity with a clear plan. In this chapter, we’ll be providing you with the workflow for a number of the above tasks. From what you have learnt so far, you should now be able to fill in the gaps and implement most of it on your own (except for those requiring AJAX, where we’ll talk about that in more detail in a [later chapter]).

We’ve included hints, tips and code snippets to help you along. Of course, if you get stuck, you can always check out [our implementation for the exercises on GitHub¹]. Let’s get started with tracking page clicks!

### 14.1 Tracking Page Clickthroughs

Currently, Rango provides direct links to external pages. While simple, this approach does not allow us to record how many clicks each page receives – a click takes you away from Rango without any ability to record information on what has just happened. To address this, we can implement a simple view to record a click – and *then* redirect the user to the request page.

This is what happens in many contemporary social media platforms, as an example – if someone sends you a link in Facebook Messenger, for instance, Facebook inserts a simple redirect device to be able to track that you clicked the link.

To implement functionality to track page links, have a go at the following steps.

1. First, create a new view called `goto_url()`. This should be mapped to the URL `/rango/goto/`, with a URL mapping name of `goto`.

¹ [https://github.com/maxwelld90/tango_with_django_2_code](https://github.com/maxwelld90/tango_with_django_2_code)

2. Take the search form and results template markup from `search.html`, and place it into `category.html` *underneath* the list of pages. You’ll only want to show the search functionality if the category exists in the database.

3. Update the search form definition such that the action refers back to the category URL, rather than the search URL.

4. Update the `show_category()` view to handle a HTTP POST request. The view must then include any search results in the context dictionary for the template to render. Remember that the search query will be provided as part of the POST request in field `query`.

5. Finally, update Rango so that only users who are logged in can view and use the updated search functionality. To restrict access within the `category.html` template, we can use something along the lines of the following code snippet.

```django
{% if user.is_authenticated %}
    <!-- Your search code goes here -->
{% endif %}
```

The markup and template code for the third step can be seen below.

```html
<form class="form-inline"
      id="user-form"
      method="post"
      action="{% url 'rango:show_category' category.slug %}">
```

### Commenting out Code

Commenting out code stops the interpreter or compiler from using the selected code. Although it cannot see it and execute it, you can still read it and use it. In Python, the simplest way to comment out a line of code is to prepend a `#` to the line – put it before any text.

Other languages will have different commenting syntax. HTML, for example, uses `<!--` to denote the start of a commented-out block of markup, and `-->` to denote the end.
