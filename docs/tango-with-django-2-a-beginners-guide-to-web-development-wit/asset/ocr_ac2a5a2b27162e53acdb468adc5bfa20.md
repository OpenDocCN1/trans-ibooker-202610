The User model has five key attributes. They are:

+   - the *username* for the user account;
+   - the account’s *password*;
+   - the user’s *email address*;
+   - the user’s *first name*; and
+   - the user’s *surname*.

The User model also comes with other attributes such as `is_active`, `is_staff` and `is_superuser`. These are boolean fields used to denote whether the account is active, owned by a staff member, or has superuser privileges respectively. Check out the Django documentation on the user model¹⁰ for a full list of attributes provided by the base User model.

# 9.5 Additional User Attributes

If you would like to include other user related attributes than what is provided by the User model, you will needed to create a model that is *associated* with the User model. For our Rango app, we want to include two more additional attributes for each user account. Specifically, we wish to include:

+   - a `URLField`, allowing a user of Rango to specify their personal website’s URL; and
+   - a `ImageField`, which allows users to specify a picture for their user profile.

This can be achieved by creating an additional model in Rango’s `models.py` file. Let’s add a new model called `UserProfile`: