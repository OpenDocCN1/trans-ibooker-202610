
we recommend that you create a workspace directory in your home folder. The path `/Users/maxwelld90/Workspace/` would then constitute as a valid directory for the user maxwelld90 on a Mac.

## Can’t find `django-admin.py`?

Try entering `django-admin` instead. Depending on your setup, some systems may not recognise `django-admin.py`. This is especially true on Windows computers – you may have to use the full path to the `django-admin.py` script, for example:

```
python c:\Users\maxwelld90\.virtualenvs\rangeonv\bin\django-admin.py
        startproject tango_with_django_project
```

as suggested on [StackOverflow](http://stackoverflow.com/questions/8112630/cant-create-django-project-using-command-prompt). Note that the path will likely vary on your own computer.

This command will invoke the `django-admin.py` script, which will set up a new Django project called `tango_with_django_project` for you. Naming conventions dictate that we would typically append `_project` to the end of our Django project directories so we know exactly what they contain—but naming this is really entirely up to you.

You’ll now notice within your workspace is a directory set to the name of your new project, `tango_with_django_project`. Within this newly created directory, you should see two items:

- `tango_with_django_project`, another *nested directory* with the same name as your main project directory (argh!); and
- a Python script called `manage.py`.

For the purposes of this tutorial, we call the `tango_with_django_project` nested directory the *project configuration directory*. Within this directory, you will find four Python scripts. We will discuss these scripts in detail later on, but for now, you should see:

- `__init__.py`, a blank Python script whose presence indicates to the Python interpreter that the directory is a Python package;
- `settings.py`, the place to store all of your Django project’s settings;
- `urls.py`, a Python script to store URL patterns for your project; and
- `wsgi.py`, a Python script used to help run your development server and deploy your project to a production environment.

In the project directory, you will see there is a file called `manage.py`. We will be calling this script time and time again as we develop our project. It provides you with a series of commands you can run to maintain your Django project. For example, `manage.py` allows you to run the built-in Django development server, test your application, and run various database commands. We will be using the script for virtually every Django command that we want to run.

## The Django Admin and Manage Scripts

For Further Information on Django admin script, see the Django documentation for more details about the [Admin and Manage scripts](https://docs.djangoproject.com/en/2.1/ref/django-admin/#django-admin-py-and-manage-py).

Note that if you run `python manage.py help` you can see the list of commands available.

You can try using the `manage.py` script now, by issuing the following command.

```bash
$ python manage.py runserver
```

Executing this command will launch Python, and instruct Django to initiate its lightweight development server. You should see the output in your terminal window similar to the example shown below:

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

In the output, you can see several things. First, there are no issues that stop the application from running. However, you will notice that a warning is raised – unapplied migration(s). We will talk about this in more detail when we set up our database, but for now we can ignore it. Third, and most importantly, you can see that a URL has been specified: http://127.0.0.1:8000/, which is the address that the Django development server is running at.

Now open up your web browser and enter the URL mentioned above into your browser’s address bar – [http://127.0.0.1:8000/](http://127.0.0.1:8000/). You should see a webpage similar to [the one shown below](http://127.0.0.1:8000/).

![](img/75169a27287f7c945bf2cbf7dede5251_38_0.png)

**A screenshot of the initial Django page you will see when running the development server for the first time.**

You can stop the development server at any time by pushing CTRL + C in your terminal or Command Prompt window. This applies to both Macs and PCs! If you wish to run the development server on a different port or allow users from other machines to access it, you can do so by supplying optional arguments. Consider the following command.

```bash
$ python manage.py runserver <your_machines_ip_address>:5555
```

Executing this command will force the development server to respond to incoming requests on TCP port 5555. You will need to replace <your_machines_ip_address> with your computer’s IP address or 127.0.0.1.

## Don’t know your IP Address?

If you use 0.0.0.0, Django figures out what your IP address is. Go ahead and try:

```bash
python manage.py runserver 0.0.0.0:5555
```

When setting ports, it is unlikely that you will be able to use TCP port 80 or 8080 as these are traditionally reserved for HTTP traffic. Also, any port below 1024 is considered to be [privileged](http://www.w3.org/Daemon/User/Installation/PrivilegedPorts.html) by your operating system.

While you won’t be using the lightweight development server to deploy your application, it’s nice to be able to demo your application on another machine in your network. Running the server with your machine’s IP address will enable others to enter in `http://<your_machines_ip_address>:<port>/` and view your web application. **Of course, this will depend on how your network is configured. There may be proxy servers or firewalls in the way that would need to be configured before this would work. Check with the administrator of the network you are using if you can’t view the development server remotely.**

## 3.3 Creating a Django App

A Django project is a collection of *configurations* and *apps* that together make up a given web application or website. One of the intended outcomes of using this approach is to promote good software engineering practices. By developing a series of small applications, the idea is that you can theoretically drop an existing application into a different Django project and have it working with minimal effort.

A Django application exists to perform a particular task. You need to create specific apps that are responsible for providing your site with particular kinds of functionality. For example, we could imagine that a project might consist of several apps including a polling app, a registration app, and a specific content related app. In another project, we may wish to re-use the polling and registration apps, and so can include them in other projects. We will talk about this later. For now, we are going to create the app for the *Rango* app.

To achieve this, you need to make sure you’re in your Django project’s directory (e.g. `<workspace>/tango_with_django_project`). From there, run the following command.

```bash
$ python manage.py startapp rango
```

The `startapp` command creates a new directory within your project’s root. Unsurprisingly, this directory is called `rango` – and contained within it are several Python scripts:

- another `__init__.py`, serving the same purpose as discussed previously;
- `admin.py`, where you can register your models so that you can benefit from some Django machinery which creates an admin interface for you;
- `apps.py`, that provides a place for any app-specific configuration;
- `models.py`, a place to store your app’s data models – where you specify the entities and relationships between data;
- `tests.py`, where you can store a series of functions to test your implementation;
- `views.py`, where you can store a series of functions that handle requests and return responses; and
- the `migrations` directory, which stores database specific information related to your models.

`views.py` and `models.py` are the two files you will use for any given app and form part of the main architectural design pattern employed by Django, i.e. the *Model-View-Template* pattern. You can check out [the official Django documentation](https://docs.djangoproject.com/en/2.1/intro/overview/) to see how models, views and templates relate to each other in more detail.

Before you can get started with creating your models and views, you must first tell your Django project about your new app’s existence. To do this, you need to modify the `settings.py` file, contained within your project’s configuration directory. Open the file and find the `INSTALLED_APPS` list. Add the `rango` app to the end of the tuple, which should then look like the following example.
