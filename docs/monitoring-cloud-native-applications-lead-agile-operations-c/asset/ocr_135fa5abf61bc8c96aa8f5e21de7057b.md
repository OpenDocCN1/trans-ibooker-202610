![](bbox=[0.235, 0.105, 0.8, 0.425])

Figure 5-30. Removing a member from an organization through UI

# Deleting a User

A user can be permanently deleted by using the influx user delete command through the CLI as per the following syntax⁵:

```
$ influx user delete -i<User id> --token <INFLUX_TOKEN>
```

Once executed, it will delete the user named peter, as depicted in Figure 5-31:

![](bbox=[0.105, 0.655, 0.93, 0.747])

Figure 5-31. Removing a user from InfluxDB through CLI

⁵Users cannot be deleted from the InfluxDB UI.