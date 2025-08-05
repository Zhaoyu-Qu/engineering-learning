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

# Versions and History
## .NET Framework (2002 - present for legacy apps)
The .NET Framework is a software framework that runs primarily on Microsoft Windows. It is an implementation of the CLI and has been superseded by the cross-platform .NET. The .NET Framework is still supported for legacy applications.
## .NET Core
The early cross-platform version of .NET. It introduced CoreCLR as its runtime. From version 5 onward, it was rebranded simply as .NET, but CoreCLR continues to be used under the hood.
## .NET
The modern .NET platform (formerly .NET Core) is a free, open-source, managed software framework for Windows, Linux, and macOS. It is the cross-platform successor to the .NET Framework.

# .NET Project Structure
## Solution
A solution (.sln file) is a container that organises one or more projects together in Visual Studio or similar IDEs. The solution keeps track of projects, build configurations, dependencies, settings and everything related to your application (or group of applications).  

A solution is always the top-level container. If your app is simple, the solution may only have one project. In more complex apps, the solution manages multiple projects (web app, class libraries, test projects, etc.).

## Project
Each project procudes something:
- An executable (.exe), e.g., a web app or desktop app
- a library (.dll), e.g., business logic, data access
- a test project, e.g., unit tests

### Content of a Project Directory
- The `obj` directory contains one compiled object file for each source code file. These objects haven't been linked together into a final executable yet.
- The `bin` folder contains the binary executable for the application for class library.
- The `.csproj` file is an MSBuild project file for a C# project. It's written in XML and tells the .NET build system (MSBuild) how to build your project.
- The `.cs` files are C# source code files.

# QuickStart
## Building console apps using VS Code
1. `dotnet new sln --name SolutionName`. This creates a solution. The default name is the name of the folder, if not specified.
2. `dotnet new console --output HelloCS -f net9.0`. This creates a project for a console app named `HellowCS`. If not specified, the project by default targets the latest .NET available on your computer.
3. `dotnet sln add HelloCS`. This links the project to the solution. The command recognises the only solution available in the current directory.
4. 

# Techniques
## Uninstalling .NET
`dotnet --info` and then `sudo rm -rf /usr/local/share/dotnet/shared/Microsoft.AspNetCore.App/8.0.11`. In macOS, deleting the directory of an application is considered a clean uninstallation.