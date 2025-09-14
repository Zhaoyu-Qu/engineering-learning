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