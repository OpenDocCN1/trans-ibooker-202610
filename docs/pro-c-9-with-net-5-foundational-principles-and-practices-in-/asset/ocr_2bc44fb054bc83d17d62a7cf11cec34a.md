```
}
}

//OrdersController.cs
using AutoLot.Api.Controllers.Base;
using AutoLot.Dal.Repos.Interfaces;
using AutoLot.Models.Entities;
using AutoLot.Services.Logging;
using Microsoft.AspNetCore.Mvc;

namespace AutoLot.Api.Controllers
{
    [Route("api/[controller]")]
    public class OrdersController : BaseCrudController<Order, OrdersController>
    {
        public OrdersController(IOrderRepo orderRepo, IAppLogging<OrdersController> logger) :
            base(orderRepo, logger)
        {
        }
    }
}
```

This completes all of the controllers, and you can use the Swagger UI to test all of the functionality. If you are going to add/update/delete records, update the RebuildDataBase value to true in the appsettings.development.json file.

```
{
...
"RebuildDataBase": true,
...
}
```

# Exception Filters

When an exception occurs in a Web API application, there isn’t an error page that gets displayed since the user is typically another application and not a human. Any information must be sent as JSON along with the HTTP status code. As discussed in Chapter 29, ASP.NET Core allows the creation of filters that run in the event of an unhandled exception. Filters can be applied globally, at the controller level, or at the action level. For this application, you are going to build an exception filter to send formatted JSON back (along with the HTTP 500) and include a stack trace if the site is running in debug mode.

> Note Filters are an extremely powerful feature of ASP.NET Core. In this chapter, we are only examining exception filters, but there are many more that can be created that can save significant time when building ASP.NET Core applications. For the full information on filters, refer to the documentation at https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/filters.