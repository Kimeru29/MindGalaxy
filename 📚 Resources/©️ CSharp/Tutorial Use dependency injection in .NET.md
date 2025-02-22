---
title: Use dependency injection
source: https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection-usage
author:
  - Microsoft
published: 2024-07-20
created: 2025-02-22
description: Official DI docs 2/4
tags:
  - clippings
  - csharp
  - dotnet
  - di
---
## Tutorial: Use dependency injection in .NET

- Article
- 07/20/2024

## In this article

1. [Prerequisites](https://learn.microsoft.com/en-us/dotnet/core/extensions/#prerequisites)
2. [Create a new console application](https://learn.microsoft.com/en-us/dotnet/core/extensions/#create-a-new-console-application)
3. [Add interfaces](https://learn.microsoft.com/en-us/dotnet/core/extensions/#add-interfaces)
4. [Add default implementations](https://learn.microsoft.com/en-us/dotnet/core/extensions/#add-default-implementations)
5. [Add a service that requires DI](https://learn.microsoft.com/en-us/dotnet/core/extensions/#add-a-service-that-requires-di)
6. [Register services for DI](https://learn.microsoft.com/en-us/dotnet/core/extensions/#register-services-for-di)
7. [Conclusion](https://learn.microsoft.com/en-us/dotnet/core/extensions/#conclusion)
8. [See also](https://learn.microsoft.com/en-us/dotnet/core/extensions/#see-also)

This tutorial shows how to use [dependency injection (DI) in .NET](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection). With *Microsoft Extensions*, DI is managed by adding services and configuring them in an [IServiceCollection](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.iservicecollection). The [IHost](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.hosting.ihost) interface exposes the [IServiceProvider](https://learn.microsoft.com/en-us/dotnet/api/system.iserviceprovider) instance, which acts as a container of all the registered services.

In this tutorial, you learn how to:

- Create a .NET console app that uses dependency injection
- Build and configure a [Generic Host](https://learn.microsoft.com/en-us/dotnet/core/extensions/generic-host)
- Write several interfaces and corresponding implementations
- Use service lifetime and scoping for DI## Prerequisites

- [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet) or later.
- Familiarity with creating new .NET applications and installing NuGet packages.

## Create a new console application

Using either the [dotnet new](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-new) command or an IDE new project wizard, create a new .NET console application named **ConsoleDI.Example**. Add the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) NuGet package to the project.

Your new console app project file should resemble the following:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net8.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>true</ImplicitUsings>
    <RootNamespace>ConsoleDI.Example</RootNamespace>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.Extensions.Configuration.Binder" Version="9.0.2" />
    <PackageReference Include="Microsoft.Extensions.Hosting" Version="9.0.2" />
  </ItemGroup>

</Project>
```

**Important**

In this example, the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) NuGet package is required to build and run the app. Some metapackages might contain the `Microsoft.Extensions.Hosting` package, in which case an explicit package reference isn't required.

## Add interfaces

In this sample app, you'll learn how dependency injection handles service lifetime. You'll create several interfaces that represent different service lifetimes. Add the following interfaces to the project root directory:

*IReportServiceLifetime.cs*

```csharp
using Microsoft.Extensions.DependencyInjection;

namespace ConsoleDI.Example;

public interface IReportServiceLifetime
{
    Guid Id { get; }

    ServiceLifetime Lifetime { get; }
}
```

The `IReportServiceLifetime` interface defines:

- A `Guid Id` property that represents the unique identifier of the service.
- A [ServiceLifetime](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.servicelifetime) property that represents the service lifetime.

*IExampleTransientService.cs*

```csharp
using Microsoft.Extensions.DependencyInjection;

namespace ConsoleDI.Example;

public interface IExampleTransientService : IReportServiceLifetime
{
    ServiceLifetime IReportServiceLifetime.Lifetime => ServiceLifetime.Transient;
}
```

*IExampleScopedService.cs*

```csharp
using Microsoft.Extensions.DependencyInjection;

namespace ConsoleDI.Example;

public interface IExampleScopedService : IReportServiceLifetime
{
    ServiceLifetime IReportServiceLifetime.Lifetime => ServiceLifetime.Scoped;
}
```

*IExampleSingletonService.cs*

```csharp
using Microsoft.Extensions.DependencyInjection;

namespace ConsoleDI.Example;

public interface IExampleSingletonService : IReportServiceLifetime
{
    ServiceLifetime IReportServiceLifetime.Lifetime => ServiceLifetime.Singleton;
}
```

All of the subinterfaces of `IReportServiceLifetime` explicitly implement the `IReportServiceLifetime.Lifetime` with a default. For example, `IExampleTransientService` explicitly implements `IReportServiceLifetime.Lifetime` with the `ServiceLifetime.Transient` value.

## Add default implementations

The example implementations all initialize their `Id` property with the result of [Guid.NewGuid()](https://learn.microsoft.com/en-us/dotnet/api/system.guid.newguid#system-guid-newguid). Add the following default implementation classes for the various services to the project root directory:

*ExampleTransientService.cs*

```csharp
namespace ConsoleDI.Example;

internal sealed class ExampleTransientService : IExampleTransientService
{
    Guid IReportServiceLifetime.Id { get; } = Guid.NewGuid();
}
```

*ExampleScopedService.cs*

```csharp
namespace ConsoleDI.Example;

internal sealed class ExampleScopedService : IExampleScopedService
{
    Guid IReportServiceLifetime.Id { get; } = Guid.NewGuid();
}
```

*ExampleSingletonService.cs*

```csharp
namespace ConsoleDI.Example;

internal sealed class ExampleSingletonService : IExampleSingletonService
{
    Guid IReportServiceLifetime.Id { get; } = Guid.NewGuid();
}
```

Each implementation is defined as `internal sealed` and implements its corresponding interface. They're not required to be `internal` or `sealed`, however, it's common to treat implementations as `internal` to avoid leaking implementation types to external consumers. Furthermore, since each type will not be extended, it's marked as `sealed`. For example, `ExampleSingletonService` implements `IExampleSingletonService`.

## Add a service that requires DI

Add the following service lifetime reporter class, which acts as a service to the console app:

*ServiceLifetimeReporter.cs*

```csharp
namespace ConsoleDI.Example;

internal sealed class ServiceLifetimeReporter(
    IExampleTransientService transientService,
    IExampleScopedService scopedService,
    IExampleSingletonService singletonService)
{
    public void ReportServiceLifetimeDetails(string lifetimeDetails)
    {
        Console.WriteLine(lifetimeDetails);

        LogService(transientService, "Always different");
        LogService(scopedService, "Changes only with lifetime");
        LogService(singletonService, "Always the same");
    }

    private static void LogService<T>(T service, string message)
        where T : IReportServiceLifetime =>
        Console.WriteLine(
            $"    {typeof(T).Name}: {service.Id} ({message})");
}
```

The `ServiceLifetimeReporter` defines a constructor that requires each of the aforementioned service interfaces, that is, `IExampleTransientService`, `IExampleScopedService`, and `IExampleSingletonService`. The object exposes a single method that allows the consumer to report on the service with a given `lifetimeDetails` parameter. When invoked, the `ReportServiceLifetimeDetails` method logs each service's unique identifier with the service lifetime message. The log messages help to visualize the service lifetime.

## Register services for DI

Update *Program.cs* with the following code:

```csharp
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using ConsoleDI.Example;

HostApplicationBuilder builder = Host.CreateApplicationBuilder(args);

builder.Services.AddTransient<IExampleTransientService, ExampleTransientService>();
builder.Services.AddScoped<IExampleScopedService, ExampleScopedService>();
builder.Services.AddSingleton<IExampleSingletonService, ExampleSingletonService>();
builder.Services.AddTransient<ServiceLifetimeReporter>();

using IHost host = builder.Build();

ExemplifyServiceLifetime(host.Services, "Lifetime 1");
ExemplifyServiceLifetime(host.Services, "Lifetime 2");

await host.RunAsync();

static void ExemplifyServiceLifetime(IServiceProvider hostProvider, string lifetime)
{
    using IServiceScope serviceScope = hostProvider.CreateScope();
    IServiceProvider provider = serviceScope.ServiceProvider;
    ServiceLifetimeReporter logger = provider.GetRequiredService<ServiceLifetimeReporter>();
    logger.ReportServiceLifetimeDetails(
        $"{lifetime}: Call 1 to provider.GetRequiredService<ServiceLifetimeReporter>()");

    Console.WriteLine("...");

    logger = provider.GetRequiredService<ServiceLifetimeReporter>();
    logger.ReportServiceLifetimeDetails(
        $"{lifetime}: Call 2 to provider.GetRequiredService<ServiceLifetimeReporter>()");

    Console.WriteLine();
}
```

Each `services.Add{LIFETIME}<{SERVICE}>` extension method adds (and potentially configures) services. We recommend that apps follow this convention. Don't place extension methods in the [Microsoft.Extensions.DependencyInjection](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection) namespace unless you're authoring an official Microsoft package. Extension methods that are defined within the `Microsoft.Extensions.DependencyInjection` namespace:

- Are displayed in [IntelliSense](https://learn.microsoft.com/en-us/visualstudio/ide/using-intellisense) without requiring additional `using` directives.
- Reduce the number of required `using` directives in the `Program` or `Startup` classes where these extension methods are typically called.

The app:

- Creates an [IHostBuilder](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.hosting.ihostbuilder) instance with [host builder settings](https://learn.microsoft.com/en-us/dotnet/core/extensions/generic-host#host-builder-settings).
- Configures services and adds them with their corresponding service lifetime.
- Calls [Build()](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.hosting.ihostbuilder.build#microsoft-extensions-hosting-ihostbuilder-build) and assigns an instance of [IHost](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.hosting.ihost).
- Calls `ExemplifyScoping`, passing in the [IHost.Services](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.hosting.ihost.services#microsoft-extensions-hosting-ihost-services).
  
## Conclusion

In this sample app, you created several interfaces and corresponding implementations. Each of these services is uniquely identified and paired with a [ServiceLifetime](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.servicelifetime). The sample app demonstrates registering service implementations against an interface, and how to register pure classes without backing interfaces. The sample app then demonstrates how dependencies defined as constructor parameters are resolved at run time.

When you run the app, it displays output similar to the following:

```csharp
// Sample output:
// Lifetime 1: Call 1 to provider.GetRequiredService<ServiceLifetimeReporter>()
//     IExampleTransientService: d08a27fa-87d2-4a06-98d7-2773af886125 (Always different)
//     IExampleScopedService: 402c83c9-b4ed-4be1-b78c-86be1b1d908d (Changes only with lifetime)
//     IExampleSingletonService: a61f1ff4-0b14-4508-bd41-21d852484a7b (Always the same)
// ...
// Lifetime 1: Call 2 to provider.GetRequiredService<ServiceLifetimeReporter>()
//     IExampleTransientService: b43d68fb-2c7b-4a9b-8f02-fc507c164326 (Always different)
//     IExampleScopedService: 402c83c9-b4ed-4be1-b78c-86be1b1d908d (Changes only with lifetime)
//     IExampleSingletonService: a61f1ff4-0b14-4508-bd41-21d852484a7b (Always the same)
// 
// Lifetime 2: Call 1 to provider.GetRequiredService<ServiceLifetimeReporter>()
//     IExampleTransientService: f3856b59-ab3f-4bbd-876f-7bab0013d392 (Always different)
//     IExampleScopedService: bba80089-1157-4041-936d-e96d81dd9d1c (Changes only with lifetime)
//     IExampleSingletonService: a61f1ff4-0b14-4508-bd41-21d852484a7b (Always the same)
// ...
// Lifetime 2: Call 2 to provider.GetRequiredService<ServiceLifetimeReporter>()
//     IExampleTransientService: a8015c6a-08cd-4799-9ec3-2f2af9cbbfd2 (Always different)
//     IExampleScopedService: bba80089-1157-4041-936d-e96d81dd9d1c (Changes only with lifetime)
//     IExampleSingletonService: a61f1ff4-0b14-4508-bd41-21d852484a7b (Always the same)
```

From the app output, you can see that:

- Transient services are always different, a new instance is created with every retrieval of the service.
- Scoped services change only with a new scope, but are the same instance within a scope.
- Singleton services are always the same, a new instance is only created once.

## See also

- [Dependency injection guidelines](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection-guidelines)
- [Understand dependency injection basics in .NET](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection-basics)
- [Dependency injection in ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection)

