To see how this works, let’s make the home page for Learning Log. We’ll define the URL for the home page, write its view function, and create a simple template.

Because we just want to ensure that Learning Log works as it’s supposed to, we’ll make a simple page for now. A functioning web app is fun to style when it’s complete; an app that looks good but doesn’t work well is pointless. For now, the home page will display only a title and a brief description.

# Mapping a URL

Users request pages by entering URLs into a browser and clicking links, so we’ll need to decide what URLs are needed. The home page URL is first: it’s the base URL people use to access the project. At the moment, the base URL, *http://localhost:8000/*, returns the default Django site that lets us know the project was set up correctly. We’ll change this by mapping the base URL to Learning Log’s home page.

In the main *ll_project* folder, open the file *urls.py*. You should see the following code:

*ll_project/urls.py*

```
① from django.contrib import admin
from django.urls import path

② urlpatterns = [
③     path('admin/', admin.site.urls),
]
```

The first two lines import the `admin` module and a function to build URL paths ①. The body of the file defines the `urlpatterns` variable ②. In this *urls.py* file, which defines URLs for the project as a whole, the `urlpatterns` variable includes sets of URLs from the apps in the project. The list includes