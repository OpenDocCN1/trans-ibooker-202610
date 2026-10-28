# Summary: What is a Template?

In the world of Django, think of a *template* as the scaffolding that is required to build a complete HTML webpage. A template contains the *static parts* of a webpage (that is, parts that never change), complete with special syntax (or *template tags*) which can be overridden and replaced with *dynamic content* that your Django app’s views can replace to produce a final HTML response.

# Configuring the Templates Directory

To get templates up and running with your Django app, you’ll need to create two directories in which template files are stored.

In your Django project’s directory (e.g. `<workspace>/tango_with_django_project/`), create a new directory called `templates`. To clarify, this should be at the same level as your project’s `manage.py` script. Within the new templates directory, you will then want to create further directory called `rango`. This means that the path `<workspace>/tango_with_django_project/templates/rango/` is the location where we will store templates associated with our rango application.

# Keep your Templates Organised

It’s good practice to separate your templates into subdirectories for each app you have. This is why we’ve created a rango directory within our templates directory. If you package your app up to distribute to other developers, it’ll be much easier to know which templates belong to which app!

To tell the Django project where templates will be stored, open your project’s `settings.py` file. Next, locate the `TEMPLATES` data structure. By default, when you create a new Django project, it will look like the following.