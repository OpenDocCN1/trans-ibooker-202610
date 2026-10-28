# Waiting to Download...

Since you’re on a shared host, downloading and installing these packages will take considerably longer than doing so on your own computer. Don’t worry if you think it’s crashed – give it time!

Once installed, check if Django has been installed correctly. You can do this with the command `which django-admin.py`. You should receive output similar to the following.

```
```text
/home/rangodemo2020/.virtualenvs/rangoenv/bin/django-admin.py
```
```

# Virtual Environments on PythonAnywhere

PythonAnywhere also provides instructions on how to set up virtual environments. [Check out their Wiki documentation for more information](https://help.pythonanywhere.com/pages/VirtualEnvForNewerDjango).

# Virtual Environment Switching

Moving between virtual environments can be done pretty easily. PythonAnywhere should have this covered for you. Below, we provide you with a quick tutorial on how to switch between virtual environments.

You can launch into an existing virtual environment with the workon command. For example, load up the `rangoenv` environment with the following command.

```
```shell
~ $ workon rangoenv
```
```

Here, `rangoenv` can be replaced with the name of the virtual environment you wish to use. Your prompt should then change to indicate you are working within a virtual environment. This is shown by the addition of `(rangoenv)` to your prompt.

```
```shell
(rangoenv) ~ $
```
```

You can then leave the virtual environment using the deactivate command. Your prompt should then be missing the `(rangoenv)` prefix, with an example shown below. This confirms that the environment has been successfully deactivated.