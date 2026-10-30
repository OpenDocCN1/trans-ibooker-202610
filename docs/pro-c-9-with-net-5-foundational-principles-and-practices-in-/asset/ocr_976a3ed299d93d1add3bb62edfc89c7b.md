In EF Core, the specified behavior is triggered only after an entity is deleted and SaveChanges() is called on the derived DbContext. See the “Query Execution” section for more details about when EF Core interacts with the data store.

# Optional Relationships

Recall from Table 22-4 that optional relationships are where the dependent entity can set the foreign key value(s) to null. For optional relationships, the default behavior is ClientSetNull. Table 22-5 shows the cascade behavior with dependent entities and the effect on database records when using SQL Server.

# Table 22-5. Cascade Behavior with Optional Relationships

| Delete Behavior | Effect on Dependents (In Memory) | Effect on Dependents (In Database) |
|---|---|---|
| Cascade | Entities are deleted. | Entities are deleted by the database. |
| ClientCascade | Entities are deleted. | For databases that do not support cascade delete, EF Core deletes the entities. |
| ClientSetNull (default) | Foreign key property/properties set to null. | None. |
| SetNull | Foreign key property/properties set to null. | Foreign key property/properties set to null. |
| Restrict | None. | None. |

# Required Relationships

Required relationships are where the dependent entity cannot set the foreign key value(s) to null. For required relationships, the default behavior is Cascade. Table 22-6 shows the cascade behavior with dependent entities and the effect on database records when using SQL Server.

# Table 22-6. Cascade Behavior with Required Relationships

| Delete Behavior | Effect on Dependents (In Memory) | Effect on Dependents (In Database) |
|---|---|---|
| Cascade (default) | Entities are deleted. | Entities are deleted. |
| ClientCascade | Entities are deleted. | For databases that do not support cascade delete, EF Core deletes the entities. |
| ClientSetNull | SaveChanges throws exception. | None. |
| SetNull | SaveChanges throws exception. | SaveChanges throws exception. |
| Restrict | None. | None. |