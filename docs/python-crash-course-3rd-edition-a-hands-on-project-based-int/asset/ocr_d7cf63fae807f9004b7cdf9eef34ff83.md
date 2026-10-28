match, and the user isn’t trying to do anything malicious in their submission.

If the submitted data is valid, we call the form’s save() method to save the username and the hash of the password to the database ④. The save() method returns the newly created user object, which we assign to new_user. When the user’s information is saved, we log them in by calling the login() function with the request and new_user objects ⑤, which creates a valid session for the new user. Finally, we redirect the user to the home page ⑥, where a personalized greeting in the header tells them their registration was successful.

At the end of the function, we render the page, which will be either a blank form or a submitted form that’s invalid.

# The register Template

Now create a template for the registration page, which will be similar to the login page. Be sure to save it in the same directory as login.xhtml:

*register.xhtml*

```
{% extends "learning_logs/base.xhtml" %}

{% block content %}

<form action="{% url 'accounts:register' %}" method='post'>
    {% csrf_token %}
    {{ form.as_div }}

    <button name="submit">Register</button>
</form>

{% endblock content %}
```

This should look like the other form-based templates we’ve been writing. We use the as_div method again so Django will display all the fields in the form appropriately,