# Testing in Software Development
## Stub
In software testing, a "stub" typically refers to a minimal implementation of an interface or class, used to replace a dependency and return controlled responses for testing purposes.

## Logger
A logger serves a similar purpose to System.out.println for debugging, but it's much more powerful and flexible. With a logger, you can:
- Write messages to various destinations (console, files, databases, monitoring tools, etc.).
- Control the level of detail (info, warning, error, debug, etc.).
- Filter and format messages.
- Keep logs even after the application stops, making it easier to diagnose issues later.  
So, a logger is a professional, configurable way to record and manage application messages, not just for debugging but also for monitoring and auditing.

## Mock HTTP Server
### C# and .NET
- class WireMock.Server.WireMockServer:
  WireMockServer is a class from the WireMock.Net library. It is used to create an in-memory HTTP server for testing purposes. With WireMockServer, you can simulate (mock) HTTP APIs, define expected requests and responses, and verify how your code interacts with external services—all without needing a real server. This is very useful for integration and unit testing of code that makes HTTP calls.