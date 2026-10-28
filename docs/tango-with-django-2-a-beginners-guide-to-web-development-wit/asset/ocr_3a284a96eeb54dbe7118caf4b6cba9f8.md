# ## Running the Unit Tests

How do you run the tests, though? This step-by-step process demonstrates the basic process on what you have to do. We will assume that you want to run the tests for [Chapter 3, Django Basics].

+   1. First, identify what chapter’s tests you want to run.
+   2. Either make a clone of our [sample code repository](https://github.com/maxwelld90/tango_with_django_2_code) on your computer, or access the individual test module that you want from the [GitHub web interface](https://github.com/maxwelld90/tango_with_django_2_code/tree/master/progress_tests).
+      - To do the latter, click the module you require (i.e. `tests_chapter3.py`). When you see the code on the GitHub website, click the Raw button and save the page that then loads.
+   3. Move the `tests_chapter3.py` module to your project’s `rango` directory. This step does not make sense right now; as you progress through the book and come back here to refresh your memory on what to do, this will make sense.
+   4. Run the command `$ python manage.py test rango.tests_chapter3`. This will start the tests.

You will also need to ensure that when these tests run, your `rangoenv` virtual environment is active.

Once the tests all complete, you should see OK. This means they all passed! If you don’t see OK, something failed – look through the output of the tests to see what test failed, and why. Sometimes, you might have missed something which causes an exception to be raised before the test can be carried out. In instances like this, you’ll need to look at what is expected, and go back and fill it in. You can tweak your code and re-run the tests to see if they then pass.

![](bbox=[0.126, 0.743, 0.176, 0.795])

# ### Test your Implementation

When you have completed enough of the book to reach another round of tests, we’ll denote the prompt for you to do this like so. We’ll tell you what module to run, and always point you back to here so you can refresh your memory if you forget how to run them.