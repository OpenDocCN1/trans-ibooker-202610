# MORE CONFIGURATION OPTIONS

Now that you have a Helm release with a working MySQL cluster, you can point an application to it, such as Wordpress. Why not try seeing if you can adapt the Wordpress deployment from Chapter 3 to point to the MySQL cluster you’ve created here.

For further learning, you could also compare your resulting configuration with that produced by the Bitnami Wordpress Helm Chart, which uses MariaDB instead of MySQL but is otherwise quite similar.

# Updating Helm Charts

If you’re running a Helm release in a production environment, chances are you’re going to need to maintain it over time. There are several reasons why you might want to update a Helm release:

+   - A new version of a chart is available
+   - A new version of an image used by your application is available
+   - You want to change the selected options

To check for a new version of a chart, execute the helm repo update command. Running this command with no options looks for updates in all of the chart repositories you have configured for your helm client:

```
helm repo update
Hang tight while we grab the latest from your chart
repositories...
...Successfully got an update from the "bitnami" chart repository
Update Complete. *Happy Helming!*
```

Next, you’ll want to make any desired updates to your configured values. If you’re upgrading to a new version of a chart, make sure to check the release notes and documentation of the configurable values. It’s a good idea to test out an upgrade before applying it. The --dry-run option allows you to do this, producing similar values to the helm template command:

```
helm upgrade mysql bitnami/mysql -f values.yaml --
```