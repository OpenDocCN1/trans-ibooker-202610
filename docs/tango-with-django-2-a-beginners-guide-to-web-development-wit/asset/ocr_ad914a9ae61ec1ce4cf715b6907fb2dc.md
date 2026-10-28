```
```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```
```

What we need to do is tell Django where our templates will be stored by modifying the `DIRS` list, which is set to an empty list by default. Change the dictionary key/value pair to look like the following.

```
```python
'DIRS': ['<workspace>/tango_with_django_project/templates']
```
```

Note that you are *required to use absolute paths* to locate the templates directory. If you are collaborating with team members or working on different computers, then this will become a problem. You’ll have different usernames and different drive structures, meaning the paths to the `<workspace>` directory will be different. One solution would be to add the path for each different configuration. For example:

```
```python
'DIRS': [ '/Users/leifos/templates',
           '/Users/maxwelld90/templates',
           '/Users/davidm/templates', ]
```
```

However, there are several problems with this. First, you have to add in the path for each setting, each time. Second, if you are running the app on different operating systems the backslashes have to be constructed differently.