
is derived from the Python implementation, but also provides several asserts and specific test cases unique to Django and web development.

If we then run the test, we will see the following output.

```text
$ python manage.py test rango

Creating test database for alias 'default'...
F
======================================================================
FAIL: test_ensure_views_are_positive (rango.tests.CategoryMethodTests)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/Users/maxwellld90/Workspace/tango_with_django_project/rango/tests.py", line 12, in test_ensure_views_are_positive
    self.assertEqual((cat.views>=0), True)
AssertionError: False != True

----------------------------------------------------------------------
Ran 1 test in 0.001s

FAILED (failures=1)
```

We can see that Django picked up our solitary test, and it FAILED. This is because the model does not check whether the value for views is less than zero. Since we want to ensure that the values are non-zero for this particular field, we will need to update the model to ensure that this requirement is fulfilled. Update the model now by adding some code to the `save()` method of the `Category` model, located in Rango’s `models.py` module. The code should check the value of the `views` attribute, and set the value to zero if the provided value is less than zero. A simple conditional check on `self.views` should suffice.

Once you have updated your model, re-run the test. See if your code now passes the test. If not, try again and work out a solution that passes the test.

Let’s try adding a further test that ensures that an appropriate slug is created. This should mean a slug with dashes instead of spaces (`-`), and all in lowercase. Add the following test method to your new `CategoryMethodTests` class.

```python
class IndexViewTests(TestCase):
    def test_index_view_with_no_categories(self):
        """
        If no categories exist, the appropriate message should be displayed.
        """

        response = self.client.get(reverse('rango:index'))

        self.assertEqual(response.status_code, 200)
        self.assertContains(response, 'There are no categories present.')
        self.assertQuerysetEqual(response.context['categories'], [])
```

As we are using the Django `reverse()` function to perform a URL lookup, we’ll need to make sure that the correct import statement is included at the top of the `tests.py` module, too.

```python
from django.urls import reverse
```

Looking at the code above, the Django `TestCase` class has access to a `client` object which can make requests. Here, it uses the helper function `reverse()` to lookup the URL of Rango’s index page. It then tries to issue an HTTP GET request on that page. The response is returned and stored in `response`. The test then checks several things: whether the page loaded successfully (with a 200 status code returned); whether the response’s HTML contains the string ‘There are no categories present.’; and whether the context dictionary used to render the response contains an empty list for the categories supplied.

Recall that when you run tests, a new database is created, which by default is not populated. This is true for each test method – and explains why the categories you create in the two `CategoryMethodTests` tests are not visible to the test in `IndexViewTests`.

Now let’s check the index view when categories are present. We can add a helper function for us to achieve this. This simple function doesn’t live within a class – just place it in the `tests.py` module.

Run the test. Does it pass?

## Update your Existing Tests

Update the existing tests that you have created so far to make use of the helper `add_category()` function.

## Testing the Rendered Page

Django’s test suite also allows you to perform tests that load up your web app, and programmatically interact with the DOM elements on the rendered HTML pages. This is incredibly useful, as you can test your web app as a human would – by ‘clicking’ links, or entering information into form fields and submitting them. This is achieved with some further third-party “driver” software that controls interactions on a webpage for you.

We don’t explicitly cover how to set these tests up here, but you should refer to the [official Django documentation](https://docs.djangoproject.com/en/2.2/topics/testing/tools/#liveservertestcase) to learn more and see how this is achieved.

If you really want us to include examples of how to run tests this way, let us know!

## 18.2 Examining Testing Coverage

One further point of discussion about testing is *coverage*. Code coverage measures how much of your codebase has been tested. You can install an additional package called coverage (`$ pip install coverage`) that can automatically analyse how much of your codebase that has been covered by tests. Once coverage is installed, run the following command at your terminal or Command Prompt.

```bash
$ coverage run --source='.' manage.py test rango
```

This will run through all of the tests that you have implemented so far, and collect coverage data for the Rango app. To see the report, you then need to type the following command once the first command is completed.

```text
$ coverage report
Name                                      Stmts   Miss  Cover
-------------------------------------------------------------
manage.py                                     9      2    78%
populate_rango.py                            33     33     0%
rango/__init__.py                              0      0   100%
rango/admin.py                                10      0   100%
rango/apps.py                                  3      3     0%
rango/bing_search.py                          38     32    16%
rango/forms.py                                34      6    82%
rango/migrations/0001_initial.py               6      0   100%
rango/migrations/0002_auto_20200111_1313.py     4      0   100%
rango/migrations/0003_category_slug.py          4      0   100%
rango/migrations/0004_auto_20200111_1702.py     6      0   100%
rango/migrations/__init__.py                    0      0   100%
rango/models.py                                33      3    91%
rango/templatetags/__init__.py                  0      0   100%
rango/templatetags/rango_template_tags.py       6      0   100%
rango/tests.py                                 34      0   100%
rango/urls.py                                   4      0   100%
rango/views.py                                207    139    33%
tango_with_django_project/__init__.py           0      0   100%
tango_with_django_project/settings.py          28      0   100%
tango_with_django_project/urls.py              12      1    92%
tango_with_django_project/wsgi.py               4      4     0%
-------------------------------------------------------------
TOTAL                                         475    223    53%
```

We can see from the output of this command that critical parts of the code have not been tested. The `views.py` has a pretty low coverage percentage as an example of 34%. Therefore, this output can provide you with a measure-based approach to determine where to focus your efforts on writing tests.

The coverage package [has many more features](https://coverage.readthedocs.io/en/latest/) that you can explore to make your tests even more comprehensive!

### Hints

You’ll want to use the timezone-aware functionality of Django to get the current date and time. This requires you to import: `from django.utils import timezone`, with the current date and time accessible with `timezone.now()`.

In order to compare two dates, you will need to make use of the `assertTrue()` method to perform the assertion. This takes a boolean expression, where you can perform your evaluation, such as `page.last_visit < timezone.now()`.

When working on the second test, you’ll need to make a call to the `rango:goto` view, passing the `page_id` as a parameter. You can do this with the `client.get()` method by providing a dictionary as the second argument with `page_id` as the key, and the ID of the page you are looking to access as the value. Depending upon how you implement this test, it may also require you to refresh a model instance from the database. This can be achieved using the `refresh_from_db()` method.

As a final hint, it may also be helpful to write a further helper function to add a page to a given category. If you are stuck, we have implemented a [model solution on GitHub](https://github.com/maxwelld90/tango_with_django_2_code/tree/ce0911025891970d6ee3ac5b8745ea970477151c/).

### Other Test Resources

- Run through [Part Five of the official Django Tutorial](https://docs.djangoproject.com/en/2.1/intro/tutorial05/) to learn more about testing.
- Check out the [tutorial on test-driven development by Harry Percival](https://www.obeythetestinggoat.com/book/part1.harry.html).
