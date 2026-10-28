
## 1.4 Rango: Initial Design and Specification

The focus of this book will be to develop an application called *Rango*. As we develop this application, it will cover the core components that need to be developed when building any web application. To see a fully-functional version of the application, you can visit our [How to Tango with Django website](http://www.tangowithdjango.com/)¹⁹.

### Design Brief

Let’s imagine that we would like to create a website called *Rango* that lets users browse through user-defined categories to access various web pages. In [Spanish, the word rango](https://www.vocabulary.com/dictionary/es/rango)²⁰ is used to mean “*a league ranked by quality*” or “*a position in a social hierarchy*” – so we can imagine that at some point, we will want to rank the web pages in Rango.

- For the **main page** of the Rango website, your client would like visitors to be able to see:
  - *the five most viewed pages;*
  - *the five most viewed (or rango’ed) categories;* and
  - *some way for visitors to browse and/or search through categories.*
- When a user views a **category page**, your client would like Rango to display:
  - *the category name, the number of visits, the number of likes,* along with the list of associated pages in that category (showing the page’s title, and linking to its URL); and
  - *some search functionality (via the search API)* to find other pages that can be linked to this category.
- For a **particular category**, the client would like: *the name of the category to be recorded;* the *number of times each category page has been visited;* and how many users have *clicked a “like” button* (i.e. the page gets rango’ed, and voted up the social hierarchy).
- *Each category should be accessible via a readable URL* – for example, `/rango/books-about-django/`.
- Only *registered users* will be able to search and add pages to categories. Therefore, visitors to the site should be able to register for an account.

At first glance, the specified application to develop seems reasonably straightforward. In essence, it is just a list of categories that link to pages. However, there are several complexities and challenges that need to be addressed. First, let’s try and build up a better picture of what needs to be developed by laying down some high-level designs.

### Exercises

Before going any further, think about these specifications and draw up the following design artefacts.

- What is the high-level architecture going to be? Draw up a **N-Tier** or **System Architecture** diagram to represent the high-level system components.
- What is the interface going to look like? Draw up some **Wireframes** of the main and category pages.
- What are the URLs that users visit going to look like? Write down a series of **URL mappings** for the application.
- What data are we going to have to store or represent? Construct an **Entity-Relationship (ER)**²¹ diagram to describe the data model that we’ll be implementing.

Try these exercises out before moving on – even if you aren’t familiar with system architecture diagrams, wireframes or ER diagrams, how would you explain and describe, formally, what you are going to build so that someone else can understand it.

### Wireframes

Wireframes are a great way to provide clients with some idea of what the application is going to look like, and what features it will provide. They can vary from hand-drawn sketches to exact mockups depending on the tools that you have at your disposal. For our Rango application, we’d like to make the index page of the site look like the screenshot below. Our category page is also shown below.

![](img/75169a27287f7c945bf2cbf7dede5251_16_0.png)

**The index page with a categories search bar on the left, also showing the top five pages and top five categories.**

![](img/75169a27287f7c945bf2cbf7dede5251_17_0.png)

### Pages and URL Mappings

From the specification, we have already identified two pages that our application will present to the user at different points in time. To access each page, we will need to describe URL mappings. Think of a URL mapping as the text a user would have to enter into a browser’s address bar to access a given page. The basic URL mappings for Rango are shown below.

- `/` or `/rango/` will point to the main `/` index page.
- `/rango/about/` will point to the about page.
- `/rango/category/<category_name>/` will point to the category page for `<category_name>`, where the category might be:
  - `games`;
  - `python-recipes`; or
  - `code-and-compilers`.

### Take Note!

Get into the habit of noting down any working assumptions that you make, just like the one-to-many relationship assumption that we assume above. You never know when they may come back to bite you later on! By noting them down, this means you can communicate it with your development team and make sure that the assumption is sensible, and that they are happy to proceed under such an assumption.

With this assumption, we can produce a series of tables that describe each entity in more detail. The tables contain information on what fields are contained within each entity. We use Django `ModelField` types to define the type of each field (i.e. `IntegerField`, `CharField`, `URLField` or `ForeignKey`). Note that in Django *primary keys* are implicit such that Django adds an id to each Model, but we will talk more about that later in the [Models and Databases chapter].

### Category Model Fields and Data Types

| Field | Data Type |
| --- | --- |
| name | `CharField` |
| views | `IntegerField` |
| likes | `IntegerField` |

### Page Model Fields and Data Types

| Field | Data Type |
| --- | --- |
| category | `ForeignKey` |
| title | `CharField` |
| url | `URLField` |
| views | `IntegerField` |

We will also have a model for the User so that they can register and login. We have not shown it here but shall introduce it later in the book when we discuss user authentication. In subsequent chapters, we will see how to instantiate these models in Django, and how we can use the built-in ORM to interact with the database.

## 1.5 Summary

These high-level design and specifications will serve as a useful reference point when building our web application. While we will be focusing on using specific technologies, these steps are common to most database-driven websites. It’s a good idea to become familiar with reading and producing such specifications and designs so that you can communicate your designs and ideas with others. Here, we will be focusing on using Django and the related technologies to implement this specification.

### Cut and Paste Coding

As you progress through the tutorial, you’ll most likely be tempted to cut and paste the code from the book to your code editor. **However, it is better to type in the code.** We know that this is a hassle, but it will help you to remember the process and get a feel for the commands that you will be using again and again.

Furthermore, cutting and pasting Python code is asking for trouble. Whitespace can end up being interpreted as spaces, tabs or a mixture of spaces and tabs. This will lead to all sorts of weird errors, and not necessarily indent errors. If you do cut and paste code be wary of this. Pay particular attention to this with regards to tabs and spaces – mixing these up will likely lead to a TabError.

Most code editors will show the ‘hidden characters’, which in turn will show whether whitespace is either a tab or a space. If you have this option, turn it on. You will likely save yourself a lot of confusion.

## 2. Getting Ready to Tango

Before we start coding, it’s really important that we set your development environment up correctly so that you can *Tango with Django* with ease. You’ll need to make sure that you have all of the necessary components installed on your computer, and that they are configured correctly. This chapter outlines the six key components you’ll need to be aware of, set up and use. These are:

- the [terminal](https://en.wikipedia.org/wiki/Terminal_emulator) (on macOS or UNIX/Linux systems), or the [Command Prompt](https://en.wikipedia.org/wiki/Cmd.exe) (on Windows);
- *Python 3*, including how to code and run Python scripts;
- the Python Package Manager *pip*;
- *Virtual Environments*;
- your *Integrated Development Environment (IDE)*, if you choose to use one; and
- a *Version Control System (VCS)* called *Git*.

We also touch on the merits of *unit testing*, discussing how being able to test your implementation at different points can help keep you on track.

If you already have Python 3 and Django 2 installed on your computer and are familiar with the technologies listed above, you can skip straight ahead to the [Django Basics chapter](#). If you are not familiar with some or all of the technologies listed, we provide an overview of each below. These go hand in hand with a later [supplementary chapter](#) that provides a series of pointers on how to set the different components up, if you need help doing so.
