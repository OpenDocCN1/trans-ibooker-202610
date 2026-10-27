```
```java
import lombok.Setter;
import lombok.ToString;

@Getter @Setter @ToString
@Entity
@Table(name="licenses")
public class License {

    @Id
    @Column(name = "license_id", nullable = false)
    private String licenseId;
    @Column(name = "description")
    private String description;
    @Column(name = "organization_id", nullable = false)
    private String organizationId;
    @Column(name = "product_name", nullable = false)
    private String productName;
    @Column(name = "license_type", nullable = false)
    private String licenseType;
    @Column(name="comment")
    private String comment;

    public License withComment(String comment){
        this.setComment(comment);
        return this;
    }
}
```
```

![](bbox=[0.349, 0.125, 0.832, 0.249])

In the listing, the License class uses several JPA annotations that will help the Spring Data framework map the data from the licenses table in the Postgres database to a Java object. The @Entity annotation lets Spring know that this Java POJO is going to be mapping objects that will hold data. The @Table annotation tells Spring/JPA what database table to map. The @Id annotation identifies the primary key for the database. Finally, each one of the columns from the database that will be mapped to individual properties is marked with a @Column attribute.

> **TIP**  If your attribute has the same name as the database column, you don’t need to add the @Column annotation.

The Spring Data and JPA framework provides your basic CRUD methods (Create, Replace, Update, Delete) for accessing a database. Table 5.3 shows some of these.

**Table 5.3  CRUD methods for Spring Data and the JPA framework**

| Method | Description |
|---|---|
| `count()` | Returns the number of entities available |
| `delete(entity)` | Deletes a given entity |
| `deleteAll()` | Deletes all entities managed by the repository |
| `deleteAll(entities)` | Deletes the given entities |