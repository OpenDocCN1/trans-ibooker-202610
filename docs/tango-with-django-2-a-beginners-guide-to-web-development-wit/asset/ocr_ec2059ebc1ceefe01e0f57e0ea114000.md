```
```python
if form.is_valid():
    form.save(commit=True)
    return redirect('rango:profile', user.username)
else:
    print(form.errors)

context_dict = {'user_profile': user_profile,
                'selected_user': user,
                'form': form}

return render(request, 'rango/profile.html', context_dict)
```
```

We’ll also need to import two classes that we haven’t used before. One is the standard Django User model, and the other is Rango’s UserProfile model. Note we did import the UserProfileForm earlier – that’s different!

```
```python
from django.contrib.auth.models import User
from rango.models import UserProfile
```
```

Our new class-based view requires that a user be logged in – hence the use of the @method_decorator(login_required) decorator for both the get() and post() methods.

Within the ProfileView class, we also created a get_user_details() helper method to ensure that we don’t repeat ourselves. This method begins by selecting the django.contrib.auth.User instance from the database – if it exists. If it does not exist, the method simply returns None, which signals to both the get() and post() methods to do a simple redirect to Rango’s homepage, rather than greet the user with a bespoke error message. We can’t display information for a non-existent user! If the user does exist, the get_user_details() method selects the UserProfile instance (or creates a blank one, if one does not exist). We then take the selected user’s details, and use those details to populate a UserProfileForm instance. The user, user_profile and populated form objects are then returned in a tuple and are unpacked¹⁰ by the get() and post() methods that call the helper method.

Within the post() method, we handle the case where a user wants to update their profile’s information. To do this, we extract information from the form into a