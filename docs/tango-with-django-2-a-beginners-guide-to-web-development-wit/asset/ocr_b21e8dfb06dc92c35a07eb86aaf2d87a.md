```
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
```

We can see from the output of this command that critical parts of the code have not been tested. The `views.py` has a pretty low coverage percentage as an example of 34%. Therefore, this output can provide you with a measure-based approach to determine where to focus your efforts on writing tests.

The coverage package [has many more features](https://coverage.readthedocs.io/en/latest/) that you can explore to make your tests even more comprehensive!