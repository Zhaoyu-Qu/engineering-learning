# Concepts
## .NET Framework and .NET as software frameworks
Both .NET Framework and .NET implement the CLI, meaning they provide a runtime environment (CLR / CoreCLR) to execute CIL and a Base Class Library (BCL) with common functionality. This is similar to the JVM plus the Java standard library in the Java ecosystem.

However, if .NET were only a runtime, we’d just call it a VM. Microsoft markets .NET as a software framework because it includes not only the runtime and the BCL, but also higher-level frameworks on top of the BCL — such as ASP.NET Core for web apps and APIs, Entity Framework Core for database ORM, WinForms / WPF / MAUI for GUI development, and Xamarin (now MAUI) for mobile apps. In addition, .NET also provides a tooling ecosystem, including the dotnet CLI for building, running, and publishing apps, the Roslyn compiler, and the NuGet package manager.

## ASP.NET vs ASP.NET Core
- ASP.NET was released with the original .NET Framework and runs only on Windows.
- ASP.NET Core is the modern, cross-platform rewrite of ASP.NET, released as part of .NET.

Both are designed for web development and provide technologies such as MVC and REST APIs.

## Common Language Infrastructure (CLI)
The Common Language Infrastructure (CLI) is an open specification that describes the runtime environment. A runtime environment that implements the CLI standard can run C#, F#, and VB.NET seamlessly. The .NET Framework, .NET, and Mono are implementations of the CLI. Code written in C#, F#, and VB.NET can interoperate since they are all ultimately compiled to Common Intermediate Language (CIL).

## Common Intermediate Language (CIL)
Common Intermediate Language (CIL), formerly called Microsoft Intermediate Language (MSIL) or simply Intermediate Language (IL), is the intermediate binary instruction set defined in the CLI specification. CIL instructions are executed by a CIL-compatible runtime environment such as the Common Language Runtime (CLR). Languages that target the CLI compile down to CIL. CIL is an object-oriented, stack-based bytecode, typically stored in an assembly (a DLL or EXE file). Runtimes typically just-in-time (JIT) compile CIL instructions into native machine code.

## CoreCLR
CoreCLR is the cross-platform runtime environment first introduced with .NET Core. While the brand name .NET Core has been superseded by simply .NET, the runtime environment’s name CoreCLR has been preserved. CoreCLR is distinct from the older CLR, which is still used in the .NET Framework.

## Upgrading an app to a newer version of .NET
The key step is recompiling the existing source code with the newer compiler. You may need to change the source code if certain features are removed in newer versions of .NET.

## Top-level statements
During compilation with .NET SDK 6 or later, all the boilerplate code to define the Program class and its main method is generated and wrapped around the statements you write.

Kye points to remeber about top-level programs include the following:
- With top-level statements, the auto-generated code does not define a namespace, so the Program class is implicitly defined in an empty namespace with no name, instead of a namespace that matches the name of the project.
- There can be only one file for top-level program code in a project.
- Any `using` statements must be at the top of the file.
- If you declare any classes or other types, they must be at the bottom of the file.
- Although you should name the entry-point method `Main` if you explicitly define it, the method is named `<Main>$` when created by the compiler.

## Implicitly imported namespaces
In this file, obj-Debug-net9.0-`HelloCS.GlobalUsings.g.cs`, you will find that some commonly used namespaces have been imported for use in all code files. This feature is called `global namespace imports`.

## Kestrel
Kestrel is the default HTTP server for ASP.NET Core projects.
### What is an HTTP Server?
An HTTP server is a program that listens on a network interface (IP address) and port (like :80 or :5000) for HTTP requests from clients (such as web browsers or APIs). It parses those requests, hands them off to the appropriate logic, then sends responses.
### Kestrel and Your ASP.NET Core App
When you run your ASP.NET Core application, Kestrel starts automatically (unless you configure a different server). It begins listening for incoming HTTP requests on one or more configured IP/port endpoints (e.g., http://localhost:5000). When Kestrel receives an HTTP request, it parses it (HTTP method, URL, headers, etc.) and passes the request into the middleware pipeline of your ASP.NET Core app. The routing middleware matches the request to the correct handler (e.g., a controller method). Your code executes, processes the request, and generates a response. Kestrel picks up the response and sends it over the network back to the client.

# Versions and History
## .NET Framework (2002 - present for legacy apps)
The .NET Framework is a software framework that runs primarily on Microsoft Windows. It is an implementation of the CLI and has been superseded by the cross-platform .NET. The .NET Framework is still supported for legacy applications.
## .NET Core
The early cross-platform version of .NET. It introduced CoreCLR as its runtime. From version 5 onward, it was rebranded simply as .NET, but CoreCLR continues to be used under the hood.
## .NET
The modern .NET platform (formerly .NET Core) is a free, open-source, managed software framework for Windows, Linux, and macOS. It is the cross-platform successor to the .NET Framework.

## Middleware and Request Pipeline

In ASP.NET Core, middleware and the request pipeline reside on the server side—inside your application, between the point where an HTTP request is received and before a response is sent back to the client. Here’s how it works:

1. When a client sends an HTTP request to your API, the request enters your application’s HTTP request pipeline.
2. The pipeline is made up of middleware components, each of which can inspect, modify, or short-circuit the request/response.
3. Middleware can handle tasks like authentication, logging, error handling, header propagation, etc.
4. After passing through all middleware, the request reaches your controller/action (your API logic).
5. The response generated by your controller then travels back through the middleware pipeline (in reverse order) before being sent to the client.

## Header Propagation

Header Propagation middleware is a feature in ASP.NET Core that automatically copies (or "propagates") specific HTTP headers from incoming requests to outgoing HTTP requests made by your application.

**Why is this useful?**

- In microservices or distributed systems, you often want to forward headers like correlation IDs, authentication tokens, or custom headers from the original client request to downstream services.
- Instead of manually copying these headers for every outgoing `HttpClient` call, the middleware does it for you.

**How does it work?**

1. You configure which headers to propagate in your Startup/Program configuration.
2. You add `app.UseHeaderPropagation()` to your middleware pipeline.
3. When your app receives a request, the middleware captures the specified headers.
4. When you make an outgoing HTTP request (e.g., via `HttpClient`), the headers are automatically added to that request.

In summary: Header Propagation middleware helps ensure important headers are consistently forwarded between services, improving traceability and context sharing in distributed systems.

# .NET Project Structure
## Solution
A solution (.sln file) is a container that organises one or more projects together in Visual Studio or similar IDEs. It is not part of .NET itself. If you build a .NET app from the command line using the .NET CLI, you don't need a `.sln` file at all.  

The solution keeps track of projects, build configurations, dependencies, settings and everything related to your application (or group of applications).  

A solution is always the top-level container. If your app is simple, the solution may only have one project. In more complex apps, the solution manages multiple projects (web app, class libraries, test projects, etc.).

## Project
A project (.csproj) is a buildable unit. Each project procudes something:
- An executable (.exe), e.g., a web app or desktop app
- a library (.dll), e.g., business logic, data access
- a test project, e.g., unit tests  

Unlike a solution which is specific to Visual Studio and similar IDES, the concept of a project is part of .NET itself. It tells the .NET build system which source files to compile, which NuGet packages to reference, and what the output should be (DLL, EXE, etc.). Even when using the .NET CLI, you are always working with a `.csproj`.

## Namespace
A `namespace` is part of the C# language. It's a way to organise and group classes, interfaces, enums, etc. `namespaces` are about logical grouping inside code, thus they don't have to follow project/solution structure. Although, most teams keep them aligned for clarity.

## Subdirectories
By default, the `.csproj` file uses wildcards to include all `.cs` files under its folder, no matter the depth. The compiler doesn't care if you put everything in the root folder or split into hundreds of subfolders. However, it does care about namespaces. The compiler uses namespaces to avoid naming conflicts and help you control visibility with `using` statements.

### Content of a Project Directory
- The `obj` directory contains one compiled object file for each source code file. These objects haven't been linked together into a final executable yet.
- The `bin` folder contains the binary executable for the application for class library.
  - `bin/Debug/net10.0` When you build your project in Debug mode (the default for development), the output goes to bin/Debug/net10.0/. When you build in Release mode, it goes to bin/Release/net10.0/. Both contain the final IL code, but the Debug build is intended for development and testing, while Release is for deployment. In .NET, the output directory (like bin/Debug/net10.0/) contains all the compiled assemblies (IL code), dependencies, and content files (like appsettings.json) at the same directory level—no subdirectories by default.
- The `.csproj` file is an MSBuild project file for a C# project. It's written in XML and tells the .NET build system (MSBuild) how to build your project.
- The `.cs` files are C# source code files.

### appsettings.json
All of the application's settings are contained in a file named `appsettings`.json. 
In ASP.NET Core, by default:
- appsettings.json is always loaded first.
- Then, if an environment (like Development, Staging, Production) is set (via ASPNETCORE_ENVIRONMENT), the corresponding appsettings.{Environment}.json (e.g., appsettings.Development.json) is loaded next and overrides any matching settings from appsettings.json.
- Additional files can be loaded as configured in Program.cs or Startup.cs.

# Data Structures
In C#, everything you can declare in code is a "type". That includes:
- class - reference type, always on the heap
- struct - value type, usually on the stack or inline in arrays
- interface
- enum
- delegate
- record (C# 9+)

Note in C#, there are no true primitives. What appear to be primitive types are only aliases for structs. (e.g., int is System.Int32).

# QuickStart
## Building console apps using VS Code
1. `dotnet new sln --name SolutionName`. This creates a solution. The default name is the name of the folder, if not specified.
2. `dotnet new console --output HelloCS -f net9.0`. This creates a project for a console app named `HellowCS`. If not specified, the project by default targets the latest .NET available on your computer.
3. `dotnet sln add HelloCS`. This links the project to the solution. The command recognises the only solution available in the current directory.
4. `cd HelloCS && dotnet run` executes the program.


# Techniques
## Uninstalling .NET
`dotnet --info` and then `sudo rm -rf /usr/local/share/dotnet/shared/Microsoft.AspNetCore.App/8.0.11`. In macOS, deleting the directory of an application is considered a clean uninstallation.