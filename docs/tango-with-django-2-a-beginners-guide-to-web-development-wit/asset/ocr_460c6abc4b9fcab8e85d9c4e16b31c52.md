rango/. When a match is made, the remainder of the URL string is then passed onto and handled by rango.urls through the use of the include() function from within the django.urls package.

![](bbox=[0.218, 0.19, 0.812, 0.212])

# Protocol and Domain Name

![](bbox=[0.118, 0.261, 0.548, 0.308])

![](bbox=[0.181, 0.337, 0.882, 0.525])

**An illustration of a URL, represented as a chain, showing how different parts of the URL following the domain are the responsibility of different url.py files.**

Think of this as a chain that processes the URL string – as illustrated in the [URL chain figure]. In this chain, the domain is stripped out and the remainder of the URL string (`rango/`) is passed on to `tango_with_django` project, where it finds a match and strips away `rango/`, leaving an empty string to be passed on to the app rango for it to handle.

Consequently, we need to create a new file called `urls.py` in the rango app directory, to handle the remaining URL string (and map the empty string to the index view):