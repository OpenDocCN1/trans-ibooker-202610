# Supervisor

As we create several different projects throughout the book, the scripts we write will need to keep running even if we’re not sitting in front of the Pi waiting for events to occur. Normally when you run a Python script from a Terminal, the Python script will stop working when you close that Terminal window or session.

You can instruct a script to continue to run in the background even after the terminal from which it was started is closed. However, there’s no guarantee that such a script will always continue to run, especially if there are memory or other resource constraints. Additionally, it would be useful if we could have certain project scripts automatically start running when the Pi reboots. What we need is a supervisor that will launch those scripts, keep them running, and restart them if they should happen to stop working.
Supervisor[^22] is the solution that will do exactly what we need. We can install Supervisor using the apt install command:

```
$ sudo apt install supervisor
```

This will install the older version of Supervisor installed as a system-level application. A newer release of Supervisor can be installed via the Pip3 command but requires additional tooling to get it working reliably as a system service. Regardless of which version you choose, both will be able to manage Python 2.7 or Python 3 scripts. Confirm that the Supervisor package was deployed successfully by checking the version of the supervisord daemon that is running on your Pi

```
$ supervisord --version
3.3.5
```

We’ll explore Supervisor’s features later and configure it for our needs when we deploy Python scripts throughout the book, beginning in Chapter 4, Water