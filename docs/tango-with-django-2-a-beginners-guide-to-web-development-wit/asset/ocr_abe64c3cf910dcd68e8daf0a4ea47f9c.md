/home/rangodemo2020/.virtualenvs/rangoenv/bin/pip

To see what packages are already installed, enter `pip list`. Now we can customise the virtual environment by installing the required packages for our Rango application. Install all the required packages by issuing the following commands.

```
```bash
$ pip install django==2.1.5
$ pip install pillow==5.4.1
$ pip install django-registration-redux==2.2
$ pip install requests
$ pip install coverage
```
```

If you decided to use the [optional Bcrypt password hasher](#), you’ll also need to install the Bcrypt library.

```
```bash
$ pip install bcrypt
```
```

**Ensure that you replace the version of Django specified above with the one you are using for development.** Here we have used version 2.1.5. Remember that this book supports a range of versions, from 2.0 to 2.2. Not using the same version as your development machine could lead to some weird errors, which will be frustrating, to say the least. To ensure you have the same setup, we recommend using `pip freeze > requirements.txt` on your local development machine. This will note down all the packages in your current development environment. Then on PythonAnywhere, you can run `pip install -r requirements.txt` to install all the packages in one go.

![](bbox=[0.127, 0.696, 0.176, 0.739])

# Using `requirements.txt`?

If you went down the path of using `pip freeze` on your computer to create a `requirements.txt` file of all packages, this should have been added to your Git repository.

In this instance, [skip to cloning your repository first](#), then come back here. When you clone your repository, you gain access to your requirements files, which means you can then install all the packages to your `rangoenv` virtual environment on PythonAnywhere with the command `pip install -r requirements.txt`.