# Hints

+   - When updating the population script, you’ll essentially follow the same process as you went through in the previous chapter’s exercises. You will need to update the data structures for each page, and also update the code that makes use of them.
+     - Update the `python_pages`, `django_pages` and `other_pages` data structures. Each page has a `title` and `url` – they all now need a count of how many `views` they see, too.
+     - Look at how the `add_page()` function is defined in your population script. Does it allow for you to pass in a `views` count? Do you need to change anything in this function?
+     - Finally, update the line where the `add_page()` function is called. If you called the views count in the data structures `views`, and the dictionary that represents a page is called `p` in the context of where `add_page()` is called, how can you pass the views count into the function?
+     - Remember to re-run the population script so that the database is updated with your new counts.
+   - You will need to edit both the `index` view and the `index.html` template to put the most viewed (i.e. popular pages) on the index page.
+     - Instead of accessing the `Category` model, you will have to ask the `Page` model for the most viewed pages.
+     - Remember to pass the list of pages through to the context.
+     - If you are not sure about the HTML template code to use, you can draw inspiration from the `category.html` template markup. The markup that you need to write is essentially the same. However, remember a `Page url` is just that – it’s the entire URL!