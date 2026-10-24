
appsettings.json
LoyaltyProgram.csproj
Program.cs
Startup.cs
EventFeed
EventFeedController.cs
EventStore.cs
Users
LoyaltyProgramSettings.cs
LoyaltyProgramUser.cs
UsersController.cs
LoyaltyProgramUnitTests
LoyaltyProgramUnitTests.csproj
UnitTest1.cs

You can now run the tiny test in UnitTest1.cs with dotnet like this:

```
PS> dotnet test
```

Once you have the initial tests running, add a dependency on Microsoft.AspNetCore.TestHost so you can use the TestServer and HttpClient. Also add a dependency on LoyaltyProgram so you can begin testing it. The dependencies in the LoyaltyProgramUnitTests.csproj now look like this:

```
<ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.TestHost" Version="3.1.8"/>
    <PackageReference Include="Microsoft.NET.Test.Sdk" Version="16.5.0" />
    <PackageReference Include="xunit" Version="2.4.0" />
    <PackageReference Include="xunit.runner.visualstudio" Version="2.4.0" />
    <PackageReference Include="coverlet.collector" Version="1.2.0" />
</ItemGroup>

<ItemGroup>
    <ProjectReference Include="..\LoyaltyProgram\LoyaltyProgram.csproj" />
</ItemGroup>
```

The last line is the reference to the LoyaltyProgram project. Now we are ready to start writing tests.

## 8.3.2 Using the TestServer and HttpClient to unit-test endpoints

Now that you have a test project set up, you can begin adding tests to it. The first test you’ll add is very simple: given that there are no registered users in the loyalty program microservice, a user expects to get back a response with a “404 Not Found” status code. Add a file called UsersEndpoints_should.cs to the LoyaltyProgramUnit-Tests project and put the following code in it.

**Listing 8.3 First test for the users endpoint**
```csharp
namespace LoyaltyProgramUnitTests
{
```
```
using System;
using System.Net;
using System.Net.Http;
using System.Threading.Tasks;
using LoyaltyProgram;
using Microsoft.AspNetCore.Hosting;
using Microsoft.AspNetCore.TestHost;
using Microsoft.Extensions.Hosting;
using Xunit;

public class UsersEndpoints_should : IDisposable
{
    private readonly IHost host;
    private readonly HttpClient sut;    // Remember that sut stands for "system under test."

    public UsersEndpoints_should()
    {
        this.host = new HostBuilder()
            .ConfigureWebHost(x => x
                .UseStartup<Startup>()    // Real LoyaltyProgram startup
                .UseTestServer())         // Use the test server so requests are in process.
            .Start();
        this.sut = this.host.GetTestClient();   // The host uses the test server to create a test HttpClient.
    }

    [Fact]
    public async Task respond_not_found_when_queried_for_unregistered_user()
    {
        var actual = await this.sut.GetAsync("/users/1000");
        Assert.Equal(HttpStatusCode.NotFound, actual.StatusCode);
    }

    public void Dispose()
    {
        this.host?.Dispose();
        this.sut?.Dispose();
    }
}
```

The most interesting part of this test class is in the constructor, where you create a host and an HttpClient object. When xUnit runs, it creates an instance of UsersEndpoints_should and then calls a method with the Fact attribute on that instance. Unlike most other .NET test frameworks, xUnit creates a new, clean instance for each Fact method.

The host object in the previous listing is initialized with the real Startup class from LoyaltyProgram. This means the LoyaltyProgram application that the HttpClient calls into is wired up the same way it is when it runs on top of a real web server and receives real HTTP requests.

Let’s move on to a test that registers a new user and then queries it to check that it was registered as it should be. Add the following test to the UsersEndpoints_should class.

# Listing 8.4 Test for registering a user through the users endpoint

```
[Fact]
public async Task allow_to_register_new_user()
{
    var expected = new LoyaltyProgramUser(
        0,
        "Christian",
        0,
        new LoyaltyProgramSettings());

    var registrationResponse =
        await this.sut.PostAsync(
            "/users",
            new StringContent(
                JsonSerializer.Serialize(expected),
                Encoding.UTF8,
                "application/json"));
    var newUser =
        await JsonSerializer.DeserializeAsync<LoyaltyProgramUser>(
            await registrationResponse.Content.ReadAsStreamAsync(),
            new JsonSerializerOptions
            {
                PropertyNameCaseInsensitive = true
            });

    var actual = await this.sut.GetAsync($"/users/{newUser?.Id}");
    var actualUser =
        JsonSerializer.Deserialize<LoyaltyProgramUser>(
            await actual.Content.ReadAsStringAsync(),
            new JsonSerializerOptions
            {
                PropertyNameCaseInsensitive = true
            });

    Assert.Equal(HttpStatusCode.OK, actual.StatusCode);
    Assert.Equal(expected.Name, actualUser?.Name);
}
```

Here, you see another use of the HttpClient: we add a body to the Post via the second argument.

The last test you'll add registers a user and then modifies it via the PUT endpoint in the loyalty program microservice. Add it to UsersEndpoints_should, as shown next.

# Listing 8.5 Test for modifying users through the users endpoint

```
[Fact]
public async Task allow_modifying_users()
{
    var expected = "jane";
    var user = new LoyaltyProgramUser(
        0, "Christian", 0, new LoyaltyProgramSettings());
```
```
var registrationResponse = await this.sut.PostAsync(
    "/users",
    new StringContent(
        JsonSerializer.Serialize(user),
        Encoding.UTF8,
        "application/json"));
var newUser = await
    JsonSerializer.DeserializeAsync<LoyaltyProgramUser>(
        await registrationResponse.Content.ReadAsStreamAsync(),
        new JsonSerializerOptions
        {
            PropertyNameCaseInsensitive = true
        });

var updatedUser = newUser! with {Name = expected};
var actual = await this.sut.PutAsync($"/users/{newUser.Id}",
    new StringContent(
        JsonSerializer.Serialize(updatedUser),
        Encoding.UTF8,
        "application/json"));
var actualUser = await
    JsonSerializer.DeserializeAsync<LoyaltyProgramUser>(
        await actual.Content.ReadAsStreamAsync(),
        new JsonSerializerOptions
        {
            PropertyNameCaseInsensitive = true
        });

Assert.Equal(HttpStatusCode.OK, actual.StatusCode);
Assert.Equal(expected, actualUser?.Name);
}
```

There’s nothing new in this code compared to what you’ve seen in the two previous tests, but I wanted to include it because it’s a good illustration of the kind of unit tests I think you should write for the endpoints in your microservices: unit tests that focus on the behavior the endpoints provide rather than on testing just one endpoint in isolation.

## 8.3.3 Injecting mocks into endpoints

Now that you’ve tested the endpoints in UsersController, let’s turn to testing the LoyaltyProgram event feed, a controller that depends on an IEventStore to store and read events. Here’s the IEventStore interface.

# Listing 8.6 IEventStore interface

```
public interface IEventStore
{
    Task RaiseEvent(string name, object content);
    Task<IEnumerable<EventFeedEvent>>
        GetEvents(int start, int end);
}
```

You saw an event feed in chapter 5, but I'll repeat it here, to remind you how it works.

# Listing 8.7 Event feed

```
namespace LoyaltyProgram.EventFeed
{
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc;

    [Route("/events")]
    public class EventFeedController : ControllerBase
    {
        private readonly IEventStore eventStore;

        public EventFeedController(IEventStore eventStore)
        {
            this.eventStore = eventStore;
        }

        [HttpGet("")]
        public async Task<ActionResult<EventFeedEvent[]>>
        GetEvents([FromQuery] int start, [FromQuery] int end)
        {
            if (start < 0 || end < start)
                return BadRequest();

            return
                (await this.eventStore.GetEvents(start, end))
                .ToArray();
        }
    }
}
```

Gets the start and end value from the query string

Reads events “start” through “end” from the event store

As you can see, the event feed is a controller that responds to requests to /events with the events it reads from IEventStore. We want to write a test to check whether the event feed returns exactly the event from the IEventFeed. Toward that end, we want to control which events IEventStore returns. So, we’ll create a fake implementation of IEventStore and use that in the test.

# Listing 8.8 Fake IEventStore to use in tests

```
public class FakeEventStore : IEventStore
{
    public Task RaiseEvent(string name, object content) =>
        throw new System.NotImplementedException();

    public Task<IEnumerable<EventFeedEvent>>
        GetEvents(int start, int end)
    {
        if (start > 100)
            return Task.FromResult(
```
