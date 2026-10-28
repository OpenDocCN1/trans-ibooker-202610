```
# until we're ready to avoid integrity problems.
profile = profile_form.save(commit=False)
profile.user = user

# Did the user provide a profile picture?
# If so, we need to get it from the input form and
#put it in the UserProfile model.
if 'picture' in request.FILES:
    profile.picture = request.FILES['picture']

# Now we save the UserProfile model instance.
profile.save()

# Update our variable to indicate that the template
# registration was successful.
registered = True
else:
    # Invalid form or forms - mistakes or something else?
    # Print problems to the terminal.
    print(user_form.errors, profile_form.errors)
else:
    # Not a HTTP POST, so we render our form using two ModelForm instances.
    # These forms will be blank, ready for user input.
    user_form = UserForm()
    profile_form = UserProfileForm()

# Render the template depending on the context.
return render(request,
    'rango/register.html',
    context = {'user_form': user_form,
               'profile_form': profile_form,
               'registered': registered})
```

While the view looks pretty complicated, it’s actually very similar to how we implemented the add category and add page views. However, the key difference here is that we have to handle two distinct ModelForm instances – one for the User model, and one for the UserProfile model. We also need to handle a user’s profile image, if he or she chooses to upload one.

One trick we use here that has caught many people out in the past is the use of commit=False when saving the UserProfile form. This stops Django from saving the