The live server requires two additional packages. These packages are used to serve the project in a production environment, where many users can be making requests at the same time.

In the same directory where *requirements.txt* is saved, make a new file called *requirements_remote.txt*. Add the following two packages to it:

# *requirements_remote.txt*

```
# Requirements for live project.
gunicorn
psycopg2
```

The `gunicorn` package responds to requests as they come in to the remote server; this takes the place of the development server we’ve been using locally. The `psycopg2` package is required to let Django manage the Postgres database that Platform.sh uses. *Postgres* is an open source database that’s extremely well suited to production apps.

# *Adding Configuration Files*

Every hosting platform requires some configuration for a project to run correctly on its servers. In this section, we’ll add three configuration files:

**.platform.app.yaml** This is the main configuration file for the project. This tells Platform.sh what kind of project we’re trying to deploy and what kinds of resources our project needs, and it includes commands for building the project on the server.

**.platform/routes.yaml** This file defines the routes to our project. When a request is received by Platform.sh, this is the configuration that helps direct these requests to our specific project.