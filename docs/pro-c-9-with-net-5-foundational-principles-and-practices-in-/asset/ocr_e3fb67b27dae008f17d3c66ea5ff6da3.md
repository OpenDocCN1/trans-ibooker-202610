# Create the ServiceSettings Class

The service settings will be populated from the settings the same way the dealer information was populated. Create a new folder named ApiWrapper in the AutoLot.Services project. In this folder, add a class named ApiServiceSettings.cs. The property names of the class need to match the property names in the JSON ApiServiceSettings section. The class is listed here:

```
namespace AutoLot.Services.ApiWrapper
{
    public class ApiServiceSettings
    {
        public ApiServiceSettings() { }
        public string Uri { get; set; }
        public string CarBaseUri { get; set; }
        public string MakeBaseUri { get; set; }
    }
}
```

# The API Service Wrapper

ASP.NET Core 2.1 introduced the IHttpClientFactory, which allows configuring strongly typed classes for calling into RESTful services. Creating a strongly typed class allows for encapsulating all the API calls in one place. This centralizes communication with the service, configuration of the HTTP client, error handling, and so on. The class can then be added into the dependency injection container for use later in your application. The DI container and IHttpClientFactory handle creating and disposing of the HttpClient.

# The IApiServiceWrapper Interface

The AutoLot service wrapper interface contains the methods to call into the AutoLot.Api service. Create a new interface named IApiServiceWrapper.cs in the ApiWrapper directory, and update the using statements to the following:

```
using System.Collections.Generic;
using System.Threading.Tasks;
using AutoLot.Models.Entities;
```

Update the interface to the code shown here:

```
namespace AutoLot.Services.ApiWrapper
{
    public interface IApiServiceWrapper
    {
        Task<IList<Car>> GetCarsAsync();
        Task<IList<Car>> GetCarsByMakeAsync(int id);
        Task<Car> GetCarAsync(int id);
        Task<Car> AddCarAsync(Car entity);
        Task<Car> UpdateCarAsync(int id, Car entity);
        Task DeleteCarAsync(int id, Car entity);
        Task<IList<Make>> GetMakesAsync();
    }
}
```