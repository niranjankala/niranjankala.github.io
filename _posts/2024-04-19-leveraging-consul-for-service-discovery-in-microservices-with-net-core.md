---
title: "Leveraging Consul for Service Discovery in Microservices with .NET Core"
published: true
description: "Leveraging Consul for Service Discovery in Microservices with .NET Core"
tags: microservices, Consul, dotnetcore
cover_image: 
canonical_url: https://niranjankala.in/post/leveraging-consul-for-service-discovery-in-microservices-with-net-core
layout: post
---

**Introduction:**

In a microservices architecture, service discovery is pivotal in enabling seamless communication between services. Imagine having a multitude of microservices running across different ports and instances and the challenge of locating and accessing them dynamically. This is where Consul comes into play.

**Introduction to Consul:**   
Consul, a distributed service mesh solution, offers robust service discovery, health checking, and key-value storage features. In this tutorial, we'll explore leveraging Consul for service discovery in a .NET Core environment. We'll set up Consul, create a .NET Core API for service registration, and develop a console application to discover the API using Consul.

**Step 1: Installing Consul:**   
Before integrating Consul into our .NET Core applications, we need to install Consul. Follow these steps to install Consul:
1. Navigate to the Consul downloads page: [Consul Downloads](https://www.consul.io/downloads).
2. Download the appropriate version of Consul for your operating system.
3. Extract the downloaded archive to a location of your choice.
   ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhLNB_O20rYcepA5ODJRdCffeK37qz-MYZjLcInaiwtkVm8-1XHpBA3SDQGZkc1L1Ab_US7ZT_qoStCOIDch9l4AxNEc8KWGcN_R8qVkyiNVyrIjc6A6977X9s0n3UOmzHAdvfVkWYgbZxUMleBSCP_X5_0X1ryJ2baoOPhC8CG_ziteOqU_otBbYla2TUk/s16000/1_Download_Consul.png)
4. Add the Consul executable to your system's PATH environment variable to run it from anywhere in the terminal or command prompt.
   ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiEwYrThFbQ4AvE5uZS3MNhYS3SO1BfncwPJf7I3zNeL5LREBZiiVi5My-UBmRLiEyFETF2IcMWYYOjP-l3d3wyI6bG89EfS1ykaFANQJbyqqkVCVQq4vgcdRqFG-9SnipIZ-Cs8cydcJyfb3Sxz82pGjgx7EAP49X5MPmph-YVmDffdPaxehZgDW0dI30Z/s16000/2_Consul_Path_Entry.png)
5. Open a terminal or command prompt and verify the Consul installation by running the command `consul --version`.
6.  Run the Consul server by running the command `consul agent -dev`.
    ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgra4ZM1W3hSk4CV3atChoayoMsqAjIXCHS-eyoJRbhaBbSckSb31GSl38foM4hfbHqjjNAdObU8Cy280NroN0ANH5kV-eHwyMNTz2_5uRu4mNr-5sKXoqx3-qKgrEpW8Y_dP8fgzXJ2pt2M58yW8KlIx4G5sfigMLjYjzFk1JY4XCQ7LvxsF_9bij0goYs/s16000/3_Start_Consul_Agent.png) 
    


**Step 2: Setting Up the Catalog API:**

Now, let's create a .NET Core API project named `ServiceDiscoveryTutorials.CatalogApi`. This API will act as a service that needs to be discovered by other applications. Use the following command to create the project:
```bash
dotnet new webapi -n ServiceDiscoveryTutorials.CatalogApi
```
Next, configure the API to register with the Consul upon startup. Add the Consul client package to the project:
```bash
dotnet add package Consul
```
In the `Startup.cs` file, configure Consul service registration in the `ConfigureServices` method:
```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers();

    services.AddSingleton<IConsulClient>(p => new ConsulClient(consulConfig =>
    {
        var consulHost = builder.Configuration["Consul:Host"];
        var consulPort = Convert.ToInt32(builder.Configuration["Consul:Port"]);
        consulConfig.Address = new Uri($"http://{consulHost}:{consulPort}");
    }));
    
    services.AddSingleton<IServiceDiscovery, ConsulServiceDiscovery>();

}
```
Create a class named `ConsulServiceDiscovery` that implements the `IServiceDiscovery` interface to handle service registration:
```csharp
public interface IServiceDiscovery
{
    Task RegisterServiceAsync(string serviceName, string serviceId, string serviceAddress, int servicePort);
    Task RegisterServiceAsync(AgentServiceRegistration serviceRegistration);
    
    Task DeRegisterServiceAsync(string serviceId);
}

public class ConsulServiceDiscovery : IServiceDiscovery
{
    private readonly IConsulClient _consulClient;

    public ConsulServiceDiscovery(IConsulClient consulClient)
    {
        _consulClient = consulClient;
    }

    public async Task RegisterServiceAsync(string serviceName, string serviceId, string serviceAddress, int servicePort)
    {
        var registration = new AgentServiceRegistration
        {
            ID = serviceId,
            Name = serviceName,
            Address = serviceAddress,
            Port = servicePort
        };
        await _consulClient.Agent.ServiceDeregister(serviceId);
        await _consulClient.Agent.ServiceRegister(registration);
    }

    public async Task DeRegisterServiceAsync(string serviceId)
    {
        await _consulClient.Agent.ServiceDeregister(serviceId);
    }

    public async Task RegisterServiceAsync(AgentServiceRegistration registration)
    {
        await _consulClient.Agent.ServiceDeregister(registration.ID);
        await _consulClient.Agent.ServiceRegister(registration);
    }
}
```
In the `Configure` method of `Startup.cs`, add the service registration logic:
```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env, IConsulClient consulClient)
{
    // Configure the HTTP request pipeline.
    if (app.Environment.IsDevelopment())
    {
        app.UseSwagger();
        app.UseSwaggerUI();
    }
    
    //app.UseHttpsRedirection();
    
    app.UseAuthorization();
    
    
    app.MapControllers();
    
    var discovery = app.Services.GetRequiredService<IServiceDiscovery>();
    var lifetime = app.Services.GetRequiredService<IHostApplicationLifetime>();
    var serviceName = "CatalogApi";
    var serviceId = Guid.NewGuid().ToString();
    var serviceAddress = "localhost";
    var servicePort = 7269;
    
    lifetime.ApplicationStarted.Register(async () =>
    {
        var registration = new AgentServiceRegistration
        {
            ID = serviceId,
            Name = serviceName,
            Address = serviceAddress,
            Port = servicePort,
            Check = new AgentServiceCheck
            {
                HTTP = $"https://{serviceAddress}:{servicePort}/Health",
                Interval = TimeSpan.FromSeconds(10),
                Timeout = TimeSpan.FromSeconds(5)
            }
        };
        await discovery.RegisterServiceAsync(registration);
    });
    
    lifetime.ApplicationStopping.Register(async () =>
    {
        await discovery.DeRegisterServiceAsync(serviceId);
    });

}
```
With these configurations, the Catalog API will register itself with the Consul upon startup and deregister upon shutdown.

**Step 3: Creating the Client Application:**

Next, create a console application named `ServiceDiscoveryTutorials.ClientApp`. Use the following command to create the project:
```bash
dotnet new console -n ServiceDiscoveryTutorials.ClientApp
```
Add the Consul client package to the project:
```bash
dotnet add package Consul
```
In the `Program.cs` file, configure the Consul client to discover services:
```csharp
class Program
{
    static async Task Main(string[] args)
    {
        using (var client = new ConsulClient(consulConfig =>
        {
            consulConfig.Address = new Uri("http://localhost:8500");
        }))
        {
            var services = await client.Catalog.Service("CatalogApi");
            foreach (var service in services.Response)
            {
                Console.WriteLine($"Service ID: {service.ServiceID}, Address: {service.ServiceAddress}, Port: {service.ServicePort}");
            }
        }
        //var consulClient = new ConsulClient();
        //// Specify the service name to discover
        //string serviceName = "CatalogApi";
        //// Query Consul for healthy instances of the service
        //var services = (await consulClient.Health.Service(serviceName, tag: null, passingOnly: true)).Response;
        //// Iterate through the discovered services
        //foreach (var service in services)
        //{
        //    var serviceAddress = service.Service.Address;
        //    var servicePort = service.Service.Port;
        //    Console.WriteLine($"Found service at {serviceAddress}:{servicePort}");
        //    // You can now use the serviceAddress and servicePort to communicate with the discovered service.
        //}

    }
}
```
This code snippet retrieves all instances of the `CatalogApi` service registered with the Consul.

**Step 3: Testing the API and Client Application:**   

Below is the project structure in the Visual Studio.
   ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhPV8piCZ1dyMifvo1cnmIjzhusqRH_98De3NUc6aCtZKp2YHwaJ7G8A4w5vAiJNr6lFVfFgp5OV0S9pVEfUymkgZV41ko3r3tFYpTcjovjapOwnK_1MJ_JqCPWzpFEF0x86zNsnUCubKlztSbsjll1woOt36dDq-cNDf9fXF0Z2bbYm9XhjgBH8QNKBAm5/s2122/5_Project_Setup_Https_Port_Settings.png) 

Next, let's run both applications using the command `dotnet run`. When this application starts, the Consul portal will display the registered service.
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjBHhutEGStgDvIulo_rm4KE1pDl7qE2s_F9RNJbFRjl1LhD4xiuSsGg_Cjs1M77f4MStU6xtkmQMSNVys6LtAfI0EdMuSE2ZYnK3IWLWVdDeRs7Clp78F-M2xZHnFw4i9uKHugkp0bO60zQVjPByvshE6t8EUbpXNfiwVVockajS1VPB9z1797bs6Iwk-2/s16000/6_Consul_Portal_State_After_Starting_Service.png) 

Below is the final results of the application.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi61agjLQZPXGDR6rznJw4Ba4BUfuOAgv3HSIeT0_norsBOay3ndCHGKFoweF52RNtxMGhDOyfpJ2il5zQ6k9rvinZCNv3RbtLLiZpnM0loSNueKhlw2HfB_5kvqxmUgMi9YNqXikkKPbZhchqPHsUo4w3IoNYCPpDpA4apFWlMWUvZAFqsxzPJmA0efDbe/s16000/7_Final_Demo.png) 




**Conclusion:**   
In this tutorial, we've learned how to set up Consul for service discovery and register a .NET Core API with Consul. Additionally, we've developed a console application to discover services using Consul's API. By leveraging Consul, you can enhance the scalability and reliability of your microservices architecture.


[**Source Code**](https://github.com/niranjankala/system-design-and-architecture/blob/main/microservices/src/ServiceDiscoveryTutorials)
