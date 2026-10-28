```
pip install -r requirements.txt
pip install -r requirements_remote.txt

mkdir logs
python manage.py collectstatic
rm -rf logs
deploy: |
    python manage.py migrate
```

The mounts section ❶ lets us define directories where we can read and write data while the project is running. This section defines a logs/ directory for the deployed project.

The hooks section ❷ defines actions that are taken at various points during the deployment process. In the build section, we install all the packages that are required to serve the project in the live environment ❸. We also run collectstatic ❹, which collects all the static files needed for the project into one place so they can be served efficiently.

Finally, in the deploy section ❺, we specify that migrations should be run each time the project is deployed. In a simple project, this will have no effect when there have been no changes.

The other two configuration files are much shorter; let’s write them now.

# ## The routes.yaml Configuration File

A route is the path a request takes as it’s processed by the server. When a request is received by Platform.sh, it needs to know where to send the request.

Make a new folder called .platform, in the same directory as manage.py. Make sure you include the dot at the beginning of the name. Inside that folder, make a file called routes.yaml and enter the following:

**.platform/routes.yaml**