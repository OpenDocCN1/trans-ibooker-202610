# Removing a Member

You can remove a member from an organization either by deleting the member from the UI or by removing using the CLI. When you remove a member from an organization, it doesn’t delete the user from InfluxDB; it only revokes the association of the member with the organization and all the permissions it had. You can use the *influx org members remove* command as per the syntax in the following:

```
$ influx org members remove -m <member id > -i<org id> --token <INFLUX_TOKEN>
```

When you run this command in the CLI, you can see that the specific user ID being removed from the members list, as depicted in Figure 5-29.

![](bbox=[0.07, 0.39, 0.9, 0.56])

*Figure 5-29. Removing a member from an organization through CLI*

The same thing can be achieved using UI by removing the user peter, who is a member of the organization monitoringcloudnativeapps2, by clicking the trash can, as shown in Figure 5-30.