# login.xhtml

```
{% extends 'learning_logs/base.xhtml' %}
{% load django_bootstrap5 %}
{% block page_header %}
<h2>Log in to your account.</h2>
{% endblock page_header %}
{% block content %}
<form action="{% url 'accounts:login' %}" method='post'>
{% csrf_token %}
{% bootstrap_form form %}
{% bootstrap_button button_type="submit" content="Log in" %}
</form>
{% endblock content %}
```

We first load the bootstrap5 template tags into this template ①. We then define the page_header block, which tells the user what the page is for ②. Notice that we’ve removed the {% if form.errors %} block from the template; django-bootstrap5 manages form errors automatically.

To display the form, we use the template tag {% bootstrap_form %} ③; this replaces the {{ form.as_div }} element we were using in Chapter 19. The {% bootstrap_form %} template tag inserts Bootstrap style rules into the form’s individual elements as the form is rendered. To generate the submit button, we use the {% bootstrap_button %} tag with arguments that designate it as a submit button, and give it the label Log in ④.

Figure 20-2 shows the login form now. The page is much cleaner, with consistent styling and a clear purpose. Try logging in with an incorrect username or password; you’ll see that even the error messages are styled consistently and integrate well with the overall site.