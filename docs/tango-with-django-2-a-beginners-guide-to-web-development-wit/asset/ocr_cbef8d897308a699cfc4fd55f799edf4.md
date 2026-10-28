Django provides a decorator called `login_required()`, which we can attach to any view where we require the user to be logged in. If a user is not logged in and attempts to access a view decorated with `login_required()`, they are then redirected to another page ([that you can set](https://docs.djangoproject.com/en/2.1/ref/settings/#login-url)) – typically the login page.

# Restricting Access with a Decorator

To try this out, create a view in Rango’s `views.py` module called `restricted()`, and add the following code.

```
```python
@login_required
def restricted(request):
    return HttpResponse("Since you're logged in, you can see this text!")
```
```

Note that to use a decorator, you place it *directly above* the function signature, and put a `@` before naming the decorator. Python will execute the decorator before executing the code of your function/method. As a decorator is still a function, you’ll still have to import it if it resides within an external module. As `login_required()` exists elsewhere, the following import statement is required at the top of `views.py`.

```
```python
from django.contrib.auth.decorators import login_required
```
```

We’ll also need to add in another URL mapping to Rango’s `urlpatterns` list in the `urls.py` file. Add the following line of code.

```
```python
path('restricted/', views.restricted, name='restricted'),
```
```

To make the new restricted page accessible, you can also add another link to Rango’s `base.html` template.