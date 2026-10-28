# Don’t hard code Paths!

The road to hell is paved with hard-coded paths. Hard-coding paths³ is a software engineering anti-pattern⁴, and will make your project less portable⁵ – meaning that when you run it on another computer, it probably won’t work! (Or it will work, just with a lot of effort!)

# Dynamic Paths

A better solution is to make use of built-in Python functions to work out the path of your templates directory automatically. This way, an absolute path can be obtained regardless of where you place your Django project’s code. This, in turn, means that your project becomes more portable.

At the top of your settings.py file, there is a variable called BASE_DIR. This variable stores the path to the directory in which your project’s settings.py module is contained. This is obtained by using the special Python __file__ attribute, which is set to the path of your settings module⁶. Using this as a parameter to os.path.abspath() guarantees the absolute path to the settings.py module. The call to os.path.dirname() then provides the reference to the absolute path of the directory containing the settings.py module. Calling os.path.dirname() again removes another directory layer, so that BASE_DIR then points to your project directory, or <workspace>/tango_with_django_project/. If you are curious, you can see how this works by adding the following lines to your settings.py file.

```
print(__file__)
print(os.path.dirname(__file__))
print(os.path.dirname(os.path.dirname(os.path.abspath(__file__))))
```

Having access to the value of BASE_DIR makes it easy for you to reference other aspects of your Django project. Using the BASE_DIR variable, we can now create a new variable called TEMPLATE_DIR that will reference your new templates directory. We can make use of the os.path.join() function to join up multiple paths, leading

³http://en.wikipedia.org/wiki/Hard_coding
⁴http://sourcemaking.com/antipatterns
⁵http://en.wikipedia.org/wiki/Software_portability
⁶http://stackoverflow.com/a/9271479