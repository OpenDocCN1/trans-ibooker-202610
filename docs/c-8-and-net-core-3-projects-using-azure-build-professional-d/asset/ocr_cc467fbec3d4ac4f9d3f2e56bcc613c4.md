Let's have a look under the hood. You will see that our Up and Down methods aren't as empty as they are in the InitialCreate class:

![](bbox=[0.206, 0.166, 0.793, 0.508])

As we mentioned earlier, the Up method executes during an upgrade and the Down method executes during a downgrade. This concept is a lot clearer now that we can see the code. In the Up method, we are adding the Read column and in the Down method, we are dropping the column.

We can make changes to this code if needed. We can, for example, change the nullable attribute of the Read column, by updating the code so that it looks as follows:

```
protected override void Up(MigrationBuilder migrationBuilder)
{
    migrationBuilder.AddColumn<bool>(
        name: "Read",
        table: "ResearchLinks",
        nullable: true,
        defaultValue: false);
}
```

We can also add a custom SQL query that will update all the existing entries to Read:

```
migrationBuilder.Sql(
@"
```