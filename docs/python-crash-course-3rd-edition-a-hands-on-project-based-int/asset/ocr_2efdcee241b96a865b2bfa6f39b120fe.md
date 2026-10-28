```
```
<form action="{% url 'accounts:logout' %}" method='post'>
    {% csrf_token %}
    <button name='submit' class='btn btn-outline-secondary btn-sm'>
        Log out</button>
</form>
{% endif %}

</div> <!-- Closes collapsible parts of navbar -->
--snip--
```
```

The logout form should be placed after the set of account-related links, but inside the collapsible section of the navigation bar. The only change in the form is the addition of a number of Bootstrap styling classes in the `<button>` element, which apply Bootstrap styling elements to the logout button ❶.

Reload the home page, and you should be able to log in and out using any of the accounts you've created.

There's still a bit more we need to add to *base.xhtml*. We need to define two blocks that the individual pages can use to place the content specific to those pages.

# Defining the Main Part of the Page

The rest of *base.xhtml* contains the main part of the page:

**base.xhtml**

```
```
--snip--
</nav> <!-- End of navbar -->

<main class="container">
    <div class="pb-2 mb-2 border-bottom">
        {% block page_header %}{% endblock page_header %}
    </div>
    <div>
        {% block content %}{% endblock content %}
    </div>
```
```