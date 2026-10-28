can either enter it by hand in your text editor or download a copy from the online resources at https://ehmatthes.github.io/pcc_3e.

Here’s the first part of .platform.app.yaml, which should be saved in the same directory as manage.py:

# .platform.app.yaml

```
① name: "ll_project"
  type: "python:3.10"

② relationships:
        database: "db:postgresql"

  # The configuration of the app when it's exposed to the web.
③ web:
        upstream:
            socket_family: unix
        commands:
④         start: "gunicorn -w 4 -b unix:$SOCKET ll_project.wsgi:application"
⑤         locations:
            "/":
                passthru: true
            "/static":
                root: "static"
                expires: 1h
                allow: true

  # The size of the persistent disk of the application (in MB).
⑥ disk: 512
```

When you save this file, make sure you include the dot at the beginning of the filename. If you omit the dot, Platform.sh won’t find the file and your project will not be deployed.

You don’t need to understand everything in .platform.app.yaml at this point; I’ll highlight the most important parts of the configuration. The file starts off by