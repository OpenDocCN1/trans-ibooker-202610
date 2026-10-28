# Deleting an Organization

There is no option to delete an organization using the UI³ and therefore the only option is to use CLI. The syntax for the command to achieve this function is mentioned in the following:

```
$ influx org delete --host <http://hostname:port> --token <INFLUX_TOKEN> -i<org-id>
```

The output of the command will look like the highlighted section in Figure 5-20 and will provide the revised organization name with the existing org-id:

![](bbox=[0.07, 0.34, 0.89, 0.43])

Figure 5-20. Deleting an organization through CLI

# Members

In InfluxDB, there is a concept of a user and a member. Users are the individuals with access to InfluxDB, and when these users are part of one or more organizations, they are referred to as the members of that organization. You can get a list of users and their user IDs by using the CLI to run the following syntax:

```
$ influx user list --host <http://hostname:port> --token <INFLUX_TOKEN>
```

When you run this command, you will get the username and associated user ID. In this case, since we haven’t created any new users yet, it will list only the admin user. See Figure 5-21.

![](bbox=[0.07, 0.75, 0.89, 0.795])

Figure 5-21. List of users through CLI