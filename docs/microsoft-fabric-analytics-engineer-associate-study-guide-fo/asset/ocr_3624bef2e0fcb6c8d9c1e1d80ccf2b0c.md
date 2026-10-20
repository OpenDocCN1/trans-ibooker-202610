Table 7-2. Permissions for item-level access control

| Permission granted while sharing | Effects |
|---|---|
| Read | The recipient can discover the item in the data hub, open it, and connect to the warehouse or SQL analytics endpoint of the lakehouse. |
| Edit | The recipient can edit the item or its content. |
| Share | The original recipient can share the item and grant to the new recipient some or all of the same permissions that they (the original recipient) have. For example, if the original recipient has Share, Edit, and Read permissions, then the original recipient can at most grant Share, Edit, and Read permissions to the new recipient. |
| Read All with SQL analytics endpoint | The recipient can read data from the SQL analytics endpoint of the lakehouse or warehouse data through TDS endpoints. |
| Read all with Apache Spark | The recipient can read lakehouse or data warehouse data through OneLake APIs and Spark, and they can also read lakehouse data through lakehouse explorer. |