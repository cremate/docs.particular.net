---
title: ASP.NET Core Dependency Injection Integration
component: core
reviewed: 2017-10-02
tags:
 - dependency injection
related:
 - nservicebus/dependency-injection
---

[ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/) comes with an integrated [Dependency Injection (DI) feature](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection). When hosting NServiceBus endpoints inside an Asp.NET Core app, components registered for DI might need to be shared between Asp.NET components and NServiceBus message handlers.

### Configuring an endpoint to use built-in DI

Note: Using this sample requires adding reference to the `NServiceBus.MSDependencyInjection` package.

snippet: ContainerConfiguration

When the sample is run a web application is started, all web requests received will trigger a message send:

snippet: RequestHandling

Message handlers will have dependencies injected at runtime by the configured Inversion of Control container:

snippet: InjectingDependency

### Configuring to use shared DI using Autofac

It is also possible to use an external container to share components between ASP.NET and NServiceBus message handlers.

Note: While this sample illustrates the scenario using [Autofac](/nservicebus/dependency-injection/autofac.md), the same can be achieved by using most other [DI libraries](/nservicebus/dependency-injection/). Using Autofac requires adding reference to the `Autofac.Extensions.DependencyInjection` package.

snippet: ContainerConfigurationAutofac

The [ConfigureServices method](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/startup#the-configureservices-method) is called by .NET Core infrastructure [at application startup time](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/startup) and returns a `IServiceProvider` instance that will be used by the infrastructure to resolve components. The same container instance is shared with the NServiceBus endpoint.