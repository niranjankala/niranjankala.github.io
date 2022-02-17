---
title: "How to resolve Global Exception Logger with Dependency Injection in ASP.NET Web API?"
published: true
description: "How to resolve Global Exception Logger with Dependency Injection in ASP.NET Web API?"
tags: aspnetwebapi
cover_image: 
canonical_url: http://niranjankala.in/post/how-to-resolve-global-exception-logger-with-dependency-injection-in-asp-net-web-api
layout: post
---
    
### Introduction

In this article, you will learn how to resolve Global Exception Logger with dependency injection in ASP.NET Web API using the Autofac. 


### How to resolve Global Exception Logger with dependency injection


Step 1:  Create the custom exception logger by inheriting the IExceptionLogger interface to write your custom logging logic.
```csharp
/// <summary>
/// The main class <c>GlobalExceptionLogger</c>.
/// Handles the exception logging requests.
/// </summary>
public class GlobalExceptionLogger : IExceptionLogger
{
    /// <summary>
    /// 
    /// </summary>
    public ILogger Logger { get; set; }
    /// <summary>
    /// Initializes a new instance of the <see cref="GlobalExceptionLogger"/> class.
    /// </summary>
    public GlobalExceptionLogger()
    {
        Logger = NullLogger.Instance;
    }
    /// <summary>
    /// Logs the exception.
    /// </summary>
    /// <param name="context">The exception context.</param>
    /// <param name="cancellationToken"></param>
    /// <returns></returns>
    public async Task LogAsync(ExceptionLoggerContext context, CancellationToken cancellationToken)
    {
        var ex = context.Exception;
        string message = $"{ex.Message}--{ex.Source}\n{ex.StackTrace}\n{ex.TargetSite}\n";
        await Task.Run(() =>
        {
            Logger.Error(ex, message);
        });
    }

}

```
Step 2: Register your custom exception logger class in the Autofac container to resolve it through dependency injection.
```csharp
/// <summary>
/// The main class <c>AutofacConfig</c>.
/// Provides Autofac DI configuration of the API.
/// </summary>
public static class AutofacConfig
{

    #region Autofac Container
    private static Lazy<IContainer> builder =
      new Lazy<IContainer>(() =>
      {
          var autofacbuilder = new ContainerBuilder();
          RegisterTypes(autofacbuilder);
          return autofacbuilder.Build();
      });

    /// <summary>
    /// Configured Autofac Container.
    /// </summary>
    public static IContainer Container => builder.Value;
    #endregion

    /// <summary>
    /// Registers the type mappings with the autofac container builder.
    /// </summary>
    /// <param name="builder">The autofac container builder to configure.</param>
    /// <remarks>
    /// </remarks>
    public static void RegisterTypes(ContainerBuilder builder)
    {
       string baseDirectoryPath = AppDomain.CurrentDomain.BaseDirectory + "bin";
        if (!Directory.Exists(baseDirectoryPath))
            baseDirectoryPath = AppDomain.CurrentDomain.BaseDirectory;

        builder.RegisterModule(new LoggingModule());
        builder.RegisterModule(new FileStoreModule());
        //builder.RegisterModule(new CloudJobManager.CloudJobManagerModule());
        builder.RegisterApiControllers(Assembly.GetExecutingAssembly()).InstancePerRequest();
        //builder.RegisterType<GlobalExceptionLogger>().InstancePerLifetimeScope();

        var assemblies = Directory.EnumerateFiles(baseDirectoryPath, "*.dll", SearchOption.TopDirectoryOnly)
            .Where(filePath => Path.GetFileName(filePath).StartsWith("MyApp"))
            .Select(Assembly.LoadFrom).Where(assemblyType =>
            (assemblyType.FullName.StartsWith("MyApp") && !assemblyType.FullName.Contains("MyApp.Framework") &&
            !assemblyType.FullName.Contains("MyApp.Reporting.API")
            )).ToArray();

        builder.RegisterAssemblyTypes(assemblies)
        .AsImplementedInterfaces().InstancePerLifetimeScope();

        builder.RegisterType<GlobalExceptionLogger>().AsSelf().AsImplementedInterfaces();
        builder.RegisterType<GenericExceptionHandler>().AsSelf().AsImplementedInterfaces();

        builder.RegisterType<ReportService>().As<IReportService>().InstancePerRequest();

    }
}

```
Step 3: Replace the custom exception logger in the HttpConfiguration services to use it in the place of the default ASP.NET exception logger.

```csharp
public static class WebApiConfig
{
    public static void Register(HttpConfiguration config)
    {
        // Web API configuration and services
        config.DependencyResolver = new AutofacWebApiDependencyResolver(AutofacConfig.Container);

        // Web API configuration and services
        //config.Services.Replace(typeof(IExceptionLogger), new  GlobalExceptionLogger());
        //config.Services.Replace(typeof(IExceptionHandler), new GenericExceptionHandler());

        // Inject our exception logger and handler
        config.Services.Replace(typeof(IExceptionHandler), config.DependencyResolver.GetService(typeof(GenericExceptionHandler)));
        config.Services.Replace(typeof(IExceptionLogger), config.DependencyResolver.GetService(typeof(GlobalExceptionLogger)));

        // Web API routes
        config.MapHttpAttributeRoutes();

        config.Routes.MapHttpRoute(
            name: "DefaultApi",
            routeTemplate: "api/{controller}/{id}",
            defaults: new { id = RouteParameter.Optional }
        );
    }
}

```

### Conclusion
Whenever an unhandled error occurs then you have a chance to log it. The information regarding the can be stored somewhere for review. There you can write the issue to a log or write custom logic.