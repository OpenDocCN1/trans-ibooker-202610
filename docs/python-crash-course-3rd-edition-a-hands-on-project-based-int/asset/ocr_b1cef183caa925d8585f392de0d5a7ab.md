out a few clear goals to keep the development process focused. Here’s the spec we’ll use:

We’ll write a web app called Learning Log that allows users to log the topics they’re interested in and make journal entries as they learn about each topic. The Learning Log home page will describe the site and invite users to either register or log in. Once logged in, a user can create new topics, add new entries, and read and edit existing entries.

When you’re researching a new topic, maintaining a journal of what you’ve learned can help you keep track of new information and information you’ve already found. This is especially true when studying technical subjects. A good app, like the one we’ll be creating, can help make this process more efficient.

# Creating a Virtual Environment

To work with Django, we’ll first set up a virtual environment. A *virtual environment* is a place on your system where you can install packages and isolate them from all other Python packages. Separating one project’s libraries from other projects is beneficial and will be necessary when we deploy Learning Log to a server in Chapter 20.

Create a new directory for your project called *learning_log*, switch to that directory in a terminal, and enter the following code to create a virtual environment:

```
learning_log$ python -m venv ll_env
learning_log$
```

Here we’re running the `venv` virtual environment module and using it to create an environment named *ll_env* (note that this name starts with two lowercase Ls, not two ones). If you use a command such as `python3` when running programs or installing packages, make sure to use that command here.