```
```yaml
# Each route describes how an incoming URL will be processed
by Platform.sh.

"https://{default}/":
    type: upstream
    upstream: "ll_project:http"

"https://www.{default}/":
    type: redirect
    to: "https://{default}/"
```
```

This file makes sure requests like *https://project_url.com* and *www.project_url.com* all get routed to the same place.

# The services.yaml Configuration File

This last configuration file specifies services that our project needs in order to run. Save this file in the *.platform/* directory, alongside *routes.yaml*:

# .platform/routes.yaml

```
```yaml
# Each service listed will be deployed in its own container as part of your
# Platform.sh project.

db:
    type: postgresql:12
    disk: 1024
```
```

This file defines one service, a Postgres database.

# Modifying settings.py for Platform.sh

Now we need to add a section at the end of *settings.py* to modify some settings for the Platform.sh environment. Add this code to the very end of *settings.py*:

# settings.py

```
```python
# --snip--
# Platform.sh settings.
```
```