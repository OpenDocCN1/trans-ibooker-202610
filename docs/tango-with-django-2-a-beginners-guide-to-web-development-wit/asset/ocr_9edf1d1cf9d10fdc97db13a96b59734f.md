# Exercises

Now that you’ve worked through the chapter, consider the following questions, and how you could solve them.

+   - What would happen if you don’t enter in a category name on the add category form?
+   - What happens when you try to add a category that already exists?
+   - In the section above where we implemented our `ModelForm` classes, we repeated the `max_length` values for fields that we had previously defined in the models chapter. This is bad practice as we are *repeating ourselves*! How can you refactor your code so that you are *not* repeating the same `max_length` values?
+   - If you have not done so already undertake part four of the official Django Tutorial¹⁰ to reinforce what you have learnt here.
+   - Now, implement functionality to let users add pages to each category. See below for the instructions (and hints!).

# Creating an *Add Pages* View, Template and URL Mapping

A next logical step would be to allow users to add pages to a given category. To do this, repeat the same workflow above, but for adding pages.

+   - Create a new view, `add_page()`.
+   - Create a new template, `rango/add_page.html`.
+   - Create a mapping between `/rango/category/<category_name_slug>/add_page/` and the new view.
+   - Update the category page/view to provide a link from the category add page functionality.

To get you started, here is the code for the `add_page()` view function.

¹⁰ https://docs.djangoproject.com/en/2.1/intro/tutorial04/