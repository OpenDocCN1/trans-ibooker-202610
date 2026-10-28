# ## Hints

If you’re struggling to get the exercises done, the following hints will provide you with some inspiration on how to progress.

+   - In your `views.py`, create a function called `def about(request)`, and have the function return a `HttpResponse()`. Within this `HttpResponse()`, insert the message that you want to return.
+   - The expression to use for matching the second view is `'/about/'`. This means that in `rango/urls.py`, you would add in a new path mapping to the `about()` view.
+   - Within the `index()` view, you will want to include an HTML [hyperlink](https://en.wikipedia.org/wiki/Hyperlink) to provide a link to the about page. A `a` tag, complete with an `href` attribute (`<a href='/rango/about/'>About</a>`) will suffice.
+   - The same will also be added to the `about()` view, although this time it will point to `/rango/`, the homepage – not `/rango/about/`. An example would look like: `<a href='/rango/'>Index</a>`.
+   - If you haven’t done so already, now’s a good time to head off and complete part one of the official [Django Tutorial](https://docs.djangoproject.com/en/2.1/intro/tutorial01/).

# ## Test your Implementation

If you have completed everything in this chapter up to and including the exercises, you can test your implementation so far. [Follow the guide we provided earlier](#), using the test module `tests_chapter3.py`. Do the tests pass when run against your implementation?