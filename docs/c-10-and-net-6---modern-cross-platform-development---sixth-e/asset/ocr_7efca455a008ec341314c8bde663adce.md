+   1. In the WorkingWithEFCore project, add two class files named Category.cs and Product.cs.
+   2. In Category.cs, define a class named Category, as shown in the following code:
+   3. In Product.cs, define a class named Product, as shown in the following code:

```
namespace Packt.Shared;

public class Category
{
}
```

```
namespace Packt.Shared;

public class Product
{
}
```

# Defining the Category and Product entity classes

The Category class, also known as an entity model, will be used to represent a row in the Categories table. This table has four columns, as shown in the following DDL:

```
CREATE TABLE Categories (
    CategoryId    INTEGER    PRIMARY KEY,
    CategoryName  NVARCHAR (15) NOT NULL,
    Description   "NTEXT",
    Picture       "IMAGE"
);
```

We will use conventions to define:

+   - Three of the four properties (we will not map the Picture column).
+   - The primary key.
+   - The one-to-many relationship to the Products table.

To map the Description column to the correct database type, we will need to decorate the string property with the Column attribute.

Later in this chapter, we will use the Fluent API to define that CategoryName cannot be null and is limited to a maximum of 15 characters.

Let's go:

+   1. Modify the Category entity model class, as shown in the following code:

```
using System.ComponentModel.DataAnnotations.Schema; // [Column]

namespace Packt.Shared;
```