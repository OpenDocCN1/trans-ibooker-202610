The **flake8** module does need upgrading. In the output, you can see how pip checks packages on which **flake8** depends. If they need upgrading, pip will do so.

```
pip install --upgrade flake8
```

```
Collecting flake8
    Using cached flake8-3.9.2-py2.py3-none-any.whl (73 kB)
    Requirement already satisfied: pyflakes<2.4.0,>=2.3.0 in
        d:\python\python39\lib\site-packages (from flake8) (2.3.1)
    Requirement already satisfied: pycodestyle<2.8.0,>=2.7.0 in
        d:\python\python39\lib\site-packages (from flake8) (2.7.0)
    Requirement already satisfied: mccabe<0.7.0,>=0.6.0 in
        d:\python\python39\lib\site-packages (from flake8) (0.6.1)

    Installing collected packages: flake8
      Attempting uninstall: flake8
        Found existing installation: flake8 3.8.4
        Uninstalling flake8-3.8.4:
          Successfully uninstalled flake8-3.8.4
    Successfully installed flake8-3.9.2
```

# Uninstall a package

I don’t need **treemap** on my system, so I uninstall it:

```
pip uninstall treemap
```

```
Found existing installation: treemap 1.5
Uninstalling treemap-1.5:
  Would remove:
    d:\python\python39\lib\site-packages\treemap-1.5.dist-info\*
    d:\python\python39\lib\site-packages\treemap\*
    d:\python\python39\scripts\treemap_coverage.exe
    d:\python\python39\scripts\treemap_demo.exe
Proceed (y/n)? y
  Successfully uninstalled treemap-1.5
```

Note how **pip** prompted me to confirm if I wanted to remove **treemap**. I responded with “y” for “yes.” Had I included the --yes option to pip, it would not have asked me for confirmation.

# Update pip

Though **pip** is installed with Python, its developers update it more frequently than Python itself. You should update it regularly to have the latest version: