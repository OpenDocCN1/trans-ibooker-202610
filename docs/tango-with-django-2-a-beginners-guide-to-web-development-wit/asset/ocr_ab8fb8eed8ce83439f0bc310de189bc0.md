+   • *flagging up errors* which may occur when the user enters incorrect data (or no data at all) in the form.
+   5. Create or update a template to display the form.
+   6. Add a `urlpattern` to map to the new view (if you created a new one).

This workflow is a bit more complicated than those we have previously seen, and the views that we have to construct are lot more complex, too. However, once you undertake the process a few times, it will become clearer how everything pieces together. Trust us.

# 7.2 Page and Category Forms

Here, we will implement the necessary infrastructure that will allow users to add categories and pages to the database via forms.

First, create a file called `forms.py` within the `rango` application directory. While this step is not necessary (you could put the forms in the `models.py`), this makes your codebase tidier and easier to work with.

# Creating ModelForm Classes

Within Rango’s `forms.py` module, we will be creating several classes that inherit from Django’s `ModelForm`. In essence, a `ModelForm` is a *helper* class that allows you to create a Django Form from a pre-existing model. As we’ve already got two models defined for Rango (`Category` and `Page`), we’ll create ModelForms for both.

In `rango/forms.py` add the following code.