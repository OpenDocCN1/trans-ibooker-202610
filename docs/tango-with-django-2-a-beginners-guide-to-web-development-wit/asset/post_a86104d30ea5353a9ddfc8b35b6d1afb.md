
```python
def show_category(request, category_name_slug):
    context_dict = {}

    try:
        category = Category.objects.get(slug=category_name_slug)
        pages = Page.objects.filter(category=category).order_by('-views')

        context_dict['pages'] = pages
        context_dict['category'] = category
    except Category.DoesNotExist:
        context_dict['category'] = None
        context_dict['pages'] = None

    # Start new search functionality code.
    if request.method == 'POST':
        query = request.POST['query'].strip()

        if query:
            context_dict['result_list'] = run_query(query)
    # End new search functionality code.

    return render(request, 'rango/category.html', context_dict)
```

We keep the `show_category()` view the same at the top, and add in an additional block of code towards the end to handle the processing of a search request. If a POST request is made, we then attempt to take the query from the request and issue the query to the `run_query()` function we defined in the [Bing Search chapter](#). This then returns a list of results which we put into the `context_dict` with a key of `result_list` – exactly the variable name that is expected in the updated `category.html` template.

Search functionality should then all work as expected. Try it out! Navigate to a category page, and you should see a search box. Enter a search query, submit it, and see what happens.

Once you are satisfied, you can safely delete the existing `search.html` template from your codebase, along with the decommissioned `search()` view.

## Restricting Access to Search Functionality

Our final requirement was to restrict the migrated search functionality only to those who are logged into Rango. This step is straightforward – one can simply wrap the search-handling template code added to `category.html` with a conditional template check to determine if the user is authenticated.

```django
{% if user.is_authenticated %}
<div>
    ....
</div>
{% endif %}
```

Too easy! You could also go further and add checks within the `show_category()` view to ensure the search functionality part is not executed when the current user is not logged in. Be wary, though – don’t be inclined to add the `login_required()` decorator to the view. Doing so will restrict all category-viewing functionality to logged in users only – you only want to restrict the *search* functionality!

## Query Box Exercise

At the end of the Bing Search API chapter, we set an exercise. We noted that in its current state, users would issue a query and then be presented with the results. However, the query box would then be blanked again – thus making *query reformulation* more taxing.

In our code examples above, we’ve deliberately kept our model solution to this particular exercise out. How could you allow the results page to ‘*remember*’ the query that was entered, and place it back in the search box?

The easiest solution to this problem would be to simply place the query variable into the `context_dict` of `show_category()`, and then make use of the variable within the `category.html` template by specifying its value as the value attribute for the search box `<input>` element.

In Rango’s `show_category()` view, locate the code block that deals with the search functionality, and add the query to the `context_dict`, like so.

```python
# Start new search functionality code.
if request.method == 'POST':
    query = request.POST['query'].strip()

    if query:
        context_dict['result_list'] = run_query(query)
        context_dict['query'] = query
# End new search functionality code.
```

Once this has been completed, open Rango’s `category.html` template and modify the query `<input>` field like so.

```html
<input class="form-control"
    type="text"
    size="50"
    name="query"
    value="{{ query }}"
    id="query" />
```

Template variable `{{ query }}` will be replaced with the user’s query, thus setting it to be the default value for the `<input>` field when the page loads.

Once everything has been completed, you should have a category page that looks similar to the example below. Well done!

![](img/75169a27287f7c945bf2cbf7dede5251_263_0.png)

Rango’s updated category view, complete with Bing Search API search functionality. Note also the inclusion of the search terms in the query box – they are still retained!

## 15.3 Creating a UserProfile Instance

This section provides one solution for creating Rango UserProfile accounts in conjunction with the django-registration-redux package. Recall that the standard `django.contrib.auth.models.User` model contains several standard fields related to user accounts, such as a username and password. However, we chose to implement an additional `UserProfile` model within Rango to store additional information such as a user’s website URL, and a profile image. Here, we’ll go through the steps required to implement this functionality. The steps that we’ll work on are listed below, and each corresponds to a subsection below.

1. We first will create a new `profile_registration.html` template that will be our pre-existing `UserProfileForm` Django form.
2. We’ll then work on creating a new `register_profile()` view to capture the details for a new `UserProfile` instance.
3. After that, the standard procedure applies. We’ll first map the new view to a new URL – in this instance, the URL will be `/rango/register_profile/`. Even though this deals with user accounts, it is *Rango specific*, so it makes sense to place it within the `/rango/` URL pattern.
4. We’ll then need to write some code for the `django-registration-redux` package to tell it to redirect to a different place when a User object has been created – or in other words, redirect the user to the new `/rango/register_profile/` URL.

The fourth step requires some additional code that requires some specialist knowledge of the `django-registration-redux` package. We’ve taken care of that for you, but we’ll point to the relevant parts of the associated documentation to show you how we figured everything out.

Once complete, the basic flow/experience for a user registering with Rango will be as follows.

1. The user will jump to the Rango website.
2. They will then click the `Register Here` link on the navigation bar.
3. They will then be redirected to the `django-registration-redux` registration form. This form is located at the URL `/accounts/register`.
4. Once this form has been completed, the form will be submitted and processed. If successful, they will then be redirected to the new `/rango/register_profile/` page, allowing them to create a `UserProfile` instance.
5. Once this has been completed and submitted, the user will be redirected to the homepage. Registration will then be complete.

Order here is important as the way we implement the new `register_profile()` view will assume that a user has created a standard User instance beforehand. This will be required to link to their new UserProfile instance and ensure referential integrity is maintained!

### Creating a Profile Registration Template

First, let’s focus on creating a simple template that will provide the necessary HTML markup for displaying the UserProfileForm form fields. As we mentioned previously, we’ll be keeping the django-registration-redux forms separate from the new profile registration form – this new template will belong in the rango templates directory. Remember, we’re not dealing with built-in Django models here – we are dealing with the creation of an instance of a custom-made model.

Create a new template in `templates/rango/` called `profile_registration.html`. Within this new template, add in the following HTML markup and Django template code.

```html
{% extends 'rango/base.html' %}
{% load staticfiles %}

{% block title_block %}
    Register - Step 2
{% endblock %}

{% block body_block %}
<div class="jumbotron p-4">
    <div class="container">
        <h1 class="jumbotron-heading">Register Here - Step 2</h1>
    </div>
</div>

<div class="container">
    <div class="row">
        <form method="post"
              action="{% url 'rango:register_profile' %}"
              enctype="multipart/form-data">
            {% csrf_token %}
            {{ form.as_p }}
            <input type="submit" value="Create Profile" />
        </form>
    </div>
</div>
{% endblock %}
```

Like the django-registration-redux forms that we created previously (with an example here), this template inherits from Rango’s base.html template. Recall that
