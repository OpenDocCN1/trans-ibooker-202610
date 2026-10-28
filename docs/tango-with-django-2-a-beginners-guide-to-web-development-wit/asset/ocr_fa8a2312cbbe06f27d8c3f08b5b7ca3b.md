# 1.4 Rango: Initial Design and Specification

The focus of this book will be to develop an application called *Rango*. As we develop this application, it will cover the core components that need to be developed when building any web application. To see a fully-functional version of the application, you can visit our [How to Tango with Django website](http://www.tangowithdjango.com/)¹⁹.

# Design Brief

Let’s imagine that we would like to create a website called *Rango* that lets users browse through user-defined categories to access various web pages. In [Spanish, the word rango](https://www.vocabulary.com/dictionary/es/rango)²⁰ is used to mean “*a league ranked by quality*” or “*a position in a social hierarchy*” – so we can imagine that at some point, we will want to rank the web pages in Rango.

+   - For the **main page** of the Rango website, your client would like visitors to be able to see:
+     - *the five most viewed pages;*
+     - *the five most viewed (or rango’ed) categories;* and
+     - *some way for visitors to browse and/or search through categories.*
+   - When a user views a **category page**, your client would like Rango to display:
+     - *the category name, the number of visits, the number of likes,* along with the list of associated pages in that category (showing the page’s title, and linking to its URL); and
+     - *some search functionality (via the search API)* to find other pages that can be linked to this category.
+   - For a **particular category**, the client would like: *the name of the category to be recorded;* the *number of times each category page has been visited;* and how many users have *clicked a “like” button* (i.e. the page gets rango’ed, and voted up the social hierarchy).
+   - *Each category should be accessible via a readable URL* – for example, `/rango/books-about-django/`.