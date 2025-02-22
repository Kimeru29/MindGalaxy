---
title: Understanding dependency injection basics in .NET
source: https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection-basics
author:
  - Microsoft
published: 2025-01-22
created: 2025-02-22
description: Official DI docs 2/4
tags:
  - clippings
  - dotnet
  - csharp
  - di
---
## Understand dependency injection basics in .NET

- Article
- 01/22/2025

## In this article

1. [Get started](https://learn.microsoft.com/en-us/dotnet/core/extensions/#get-started)
2. [Dependency injection basics](https://learn.microsoft.com/en-us/dotnet/core/extensions/#dependency-injection-basics)
3. [Create example services](https://learn.microsoft.com/en-us/dotnet/core/extensions/#create-example-services)
4. [Update the Program class](https://learn.microsoft.com/en-us/dotnet/core/extensions/#update-the-program-class)
5. [Run the sample app](https://learn.microsoft.com/en-us/dotnet/core/extensions/#run-the-sample-app)
6. [See also](https://learn.microsoft.com/en-us/dotnet/core/extensions/#see-also)

In this article, you create a .NET console app that manually creates a [ServiceCollection](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.servicecollection) and corresponding [ServiceProvider](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovider). You learn how to register services and resolve them using dependency injection (DI). This article uses the [Microsoft.Extensions.DependencyInjection](https://www.nuget.org/packages/Microsoft.Extensions.DependencyInjection) NuGet package to demonstrate the basics of DI in .NET.

 **Note**
This article doesn't take advantage of the [Generic Host](https://learn.microsoft.com/en-us/dotnet/core/extensions/generic-host) features. For a more comprehensive guide, see [Use dependency injection in .NET](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection-usage).
## Get started

To get started, create a new .NET console application named **DI.Basics**. Some of the most common approaches for creating a console project are referenced in the following list:

- [Visual Studio: **File > New > Project**](https://learn.microsoft.com/en-us/visualstudio/get-started/csharp/tutorial-console) menu.
- [Visual Studio Code](https://code.visualstudio.com/) and the [C# Dev Kit extension's](https://code.visualstudio.com/docs/csharp/project-management): **Solution Explorer** menu option.
- [.NET CLI: `dotnet new console`](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-new-sdk-templates#console) command in the terminal.

You need to add the package reference to the [Microsoft.Extensions.DependencyInjection](https://www.nuget.org/packages/Microsoft.Extensions.DependencyInjection) in the project file. Regardless of the approach, ensure the project resembles the following XML of the *DI.Basics.csproj* file:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net8.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.Extensions.DependencyInjection" Version="9.0.2" />
  </ItemGroup>

</Project>
```
## Dependency injection basics

Dependency injection is a design pattern that allows you to remove hard-coded dependencies and make your application more maintainable and testable. DI is a technique for achieving [Inversion of Control (IoC)](https://learn.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/architectural-principles#dependency-inversion) between classes and their dependencies.

The abstractions for DI in .NET are defined in the [Microsoft.Extensions.DependencyInjection.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.DependencyInjection.Abstractions) NuGet package:

- [IServiceCollection](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection): Defines a contract for a collection of service descriptors.
- [IServiceProvider](https://learn.microsoft.com/en-us/dotnet/api/system.iserviceprovider): Defines a mechanism for retrieving a service object.
- [ServiceDescriptor](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor): Describes a service with its service type, implementation, and lifetime.

In .NET, DI is managed by adding services and configuring them in an `IServiceCollection`. After services are registered, an `IServiceProvider` instance is built by calling the [BuildServiceProvider](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) method. The `IServiceProvider` acts as a container of all the registered services, and it's used to resolve services.## Create example services

Not all services are created equally. Some services require a new instance each time that the service container gets them (*transient*), while others should be shared across requests (*scoped*) or for the entire lifetime of the app (*singleton*). For more information on service lifetimes, see [Service lifetimes](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection#service-lifetimes).

Likewise, some services only expose a concrete type, while others are expressed as a contract between an interface and an implementation type. You create several variations of services to help demonstrate these concepts.

Create a new C# file named *IConsole.cs* and add the following code:

```csharp
public interface IConsole
{
    void WriteLine(string message);
}
```

This file defines an `IConsole` interface that exposes a single method, `WriteLine`. Next, create a new C# file named *DefaultConsole.cs* and add the following code:

```csharp
internal sealed class DefaultConsole : IConsole
{
    public bool IsEnabled { get; set; } = true;

    void IConsole.WriteLine(string message)
    {
        if (IsEnabled is false)
        {
            return;
        }

        Console.WriteLine(message);
    }
}
```

The preceding code represents the default implementation of the `IConsole` interface. The `WriteLine` method conditionally writes to the console based on the `IsEnabled` property.

**Tip**

The naming of an implementation is a choice that your dev-team should agree on. The `Default` prefix is a common convention to indicate a *default* implementation of an interface, but it's *not* required.

Next, create an *IGreetingService.cs* file and add the following C# code:

```csharp
public interface IGreetingService
{
    string Greet(string name);
}
```

Then add a new C# file named *DefaultGreetingService.cs* and add the following code:

```csharp
internal sealed class DefaultGreetingService(
    IConsole console) : IGreetingService
{
    public string Greet(string name)
    {
        var greeting = $"Hello, {name}!";

        console.WriteLine(greeting);

        return greeting;
    }
}
```

The preceding code represents the default implementation of the `IGreetingService` interface. The service implementation requires an `IConsole` as a primary constructor parameter. The `Greet` method:

- Creates a `greeting` given the `name`.
- Calls the `WriteLine` method on the `IConsole` instance.
- Returns the `greeting` to the caller.

The last service to create is the *FarewellService.cs* file, add the following C# code before continuing:

```csharp
public class FarewellService(IConsole console)
{
    public string SayGoodbye(string name)
    {
        var farewell = $"Goodbye, {name}!";

        console.WriteLine(farewell);

        return farewell;
    }
}
```

The `FarewellService` represents a concrete type, not an interface. It should be declared as `public` to make it accessible to consumers. Unlike other service implementation types that were declared as `internal` and `sealed`, this code demonstrates that not all services need to be interfaces. It also shows that service implementations can be `sealed` to prevent inheritance and `internal` to restrict access to the assembly.## Update the `Program` class

Open the *Program.cs* file and replace the existing code with the following C# code:

```csharp
using Microsoft.Extensions.DependencyInjection;

// 1. Create the service collection.
var services = new ServiceCollection();

// 2. Register (add and configure) the services.
services.AddSingleton<IConsole>(
    implementationFactory: static _ => new DefaultConsole
    {
        IsEnabled = true
    });
services.AddSingleton<IGreetingService, DefaultGreetingService>();
services.AddSingleton<FarewellService>();

// 3. Build the service provider from the service collection.
var serviceProvider = services.BuildServiceProvider();

// 4. Resolve the services that you need.
var greetingService = serviceProvider.GetRequiredService<IGreetingService>();
var farewellService = serviceProvider.GetRequiredService<FarewellService>();

// 5. Use the services
var greeting = greetingService.Greet("David");
var farewell = farewellService.SayGoodbye("David");
```

The preceding updated code demonstrates the how-to:

- Create a new `ServiceCollection` instance.
- Register and configure services in the `ServiceCollection`:
- The `IConsole` using the implementation factory overload, return a `DefaultConsole` type with the `IsEnabled` set to `true`.
- The `IGreetingService` is added with a corresponding implementation type of `DefaultGreetingService` type.
- The `FarewellService` is added as a concrete type.
- Build the `ServiceProvider` from the `ServiceCollection`.
- Resolve the `IGreetingService` and `FarewellService` services.
- Use the resolved services to greet and say goodbye to a person named `David`.

If you update the `IsEnabled` property of the `DefaultConsole` to `false`, the `Greet` and `SayGoodbye` methods omit writing to the resulting messages to console. A change like this, helps to demonstrate that the `IConsole` service is *injected* into the `IGreetingService` and `FarewellService` services as a *dependency* that influences that apps behavior.

All of these services are registered as singletons, although for this sample, it works identically if they were registered as *transient* or *scoped* services.

**Important**

In this contrived example, the service lifetimes don't matter, but in a real-world application, you should carefully consider the lifetime of each service.## Run the sample app

To run the sample app, either press F5 in Visual Studio, Visual Studio Code, or run the `dotnet run` command in the terminal. When the app completes, you should see the following output:

```console
Hello, David!
Goodbye, David!
```
### Service descriptors

The most commonly used APIs for adding services to the `ServiceCollection` are lifetime-named generic extension methods, such as:

- `AddSingleton<TService>`
- `AddTransient<TService>`
- `AddScoped<TService>`

These methods are convenience methods that create a [ServiceDescriptor](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.servicedescriptor) instance and add it to the `ServiceCollection`. The `ServiceDescriptor` is a simple class that describes a service with its service type, implementation type, and lifetime. It can also describe implementation factories and instances.

For each of the services that you registered in the `ServiceCollection`, you could instead call the `Add` method with a `ServiceDescriptor` instance directly. Consider the following examples:

```csharp
services.Add(ServiceDescriptor.Describe(
    serviceType: typeof(IConsole),
    implementationFactory: static _ => new DefaultConsole
    {
        IsEnabled = true
    },
    lifetime: ServiceLifetime.Singleton));
```

The preceding code is equivalent to how the `IConsole` service was registered in the `ServiceCollection`. The `Add` method is used to add a `ServiceDescriptor` instance that describes the `IConsole` service. The static method `ServiceDescriptor.Describe` delegates to various `ServiceDescriptor` constructors. Consider the equivalent code for the `IGreetingService` service:

```csharp
services.Add(ServiceDescriptor.Describe(
    serviceType: typeof(IGreetingService),
    implementationType: typeof(DefaultGreetingService),
    lifetime: ServiceLifetime.Singleton));
```

The preceding code describes the `IGreetingService` service with its service type, implementation type, and lifetime. Finally, consider the equivalent code for the `FarewellService` service:

```csharp
services.Add(ServiceDescriptor.Describe(
    serviceType: typeof(FarewellService),
    implementationType: typeof(FarewellService),
    lifetime: ServiceLifetime.Singleton));
```

The preceding code describes the concrete `FarewellService` type as both the service and implementation types. The service is registered as a singleton service.## See also

- [.NET dependency injection](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection)
- [Dependency injection guidelines](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection-guidelines)
- [Dependency injection in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection)

Collaborate with us on GitHub

The source for this content can be found on GitHub, where you can also create and review issues and pull requests. For more information, see [our contributor guide](https://learn.microsoft.com/contribute/content/dotnet/dotnet-contribute).