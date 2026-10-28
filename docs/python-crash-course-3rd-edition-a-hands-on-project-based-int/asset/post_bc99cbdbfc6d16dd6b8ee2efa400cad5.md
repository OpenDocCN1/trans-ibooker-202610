
Make a second entry for Chess and one entry for Rock Climbing so we have some initial data. Here's a second entry for Chess:

In the opening phase of the game, it's important to bring out your bishops and knights. These pieces are powerful and maneuverable enough to play a significant role in the beginning moves of a game.

And here's a first entry for Rock Climbing:

One of the most important concepts in climbing is to keep your weight on your feet as much as possible. There's a myth that climbers can hang all day on their arms. In reality, good climbers have practiced specific ways of keeping their weight over their feet whenever possible.

These three entries will give us something to work with as we continue to develop Learning Log.

## The Django Shell

Now that we've entered some data, we can examine it programmatically through an interactive terminal session. This interactive environment is called the Django shell, and it's a great environment for testing and troubleshooting your project. Here's an example of an interactive shell session:

```
(ll_env)learning_log$ python manage.py shell
>>> from learning_logs.models import Topic
>>> Topic.objects.all()
<QuerySet [<Topic: Chess>, <Topic: Rock Climbing>]>
```

The command `python manage.py shell`, run in an active virtual environment, launches a Python interpreter that you can use to explore the data stored in your project's database. Here, we import the model Topic from the learning_logs.models module ❶. We then use the method Topic.objects.all() to get all instances of the model Topic; the list that's returned is called a queryset.

We can loop over a queryset just as we'd loop over a list. Here's how you can see the ID that's been assigned to each topic object:

```
>>> topics = Topic.objects.all()
>>> for topic in topics:
...     print(topic.id, topic)
...
1 Chess
2 Rock Climbing
```

We assign the queryset to topics and then print each topic's id attribute and the string representation of each topic. We can see that Chess has an ID of 1 and Rock Climbing has an ID of 2.

If you know the ID of a particular object, you can use the method Topic.objects.get() to retrieve that object and examine any attribute the object has. Let's look at the text and date_added values for Chess:

```
>>> t = Topic.objects.get(id=1)
>>> t.text
'Chess'
>>> t.date_added
datetime.datetime(2022, 5, 20, 3, 33, 36, 928759,
    tzinfo=datetime.timezone.utc)
```

We can also look at the entries related to a certain topic. Earlier, we defined the topic attribute for the Entry model. This was a ForeignKey, a connection between each entry and a topic. Django can use this connection to get every entry related to a certain topic, like this:

```
① >>> t.entry_set.all()
<QuerySet [<Entry: The opening is the first part of the game, roughly...>, <Entry:
In the opening phase of the game, it's important t...>]
```

To get data through a foreign key relationship, you use the lowercase name of the related model followed by an underscore and the word set ①. For example, say you have the models Pizza and Topping, and Topping is related to Pizza through a foreign key. If your object is called my_pizza, representing a single pizza, you can get all of the pizza's toppings using the code my_pizza.topping_set.all().

We'll use this syntax when we begin to code the pages users can request. The shell is really useful for making sure your code retrieves the data you want it to. If your code works as you expect it to in the shell, it should also work properly in the files within your project. If your code generates errors or doesn't retrieve the data you expect it to, it's much easier to troubleshoot your code in the simple shell environment than within the files that generate web pages. We won't refer to the shell much, but you should continue using it to practice working with Django's syntax for accessing the data stored in the project.

Each time you modify your models, you'll need to restart the shell to see the effects of those changes. To exit a shell session, press CTRL-D; on Windows, press CTRL-Z and then press ENTER.

To see how this works, let's make the home page for Learning Log. We'll define the URL for the home page, write its view function, and create a simple template.

Because we just want to ensure that Learning Log works as it's supposed to, we'll make a simple page for now. A functioning web app is fun to style when it's complete; an app that looks good but doesn't work well is pointless. For now, the home page will display only a title and a brief description.

## Mapping a URL

Users request pages by entering URLs into a browser and clicking links, so we'll need to decide what URLs are needed. The home page URL is first: it's the base URL people use to access the project. At the moment, the base URL, *http://localhost:8000/*, returns the default Django site that lets us know the project was set up correctly. We'll change this by mapping the base URL to Learning Log's home page.

In the main *ll_project* folder, open the file *urls.py*. You should see the following code:

*ll_project/urls.py*

```
① from django.contrib import admin
from django.urls import path

② urlpatterns = [
③     path('admin/', admin.site.urls),
]
```

The first two lines import the `admin` module and a function to build URL paths ①. The body of the file defines the `urlpatterns` variable ②. In this *urls.py* file, which defines URLs for the project as a whole, the `urlpatterns` variable includes sets of URLs from the apps in the project. The list includes the module admin.site.urls, which defines all the URLs that can be requested from the admin site ③.

We need to include the URLs for learning_logs, so add the following:

```
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('learning_logs.urls')),
]
```

We've imported the include() function, and we've also added a line to include the module learning_logs.urls.

The default urls.py is in the ll_project folder; now we need to make a second urls.py file in the learning_logs folder. Create a new Python file, save it as urls.py in learning_logs, and enter this code into it:

learning_logs/urls.py

```
① """Defines URL patterns for learning_logs."""

② from django.urls import path

③ from . import views

④ app_name = 'learning_logs'
⑤ urlpatterns = [
    # Home page
⑥    path('', views.index, name='index'),
]
```

To make it clear which urls.py we're working in, we add a docstring at the beginning of the file ①. We then import the path function, which is needed when mapping URLs to views ②. We also import the views module ③; the dot tells Python to import the *views.py* module from the same directory as the current *urls.py* module. The variable `app_name` helps Django distinguish this *urls.py* file from files of the same name in other apps within the project ④. The variable `urlpatterns` in this module is a list of individual pages that can be requested from the `learning_logs` app ⑤.

The actual URL pattern is a call to the `path()` function, which takes three arguments ⑥. The first argument is a string that helps Django route the current request properly. Django receives the requested URL and tries to route the request to a view. It does this by searching all the URL patterns we've defined to find one that matches the current request. Django ignores the base URL for the project (*http://localhost:8000/*), so the empty string ('') matches the base URL. Any other URL won't match this pattern, and Django will return an error page if the URL requested doesn't match any existing URL patterns.

The second argument in `path()` ⑥ specifies which function to call in *views.py*. When a requested URL matches the pattern we're defining, Django calls the `index()` function from *views.py*. (We'll write this view function in the next section.) The third argument provides the name *index* for this URL pattern so we can refer to it more easily in other files throughout the project. Whenever we want to provide a link to the home page, we'll use this name instead of writing out a URL.

## *Writing a View*

A view function takes in information from a request, prepares the data needed to generate a page, and then sends the data back to the browser. It often does this by using a template that defines what the page will look like.

The file *views.py* in *learning_logs* was generated automatically when we ran the command `python manage.py`
