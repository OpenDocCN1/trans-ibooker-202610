```
```text
$ python manage.py runserver

Performing system checks...

System check identified no issues (0 silenced).

You have 14 unapplied migration(s).
Your project may not work properly until you apply the migrations for app(s):
admin, auth, contenttypes, sessions.

Run 'python manage.py migrate' to apply them.

July 23, 2019 - 17:12:34
Django version 2.1.5, using settings 'tango_with_django_project.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```
```

In the output, you can see several things. First, there are no issues that stop the application from running. However, you will notice that a warning is raised – unapplied migration(s). We will talk about this in more detail when we set up our database, but for now we can ignore it. Third, and most importantly, you can see that a URL has been specified: http://127.0.0.1:8000/, which is the address that the Django development server is running at.

Now open up your web browser and enter the URL mentioned above into your browser’s address bar – [http://127.0.0.1:8000/](http://127.0.0.1:8000/). You should see a webpage similar to [the one shown below](http://127.0.0.1:8000/).