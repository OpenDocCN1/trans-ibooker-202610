up your coding tasks, you can use the cookiecutter Python module to create these files and folders automatically. You'll find the full documentation for both the module and the Cookiecutter command line program at https://cookiecutter.readthedocs.io/.

To install Cookiecutter, run pip install --user cookiecutter (on Windows) or pip3 install --user cookiecutter (on macOS and Linux). This installation includes the Cookiecutter command line program and the cookiecutter Python module. The output might warn you that the command line program is installed to a folder not listed in the PATH environment variable:

```
Installing collected packages: cookiecutter
    WARNING: The script cookiecutter.exe is installed in 'C:\Users\Al\AppData\Roaming\Python\Python38\Scripts' which is not on PATH.
    Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
```

Consider adding the folder (C:\Users\Al\AppData\Roaming\Python\Python38\Scripts in this case) to the PATH environment variable by following the instructions in “Environment Variables and PATH” on page 35. Otherwise, you’ll have to run Cookiecutter as a Python module by entering python -m cookiecutter (on Windows) or python3 -m cookiecutter (on macOS and Linux) instead of simply cookiecutter.

In this chapter, we’ll create a repo for a module named wizcoin, which handles the galleon, sickle, and knut coins of a fictional wizarding currency. The cookiecutter module uses templates to create the starting files for several different kinds of projects. Often, the template is simply a GitHub.com link. For example, from a C:\Users\Al folder, you could enter the following in a Terminal window to create a C:\Users\Al\wizcoin folder with the boilerplate files for a basic Python project. The cookiecutter module downloads the template from GitHub and asks you a series of questions about the project you want to create:

```
C:\Users\Al>cookiecutter gh:asweigart/cookiecutter-basicpythonproject
project_name [Basic Python Project]: WizCoin
module_name [basicpythonproject]: wizcoin
author_name [Susie Softwaredeveloper]: Al Sweigart
author_email [susie@example.com]: al@inventwithpython.com
github_username [susieexample]: asweigart
project_version [0.1.0]:
project_short_description [A basic Python project.]: A Python module to represent the galleon, sickle, and knut coins of wizard currency.
```

If you get an error, you can also run python -m cookiecutter instead of cookiecutter. This command downloads a template I’ve created from https://github.com/asweigart/cookiecutter-basicpythonproject. You’ll find templates for many programming languages at https://github.com/cookiecutter/cookiecutter. Because Cookiecutter templates are often hosted on GitHub, you could also enter gh: as a shortcut for https://github.com/ in the command line argument.

As Cookiecutter asks you questions, you can either enter a response or simply press ENTER to use the default response shown in between