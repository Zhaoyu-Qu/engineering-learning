# Testing in Software Development
## Stub and Mock
A "stub" is a simple object with no internal logic. It returns fixed, pre-defined responses when called. Stubs are used to test how your application handles specific responses from dependencies.

A "mock" is more advanced. It can simulate complex behavior and interactions, often mimicking a real dependency. Mocks allow you to test both how your application sends requests and how it processes responses, including verifying that the correct requests were made.

### C# and .NET
`Moq` is one of the most popular mocking libraries for .NET and is widely used in unit testing. Below is an example from their documentation:

```csharp
// This creates a mock object for the interface ILoveThisLibrary.
// The mock lets you set up (arrange) how certain methods should behave.
var mock = new Mock<ILoveThisLibrary>();

// This says: "If DownloadExists("2.0.0.0") is called on this mock object, return true."
// You can use Setup for methods, properties, indexers, etc.
// Think of this as programming the mock for specific inputs and expected outputs.
mock.Setup(library => library.DownloadExists("2.0.0.0"))
    .Returns(true);

// mock.Object gives you the actual proxy object, which you can use in your code just like an ordinary implementation of ILoveThisLibrary.
// When lovable.DownloadExists("2.0.0.0") is called, it returns true because of your setup.
ILoveThisLibrary lovable = mock.Object;
bool download = lovable.DownloadExists("2.0.0.0");

// After you run your test, you can verify that a method was called the way you expect.
// Here, you're asserting that DownloadExists("2.0.0.0") was called at most once on this mock object.
// If the method is called more than once, your test will fail.
mock.Verify(library => library.DownloadExists("2.0.0.0"), Times.AtMostOnce());
```

## Logger
A logger serves a similar purpose to System.out.println for debugging, but it's much more powerful and flexible. With a logger, you can:
- Write messages to various destinations (console, files, databases, monitoring tools, etc.).
- Control the level of detail (info, warning, error, debug, etc.).
- Filter and format messages.
- Keep logs even after the application stops, making it easier to diagnose issues later.  
So, a logger is a professional, configurable way to record and manage application messages, not just for debugging but also for monitoring and auditing.

## Mock HTTP Server
### C# and .NET
- **class WireMock.Server.WireMockServer**:
  WireMockServer is a class from the WireMock.Net library. It is used to create an in-memory HTTP server for testing purposes. With WireMockServer, you can simulate (mock) HTTP APIs, define expected requests and responses, and verify how your code interacts with external services—all without needing a real server. This is very useful for integration and unit testing of code that makes HTTP calls.
  - **WireMockServer WireMockServer.Start([int? port = 0], [bool useSSL = false], [bool useHttp2 = false])**
    when you call WireMockServer.Start(), it returns an instance of WireMockServer. A WireMockServer object is a handle to a running in-memory HTTP server provided by the WireMock.Net library. This server can receive, match, and respond to HTTP requests according to rules you define in your tests. It is commonly used for mocking external HTTP APIs during integration or unit testing.

- **class Microsoft.AspNetCore.Mvc.Testing.WebApplicationFactory<TEntryPoint> where TEntryPoint : class**
  Creates an instance of WebApplicationFactory<TEntryPoint>. This factory can be used to create a TestServer instance using the MVC application defined by TEntryPoint and one or more HttpClient instances used to send HttpRequestMessage to the TestServer. The WebApplicationFactory<TEntryPoint> will find the entry point class of TEntryPoint assembly and initialize the application by calling IWebHostBuilder CreateWebHostBuilder(string [] args) on TEntryPoint.

  When you instantiate WebApplicationFactory<TEntryPoint>, you are preparing the factory, but it has not started the web application (SUT) yet. When you request a HttpClient (via CreateClient()), the factory sets up a TestServer in-memory instance of the web application, using the configuration logic you described. Only at that point is the SUT actually running and able to respond to requests.
  - **System.Net.Http.HttpClient CreateClient()**  
    CreateClient() returns an HttpClient already wired to that TestServer. You use it to simulate the caller sending requests into your System Under Test (SUT). The HttpClient is configured internally so that all requests are handled in-memory by the TestServer. There is no actual network communication, so no real IP address or port is required. You typically use a relative path when making requests; for example:
    ```
    var client = factory.CreateClient();
    var response = await client.PostAsync("/api/resource", content);
    ```
    The base address (BaseAddress) for the HttpClient defaults to http://localhost, but this is just a placeholder for forming absolute URLs. It doesn't represent a real network endpoint. So just use relative URLs or, if needed, use the default base address, e.g., http://localhost.

  - **void CustomWebApplicationFactory.ConfigureWebHost(IWebHostBuilder builder)**
    Gives a fixture an opportunity to configure the application before it gets built.

- **interface Microsoft.AspNetCore.Hosting.IWebHostBuilder**:
  - **IWebHostBuilder IWebHostBuilder.ConfigureAppConfiguration(Action<WebHostBuilderContext, IConfigurationBuilder> configureDelegate)**
    Adds a delegate for configuring the IConfigurationBuilder that will construct an IConfiguration. Example:
    ```csharp
    builder.ConfigureAppConfiguration((_, configBuilder) =>
    {
        configBuilder.AddInMemoryCollection(new Dictionary<string, string>
        {
            ["ProposalSubmission:Url"] = $"{InsurerMock.Server.Url}/Api.LenderMortgageInsurance/v1.1/",
            ["LdpPremiumQuotation:Url"] = $"{InsurerMock.Server.Url}/Api.LDPPremiumQuotation/v1/",
            ["PolicyVariation:Url"] = $"{InsurerMock.Server.Url}/Api.LdpVariation/v1/",
            ["LmiLdpEligibility:Url"] = $"{InsurerMock.Server.Url}/Api.LMILDPEligibility/v1/",
            ["MortgageInsurancePolicy:Url"] = $"{InsurerMock.Server.Url}/Api.MortgageInsurancePolicy/v1/",
            ["MIAccountClosure:Url"] = $"{InsurerMock.Server.Url}/Api.MortgageInsuranceAccountClosure/v1/",
            ["PremiumQuotation:Url"] = $"{InsurerMock.Server.Url}/Api.LenderMortgageInsurance/v1.1/"
        }!);
    });
    ```
    In the example above, "PolicyVariation:Url" means: section "PolicyVariation", key "Url". This matches how configuration sections are structured in appsettings.json:
    ```json
    "PolicyVariation": {
      "Url": "..."
    }
    ```
    The line `["PolicyVariation:Url"] = $"{InsurerMock.Server.Url}/Api.LdpVariation/v1/"` in your in-memory configuration will override the value from the `appsettings.json` file for the key `PolicyVariation:Url`.

    ASP.NET Core merges configuration sources in order, and later sources (like your in-memory collection) take precedence over earlier ones (like appsettings files). So, during your tests, the application will use the mock URL you provide instead of the real one from the appsettings file.