**the existing entry for the accounts/ URL – not after!** To ensure you get it in the right place, we have included a complete copy of what your `urlpatterns` list should look like.

```
urlpatterns = [
    path('', views.index, name='index'),
    path('rango/', include('rango.urls')),
    path('admin/', admin.site.urls),

    # New line below - don't forget the slash after register!
    path('accounts/register/',
         MyRegistrationView.as_view(),
         name='registration_register'),

    path('accounts/', include('registration.backends.simple.urls')),
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

This overrides the existing `registration_register` URL mapping, provided to us in the standard, out-of-the-box django-registration-redux urls.py module, and replaces the URL we jump to with a mapping to our new class-based view.

# 15.4 Class-Based Views

What is a class-based view, though? What *exactly* have we made implement up in the example solution above? Class-based views are a more sophisticated and elegant mechanism for handling requests. Rather than taking a functional approach as we have done so far in this tutorial—that is creating *functions* in Rango’s `views.py` module to serve requests—the class-based approach means creating classes that inherit and implement a series of methods to handle your app’s requests.

For example, rather than checking if a request was an HTTP GET or an HTTP POST request, class-based viewed allow you to implement a `get()` and `post()` method within the class handling your request for a given view. When your project and handlers become more complex, using a class-based approach is the more preferable solution. You can look at the [Django Documentation for more information about class-based views](https://docs.djangoproject.com/en/2.1/topics/class-based-views/).