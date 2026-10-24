```
public class TestController_should : IDisposable
{
    private readonly IHost host;
    private readonly HttpClient sut;

    public class TestController : ControllerBase
    {
        [HttpGet("/")]
        public OkResult Get() => Ok();
    }

    public TestController_should()
    {
        this.host = new HostBuilder()
            .ConfigureWebHost(host =>
            {
                host.ConfigureServices(x =>
                    x.AddControllersByType(typeof(TestController)))
                .Configure(x =>
                {
                    x.UseRouting()
                    .UseEndpoints(opt => opt.MapControllers())
                    .UseTestServer()
                })
                .Start();
            });
        this.sut = this.host.GetTestClient();
    }

    [Fact]
    public async Task respond_ok_to_request_to_root()
    {
        var actual = await this.sut.GetAsync("/");
        Assert.Equal(HttpStatusCode.OK, actual.StatusCode);
    }

    public void Dispose()
    {
        this.host?.Dispose();
        this.sut?.Dispose();
    }
}
```

![](bbox=[0.09, 0.35, 0.24, 0.39])

![](bbox=[0.55, 0.17, 0.68, 0.21])

![](bbox=[0.55, 0.25, 0.71, 0.29])

![](bbox=[0.71, 0.36, 0.86, 0.43])

![](bbox=[0.67, 0.49, 0.81, 0.53])

![](bbox=[0.74, 0.53, 0.86, 0.56])

![](bbox=[0.52, 0.6, 0.7, 0.64])

There are a few things to notice about this test:

+   - The test class, TestController_should, is IDisposable. The xUnit runner understands IDisposable and calls the Dispose method after running the Fact. In the dispose method, the host and sut are disposed, which makes sure that everything is cleaned up. If the host is not disposed, the test will hang until the test runner times out.
+   - In the TestController_should constructor we create a web host just like we do in the production code. This is where we can configure everything around the endpoint under test. In this test, we use a setup similar to that in the production code, except that we use the test server, which ensures that requests are done in process.