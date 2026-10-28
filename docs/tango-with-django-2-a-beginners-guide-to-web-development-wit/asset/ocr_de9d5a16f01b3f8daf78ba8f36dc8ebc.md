![](bbox=[0.152, 0.131, 0.851, 0.466])

The PythonAnywhere dashboard, showing the main components you can use. We’re logged in as rangodemo2020 here. You can see links to Consoles, Files, Notebooks (that we don’t use here), and web apps (or the settings for web apps).

# 19.3 Creating a Virtual Environment

As part of its standard default Bash environment, PythonAnywhere comes with Python 2.7+ and several pre-installed Python packages (including *Django*). Since we are using a different setup, we need to set up the Python 3 version manually through the creation of a virtual environment.

First, open a Bash console. By clicking `$ Bash` from under the New console header. If you run `python --version` then you will see that the Python is 2.7+. To set up our Python 3.7+ environment, we’ll need to issue the following command.