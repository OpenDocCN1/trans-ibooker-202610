```html
</div>
{% endblock %}
```

As mentioned previously, this template is pretty straightforward compared to what we have been doing as of late! A series of `<div>` tags have been created using various Bootstrap classes to style the list. For each user, we display their username and provide a link to their profile page. Notice that since we pass through a list of `UserProfile` objects, we need to pass through the `user` attribute to lead to the username of the user!

# Creating the View for Listing User Profiles

With our template created, we can now create the corresponding view that selects all users from the `UserProfile` model. We also assume that the current user must be logged in to view the other users of Rango, as stated above.

The following simple class-based view should satisfy these requirements nicely. Again, this is a straightforward view – selecting all of the `UserProfile` instances via an ORM query is perhaps the trickiest part here. Remember, the list must have a name of `userprofile_list` in the context dictionary to match up with our template defined above. This code would live in `views.py`.

```
```python
class ListProfilesView(View):
    @method_decorator(login_required)
    def get(self, request):
        profiles = UserProfile.objects.all()

        return render(request,
                      'rango/list_profiles.html',
                      {'userprofile_list': profiles})
```
```

# Mapping and Linking the View

Our final step is to map a URL to the new `ListProfilesView` class-based view. Add the following pattern to the `urlpatterns` list provided in Rango’s `urls.py`.