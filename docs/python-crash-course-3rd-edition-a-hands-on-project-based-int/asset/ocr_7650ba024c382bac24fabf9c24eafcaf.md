enter the following commands to create a new project:

```
1  (ll_env)learning_log$ django-admin startproject ll_project .
2  (ll_env)learning_log$ ls
ll_env ll_project manage.py
3  (ll_env)learning_log$ ls ll_project
__init__.py asgi.py settings.py urls.py wsgi.py
```

The startproject command ① tells Django to set up a new project called ll_project. The dot (.) at the end of the command creates the new project with a directory structure that will make it easy to deploy the app to a server when we’re finished developing it.

NOTE
Don’t forget this dot, or you might run into some configuration issues when you deploy the app. If you forget the dot, delete the files and folders that were created (except ll_env) and run the command again.

Running the ls command (dir on Windows) ② shows that Django has created a new directory called ll_project. It also created a manage.py file, which is a short program that takes in commands and feeds them to the relevant part of Django. We’ll use these commands to manage tasks, such as working with databases and running servers.

The ll_project directory contains four files ③; the most important are settings.py, urls.py, and wsgi.py. The settings.py file controls how Django interacts with your system and manages your project. We’ll modify a few of these settings and add some settings of our own as the project evolves. The urls.py file tells Django which pages to build in response to browser requests. The wsgi.py file helps