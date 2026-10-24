It’s events of this type that you’ll store in the ShoppingCart database. The first step is to go into the ShoppingCart database and add a table like the one shown in figure 6.9. The database script in the file `Chapter06\ShoppingCart\database-scripts\create-shopping-cart-db.sql` in the code download creates this table, along with the other two tables in the ShoppingCart database.

| EventStore | |
|---|---|
| **PK** | **ID** |
|  | Name |
|  | OccuredAt |
|  | Content |

**Figure 6.9**  The **EventStore** table has four columns for these categories: event ID, event name, the time the event occurred, and the contents of the event.

Next, add a file named EventStore.cs to the shopping cart, and add to it the following code for writing events.

```
```csharp
namespace ShoppingCart.EventFeed
{
    using System;
    using System.Collections.Generic;
    using System.Data.SqlClient;
    using System.Text.Json;
    using System.Threading.Tasks;
    using Dapper;

    public interface IEventStore
    {
        Task<IEnumerable<Event>> GetEvents(long firstEventSequenceNumber,
            long lastEventSequenceNumber);
        Task Raise(string eventName, object content);
    }

    public class EventStore : IEventStore
    {
        private string connectionString =
            @"Data Source=localhost;Initial Catalog=ShoppingCart;
User Id=SA; Password=yourStrong(!)Password";

        private const string writeEventSql =
            @"insert into EventStore(Name, OccuredAt, Content)
values (@Name, @OccuredAt, @Content)";

        public async Task Raise(string eventName, object content)
        {
            var jsonContent = JsonSerializer.Serialize(content);
```
```