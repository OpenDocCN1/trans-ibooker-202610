# 7. Forms

As part of the Rango application, we will want to capture new categories and new pages from users. In this chapter, we will run through how to capture data through web forms. Django comes with some excellent form handling functionality, making it a pretty straightforward process to collect information from users and save it to the database via the models. According to Django’s documentation on forms¹, the form handling functionality allows you to:

+   1. display an HTML form with automatically generated *form widgets* (like a text field or date picker);
+   2. check submitted data against a set of validation rules;
+   3. redisplay a form in case of validation errors; and
+   4. convert submitted form data to the relevant Python data types.

One of the major advantages of using Django’s forms functionality is that it can save you a lot of time and hassle creating the HTML forms.

# 7.1 Basic Workflow

The basic steps involved in creating a form and handling user input is as follows.

+   1. If you haven’t already got one, create a `forms.py` module within your Django app’s directory (`rango`) to store form-related classes.
+   2. Create a `ModelForm` class for each model that you wish to represent as a form.
+   3. Customise the forms as you desire.
+   4. Create or update a view to handle the form...
+      - including *displaying* the form,
+      - *saving* the form data, and