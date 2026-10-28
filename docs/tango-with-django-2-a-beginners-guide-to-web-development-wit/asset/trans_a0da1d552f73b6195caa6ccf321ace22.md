

to a variable definition like the example below. Make sure you put this underneath the definition of `BASE_DIR`!

### Delete those Lines!

If you included the three `print()` statements above to see what’s going on, make sure you remove them once you understand. Don’t just leave them lying there. They will clutter your settings module, and clutter the output of the Django development server!

```python
TEMPLATE_DIR = os.path.join(BASE_DIR, 'templates')
```

Here, we make use of the `os.path.join()` function to join together (or *concatenate*) the value of the `BASE_DIR` variable, and the string `'templates'`. Upon completion, this concatenation yields `<workspace>/tango_with_django_project/templates/`. From here, we can then use our new `TEMPLATE_DIR` variable to replace the hard-coded path we defined earlier in `TEMPLATES`. Update the DIRS key/value pairing to look like the following code snippet.

```python
'DIRS': [TEMPLATE_DIR, ]
```

### Why `TEMPLATE_DIR`?

You’ve created a new variable called `TEMPLATE_DIR` at the top of your `settings.py` file because it’s easier to access should you ever need to change it. For more complex Django projects, the DIRS list allow you to specify more than one template directory to draw templates from. For this book however, one location is sufficient to get everything working.

### Concatenating Paths

**When concatenating system paths together, always use `os.path.join()`.** Using this built-in function ensures that the correct path separators are used. On a UNIX operating system (or derivative of), forward slashes (`/`) would be used to separate directories, whereas a Windows operating system would use backward slashes (`\`). If you manually append slashes to paths, you may end up with path errors when attempting to run your code on a different operating system, thus reducing your project’s portability.

### Adding a Template

With your template directory and path now set up, create a file called index.html and place it in the templates/rango/ directory. Within this new file, add the following HTML markup and Django template code.

```html
<!DOCTYPE html>
<html>

<head>
<title>Rango</title>
</head>

<body>
<h1>Rango says...</h1>
<div>
hey there partner! <br />
<strong>{{ boldmessage }}</strong><br />
</div>
<div>
<a href="/rango/about/">About</a><br />
</div>
</body>

</html>
```

From this HTML code, it should be clear that a simple HTML page is going to be generated that greets a user with a hello world message. You might also notice some non-HTML in the form of `{{ boldmessage }}`. This is a Django template variable. We can set values to these variables so they are replaced with whatever we want when the template is rendered. We’ll get to that in a moment.

To use this template, we need to reconfigure the `index()` view that we created earlier. Instead of dispatching a simple response, we will change the view to dispatch our template.

In `rango/views.py`, check to see if the following import statement exists at the top of the file. Django should have added it for you when you created the Rango app. If it is not present, add it, but you should have kept it from earlier.

```python
from django.shortcuts import render
```

You can then update the `index()` view function as follows. Check out the inline commentary to see what each line does.

```python
def index(request):
    # Construct a dictionary to pass to the template engine as its context.
    # Note the key boldmessage matches to {{ boldmessage }} in the template!
    context_dict = {'boldmessage': 'Crunchy, creamy, cookie, candy, cupcake!'}

    # Return a rendered response to send to the client.
    # We make use of the shortcut function to make our lives easier.
    # Note that the first parameter is the template we wish to use.
    return render(request, 'rango/index.html', context=context_dict)
```

First, we construct a dictionary of key/value pairs that we want to use within the template. Then, we call the `render()` helper function. This function takes as input the user’s request, the template filename, and the context dictionary. The `render()` function will take this data and mash it together with the template to produce a complete HTML page that is returned with a `HttpResponse`. This response is then returned and dispatched to the user’s web browser.

### What is the Template Context?

![](img/75169a27287f7c945bf2cbf7dede5251_56_0.png)

When a template file is loaded with the Django templating system, a *template context* is created. In simple terms, a template context is a Python dictionary that maps template variable names with Python variables. In the template we created above, we included a template variable name called `boldmessage`. In our updated `index(request)` view example, the string `Crunchy, creamy, cookie, candy, cupcake!` is mapped to template variable `boldmessage`. The string `Crunchy, creamy, cookie, candy, cupcake!` therefore replaces *any* instance of `{{ boldmessage }}` within the template.

Now that you have updated the view to employ the use of your template, start the Django development server and visit `http://127.0.0.1:8000/rango/`. You should see your simple HTML template rendered in your browser – and it should look just like the [example screenshot shown below](#). If you don’t, read the error message presented to see what the problem is, and then double-check all the changes that you have made. One of the most common issues people have with templates is that the path is set incorrectly in `settings.py`. Sometimes it’s worth adding a print statement to `settings.py` to report the `BASE_DIR` and `TEMPLATE_DIR` to make sure everything is correct.

This example demonstrates how to use templates within your views. However, we have only touched on a fraction of the functionality provided by the Django templating engine. We will use templates in more sophisticated ways as you progress through this book. In the meantime, you can find out more about [templates from the official Django documentation](https://docs.djangoproject.com/en/2.1/ref/templates/).

![](img/75169a27287f7c945bf2cbf7dede5251_57_0.png)

**What you should see when your first template is working correctly. Note the bold text – Crunchy, creamy, cookie, candy, cupcake! – which originates from the view, but is rendered in the template.**

## 4.2 Serving Static Media Files

While you’ve got templates working, your Rango app is admittedly looking a bit plain right now – there’s no styling or imagery. We can add references to other files in our HTML template such as *Cascading Style Sheets (CSS)*⁸, *JavaScript*⁹ and images to improve the presentation. These are called *static files*, because they are not generated dynamically by a web server; they are simply sent as is to a client’s web browser. This section shows you how to set Django up to serve static files, and shows you how to include an image within your simple template.

### Configuring the Static Media Directory

To start, you will need to set up a directory in which static media files are stored. In your project directory (e.g. `<workspace>/tango_with_django_project/`), create a new directory called `static` and a new directory called `images` inside `static`. Check that the new static directory is at the same level as the templates directory you created earlier in this chapter.

Next, place an image inside the `images` directory. As shown in below, we chose a picture of [the chameleon Rango](https://www.imdb.com/title/tt1192628/)¹⁰ — a fitting mascot, if ever there was one.

> ⁸ http://en.wikipedia.org/wiki/Cascading_Style_Sheets  
> ⁹ https://en.wikipedia.org/wiki/JavaScript  
> ¹⁰ http://www.imdb.com/title/tt1192628/

### Test your Configuration

As a small exercise, test to see if everything is working correctly. Try and view the `rango.jpg` image in your browser when the Django development server is running. If your `STATIC_URL` is set to `/static/` and `rango.jpg` can be found at `images/rango.jpg`, what is the complete URL that you would enter into your web browser’s window to access this resource?

**Try to figure this out before you move on! It’ll help you understand how to interpret static URLs.** The answer is coming up if you are struggling to figure it out.

### Don’t Forget the Slashes!

When setting `STATIC_URL`, check that you end the URL you specify with a forward slash (e.g. `/static/`, not `/static`). As per the [official Django documentation](https://docs.djangoproject.com/en/2.1/ref/settings/#std:setting-STATIC_URL), not doing so can open you up to a world of pain. The extra slash at the end ensures that the root of the URL (e.g. `/static/`) is separated from the static content you want to serve (e.g. `images/rango.jpg`).

### Serving Static Content

While using the Django development server to serve your static media files is fine for a development environment, it’s highly unsuitable for a production environment. The [official Django documentation on deployment](https://docs.djangoproject.com/en/2.1/howto/static-files/deployment/) provides further information about deploying static files in a production environment. We’ll look at this issue in more detail however when we [deploy Rango].

If you haven’t managed to figure out where the image should be accessible from, point your web browser to `http://127.0.0.1:8000/static/images/rango.jpg`.

### Static Media Files and Templates

Now that you have your Django project set up to handle static files, you can now make use of these files within your templates to improve their appearance and add additional functionality.

