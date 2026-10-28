```
```python
--snip--
# My settings.
LOGIN_REDIRECT_URL = 'learning_logs:index'
LOGOUT_REDIRECT_URL = 'learning_logs:index'
LOGIN_URL = 'accounts:login'
```
```

Now when an unauthenticated user requests a page protected by the `@login_required` decorator, Django will send the user to the URL defined by `LOGIN_URL` in *settings.py*.

You can test this setting by logging out of any user accounts and going to the home page. Click the **Topics** link, which should redirect you to the login page. Then log in to any of your accounts, and from the home page, click the **Topics** link again. You should be able to access the topics page.

# Restricting Access Throughout Learning Log

Django makes it easy to restrict access to pages, but you have to decide which pages to protect. It’s best to think about which pages need to be unrestricted first, and then restrict all the other pages in the project. You can easily correct over-restricted access, and it’s less dangerous than leaving sensitive pages unrestricted.

In Learning Log, we’ll keep the home page and the registration page unrestricted. We’ll restrict access to every other page.

Here’s *learning_logs/views.py* with `@login_required` decorators applied to every view except `index()`:

# *learning_logs/views.py*

```
```python
--snip--
@login_required
def topics(request):
    --snip--

@login_required
```
```