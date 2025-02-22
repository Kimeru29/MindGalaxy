---
title: Dependency injection guidelines
source: https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection-guidelines
author:
  - Microsoft
published: 2024-07-18
created: 2025-02-22
description: Official DI docs 4/4
tags:
  - clippings
  - csharp
  - dotnet
  - di
---
## Dependency injection guidelines

- Article
- 07/18/2024

## In this article

1. [Design services for dependency injection](https://learn.microsoft.com/en-us/dotnet/core/extensions/#design-services-for-dependency-injection)
2. [Default service container replacement](https://learn.microsoft.com/en-us/dotnet/core/extensions/#default-service-container-replacement)
3. [Thread safety](https://learn.microsoft.com/en-us/dotnet/core/extensions/#thread-safety)
4. [Recommendations](https://learn.microsoft.com/en-us/dotnet/core/extensions/#recommendations)
5. [Example anti-patterns](https://learn.microsoft.com/en-us/dotnet/core/extensions/#example-anti-patterns)
6. [See also](https://learn.microsoft.com/en-us/dotnet/core/extensions/#see-also)

This article provides general guidelines and best practices for implementing dependency injection in .NET applications.## Design services for dependency injection

When designing services for dependency injection:

- Avoid stateful, static classes and members. Avoid creating global state by designing apps to use singleton services instead.
- Avoid direct instantiation of dependent classes within services. Direct instantiation couples the code to a particular implementation.
- Make services small, well-factored, and easily tested.

If a class has many injected dependencies, it might be a sign that the class has too many responsibilities and violates the [Single Responsibility Principle (SRP)](https://learn.microsoft.com/en-us/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility). Attempt to refactor the class by moving some of its responsibilities into new classes.### Disposal of services

The container is responsible for cleanup of types it creates, and calls [Dispose](https://learn.microsoft.com/en-us/dotnet/api/system.idisposable.dispose) on [IDisposable](https://learn.microsoft.com/en-us/dotnet/api/system.idisposable) instances. Services resolved from the container should never be disposed by the developer. If a type or factory is registered as a singleton, the container disposes the singleton automatically.

In the following example, the services are created by the service container and disposed automatically:

```csharp
namespace ConsoleDisposable.Example;

public sealed class TransientDisposable : IDisposable
{
    public void Dispose() => Console.WriteLine($"{nameof(TransientDisposable)}.Dispose()");
}
```

The preceding disposable is intended to have a transient lifetime.

```csharp
namespace ConsoleDisposable.Example;

public sealed class ScopedDisposable : IDisposable
{
    public void Dispose() => Console.WriteLine($"{nameof(ScopedDisposable)}.Dispose()");
}
```

The preceding disposable is intended to have a scoped lifetime.

```csharp
namespace ConsoleDisposable.Example;

public sealed class SingletonDisposable : IDisposable
{
    public void Dispose() => Console.WriteLine($"{nameof(SingletonDisposable)}.Dispose()");
}
```

The preceding disposable is intended to have a singleton lifetime.

```csharp
using ConsoleDisposable.Example;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

HostApplicationBuilder builder = Host.CreateApplicationBuilder(args);
builder.Services.AddTransient<TransientDisposable>();
builder.Services.AddScoped<ScopedDisposable>();
builder.Services.AddSingleton<SingletonDisposable>();

using IHost host = builder.Build();

ExemplifyDisposableScoping(host.Services, "Scope 1");
Console.WriteLine();

ExemplifyDisposableScoping(host.Services, "Scope 2");
Console.WriteLine();

await host.RunAsync();

static void ExemplifyDisposableScoping(IServiceProvider services, string scope)
{
    Console.WriteLine($"{scope}...");

    using IServiceScope serviceScope = services.CreateScope();
    IServiceProvider provider = serviceScope.ServiceProvider;

    _ = provider.GetRequiredService<TransientDisposable>();
    _ = provider.GetRequiredService<ScopedDisposable>();
    _ = provider.GetRequiredService<SingletonDisposable>();
}
```

The debug console shows the following sample output after running:

```console
Scope 1...
ScopedDisposable.Dispose()
TransientDisposable.Dispose()

Scope 2...
ScopedDisposable.Dispose()
TransientDisposable.Dispose()

info: Microsoft.Hosting.Lifetime[0]
      Application started.Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
     Hosting environment: Production
info: Microsoft.Hosting.Lifetime[0]
     Content root path: .\configuration\console-di-disposable\bin\Debug\net5.0
info: Microsoft.Hosting.Lifetime[0]
     Application is shutting down...
SingletonDisposable.Dispose()
```### Services not created by the service container

Consider the following code:

```csharp
// Register example service in IServiceCollection
builder.Services.AddSingleton(new ExampleService());
```

In the preceding code:

- The `ExampleService` instance is **not** created by the service container.
- The framework does **not** dispose of the services automatically.
- The developer is responsible for disposing the services.### IDisposable guidance for transient and shared instances#### Transient, limited lifetime

**Scenario**

The app requires an [IDisposable](https://learn.microsoft.com/en-us/dotnet/api/system.idisposable) instance with a transient lifetime for either of the following scenarios:

- The instance is resolved in the root scope (root container).
- The instance should be disposed before the scope ends.

**Solution**

Use the factory pattern to create an instance outside of the parent scope. In this situation, the app would generally have a `Create` method that calls the final type's constructor directly. If the final type has other dependencies, the factory can:

- Receive an [IServiceProvider](https://learn.microsoft.com/en-us/dotnet/api/system.iserviceprovider) in its constructor.
- Use [ActivatorUtilities.CreateInstance](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.activatorutilities.createinstance) to instantiate the instance outside of the container, while using the container for its dependencies.#### Shared instance, limited lifetime

**Scenario**

The app requires a shared [IDisposable](https://learn.microsoft.com/en-us/dotnet/api/system.idisposable) instance across multiple services, but the [IDisposable](https://learn.microsoft.com/en-us/dotnet/api/system.idisposable) instance should have a limited lifetime.

**Solution**

Register the instance with a scoped lifetime. Use [IServiceScopeFactory.CreateScope](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) to create a new [IServiceScope](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope). Use the scope's [IServiceProvider](https://learn.microsoft.com/en-us/dotnet/api/system.iserviceprovider) to get required services. Dispose the scope when it's no longer needed.#### General `IDisposable` guidelines

- Don't register [IDisposable](https://learn.microsoft.com/en-us/dotnet/api/system.idisposable) instances with a transient lifetime. Use the factory pattern instead.
- Don't resolve [IDisposable](https://learn.microsoft.com/en-us/dotnet/api/system.idisposable) instances with a transient or scoped lifetime in the root scope. The only exception to this is if the app creates/recreates and disposes [IServiceProvider](https://learn.microsoft.com/en-us/dotnet/api/system.iserviceprovider), but this isn't an ideal pattern.
- Receiving an [IDisposable](https://learn.microsoft.com/en-us/dotnet/api/system.idisposable) dependency via DI doesn't require that the receiver implement [IDisposable](https://learn.microsoft.com/en-us/dotnet/api/system.idisposable) itself. The receiver of the [IDisposable](https://learn.microsoft.com/en-us/dotnet/api/system.idisposable) dependency shouldn't call [Dispose](https://learn.microsoft.com/en-us/dotnet/api/system.idisposable.dispose) on that dependency.
- Use scopes to control the lifetimes of services. Scopes aren't hierarchical, and there's no special connection among scopes.

For more information on resource cleanup, see [Implement a `Dispose` method](https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/implementing-dispose), or [Implement a `DisposeAsync` method](https://learn.microsoft.com/en-us/dotnet/standard/garbage-collection/implementing-disposeasync). Additionally, consider the [Disposable transient services captured by container](https://learn.microsoft.com/en-us/dotnet/core/extensions/#disposable-transient-services-captured-by-container) scenario as it relates to resource cleanup.## Default service container replacement

The built-in service container is designed to serve the needs of the framework and most consumer apps. We recommend using the built-in container unless you need a specific feature that it doesn't support, such as:

- Property injection
- Injection based on name (.NET 7 and earlier versions only. For more information, see [Keyed services](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection#keyed-services).)
- Child containers
- Custom lifetime management
- `Func<T>` support for lazy initialization
- Convention-based registration

The following third-party containers can be used with ASP.NET Core apps:

- [Autofac](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html)
- [DryIoc](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection)
- [Grace](https://www.nuget.org/packages/Grace.DependencyInjection.Extensions)
- [LightInject](https://github.com/seesharper/LightInject.Microsoft.DependencyInjection)
- [Lamar](https://jasperfx.github.io/lamar/)
- [Stashbox](https://github.com/z4kn4fein/stashbox-extensions-dependencyinjection)
- [Simple Injector](https://docs.simpleinjector.org/en/latest/aspnetintegration.html)## Thread safety

Create thread-safe singleton services. If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending on how it's used by the singleton.

The factory method of a singleton service, such as the second argument to [AddSingleton<TService>(IServiceCollection, Func<IServiceProvider,TService>)](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addsingleton), doesn't need to be thread-safe. Like a type (`static`) constructor, it's guaranteed to be called only once by a single thread.

## Recommendations

- `async/await` and `Task` based service resolution isn't supported. Because C# doesn't support asynchronous constructors, use asynchronous methods after synchronously resolving the service.
- Avoid storing data and configuration directly in the service container. For example, a user's shopping cart shouldn't typically be added to the service container. Configuration should use the options pattern. Similarly, avoid "data holder" objects that only exist to allow access to another object. It's better to request the actual item via DI.
- Avoid static access to services. For example, avoid capturing [IApplicationBuilder.ApplicationServices](https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.applicationservices#microsoft-aspnetcore-builder-iapplicationbuilder-applicationservices) as a static field or property for use elsewhere.
- Keep [DI factories](https://learn.microsoft.com/en-us/dotnet/core/extensions/#async-di-factories-can-cause-deadlocks) fast and synchronous.
- Avoid using the [*service locator pattern*](https://learn.microsoft.com/en-us/dotnet/core/extensions/#scoped-service-as-singleton). For example, don't invoke [GetService](https://learn.microsoft.com/en-us/dotnet/api/system.iserviceprovider.getservice) to obtain a service instance when you can use DI instead.
- Another service locator variation to avoid is injecting a factory that resolves dependencies at run time. Both of these practices mix [Inversion of Control](https://learn.microsoft.com/en-us/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.
- Avoid calls to [BuildServiceProvider](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) when configuring services. Calling `BuildServiceProvider` typically happens when the developer wants to resolve a service when registering another service. Instead, use an overload that includes the `IServiceProvider` for this reason.
- [Disposable transient services are captured](https://learn.microsoft.com/en-us/dotnet/core/extensions/#disposable-transient-services-captured-by-container) by the container for disposal. This can turn into a memory leak if resolved from the top-level container.
- Enable scope validation to make sure the app doesn't have singletons that capture scoped services. For more information, see [Scope validation](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection#scope-validation).

Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required. Exceptions are rare, mostly special cases within the framework itself.

DI is an *alternative* to static/global object access patterns. You may not be able to realize the benefits of DI if you mix it with static object access.## Example anti-patterns

In addition to the guidelines in this article, there are several anti-patterns *you **should** avoid*. Some of these anti-patterns are learnings from developing the runtimes themselves.

Warning

These are example anti-patterns, *do not* copy the code, *do not* use these patterns, and avoid these patterns at all costs.### Disposable transient services captured by container

When you register *Transient* services that implement [IDisposable](https://learn.microsoft.com/en-us/dotnet/api/system.idisposable), by default the DI container will hold onto these references, and not [Dispose()](https://learn.microsoft.com/en-us/dotnet/api/system.idisposable.dispose#system-idisposable-dispose) of them until the container is disposed when application stops if they were resolved from the container, or until the scope is disposed if they were resolved from a scope. This can turn into a memory leak if resolved from container level.

[![Anti-pattern: Transient disposables without dispose. Do not copy!](https://learn.microsoft.com/en-us/dotnet/core/extensions/media/transient-disposables-without-dispose.png)](https://learn.microsoft.com/en-us/dotnet/core/extensions/media/transient-disposables-without-dispose.png#lightbox)

In the preceding anti-pattern, 1,000 `ExampleDisposable` objects are instantiated and rooted. They will not be disposed of until the `serviceProvider` instance is disposed.

For more information on debugging memory leaks, see [Debug a memory leak in .NET](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/debug-memory-leak).### Async DI factories can cause deadlocks

The term "DI factories" refers to the overload methods that exist when calling `Add{LIFETIME}`. There are overloads accepting a `Func<IServiceProvider, T>` where `T` is the service being registered, and the parameter is named `implementationFactory`. The `implementationFactory` can be provided as a lambda expression, local function, or method. If the factory is asynchronous, and you use [Task<TResult>.Result](https://learn.microsoft.com/en-us/dotnet/api/system.threading.tasks.task-1.result#system-threading-tasks-task-1-result), this will cause a deadlock.

[![Anti-pattern: Deadlock with async factory. Do not copy!](https://learn.microsoft.com/en-us/dotnet/core/extensions/media/deadlock-with-async-factory.png)](https://learn.microsoft.com/en-us/dotnet/core/extensions/media/deadlock-with-async-factory.png#lightbox)

In the preceding code, the `implementationFactory` is given a lambda expression where the body calls [Task<TResult>.Result](https://learn.microsoft.com/en-us/dotnet/api/system.threading.tasks.task-1.result#system-threading-tasks-task-1-result) on a `Task<Bar>` returning method. This ***causes a deadlock***. The `GetBarAsync` method simply emulates an asynchronous work operation with [Task.Delay](https://learn.microsoft.com/en-us/dotnet/api/system.threading.tasks.task.delay), and then calls [GetRequiredService<T>(IServiceProvider)](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.serviceproviderserviceextensions.getrequiredservice#microsoft-extensions-dependencyinjection-serviceproviderserviceextensions-getrequiredservice-1\(system-iserviceprovider\)).

[![Anti-pattern: Deadlock with async factory inner issue. Do not copy!](https://learn.microsoft.com/en-us/dotnet/core/extensions/media/deadlock-with-async-factory-01.png)](https://learn.microsoft.com/en-us/dotnet/core/extensions/media/deadlock-with-async-factory-01.png#lightbox)

For more information on asynchronous guidance, see [Asynchronous programming: Important info and advice](https://learn.microsoft.com/en-us/dotnet/csharp/asynchronous-programming/async-scenarios#important-info-and-advice). For more information debugging deadlocks, see [Debug a deadlock in .NET](https://learn.microsoft.com/en-us/dotnet/core/diagnostics/debug-deadlock).

When you're running this anti-pattern and the deadlock occurs, you can view the two threads waiting from Visual Studio's Parallel Stacks window. For more information, see [View threads and tasks in the Parallel Stacks window](https://learn.microsoft.com/en-us/visualstudio/debugger/using-the-parallel-stacks-window).### Captive dependency

The term ["captive dependency"](https://blog.ploeh.dk/2014/06/02/captive-dependency) was coined by [Mark Seemann](https://blog.ploeh.dk/about), and refers to the misconfiguration of service lifetimes, where a longer-lived service holds a shorter-lived service captive.

[![Anti-pattern: Captive dependency. Do not copy!](https://learn.microsoft.com/en-us/dotnet/core/extensions/media/captive-dependency.png)](https://learn.microsoft.com/en-us/dotnet/core/extensions/media/captive-dependency.png#lightbox)

In the preceding code, `Foo` is registered as a singleton and `Bar` is scoped - which on the surface seems valid. However, consider the implementation of `Foo`.

```csharp
namespace DependencyInjection.AntiPatterns;

public class Foo(Bar bar)
{
}
```

The `Foo` object requires a `Bar` object, and since `Foo` is a singleton, and `Bar` is scoped - this is a misconfiguration. As is, `Foo` would only be instantiated once, and it would hold onto `Bar` for its lifetime, which is longer than the intended scoped lifetime of `Bar`. You should consider validating scopes, by passing `validateScopes: true` to the [BuildServiceProvider(IServiceCollection, Boolean)](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider#microsoft-extensions-dependencyinjection-servicecollectioncontainerbuilderextensions-buildserviceprovider\(microsoft-extensions-dependencyinjection-iservicecollection-system-boolean\)). When you validate the scopes, you'd get an [InvalidOperationException](https://learn.microsoft.com/en-us/dotnet/api/system.invalidoperationexception) with a message similar to "Cannot consume scoped service 'Bar' from singleton 'Foo'.".

For more information, see [Scope validation](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection#scope-validation).### Scoped service as singleton

When using scoped services, if you're not creating a scope or within an existing scope - the service becomes a singleton.

[![Anti-pattern: Scoped service becomes singleton. Do not copy!](https://learn.microsoft.com/en-us/dotnet/core/extensions/media/scoped-services-becomes-singleton.png)](https://learn.microsoft.com/en-us/dotnet/core/extensions/media/scoped-services-becomes-singleton.png#lightbox)

In the preceding code, `Bar` is retrieved within an [IServiceScope](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope), which is correct. The anti-pattern is the retrieval of `Bar` outside of the scope, and the variable is named `avoid` to show which example retrieval is incorrect.## See also

- [Dependency injection in .NET](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection)
- [Understand dependency injection basics in .NET](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection-basics)
- [Tutorial: Use dependency injection in .NET](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection-usage)