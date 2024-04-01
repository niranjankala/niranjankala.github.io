---
title: "Securing Your Microservices: Azure B2C Authentication in ASP.NET Core API with Ocelot API Gateway"
published: true
description: "Securing Your Microservices: Azure B2C Authentication in ASP.NET Core API with Ocelot API Gateway"
tags: microservices,Ocelot,Azure AD B2C,dotnetcore
cover_image: 
canonical_url: https://niranjankala.in/post/securing-your-microservices-azure-b2c-authentication-in-asp-net-core-api-with-ocelot-api-gateway
layout: post
---

**Introduction:**   
Microservices architecture offers flexibility and scalability, but it also presents challenges in managing authentication and authorization across multiple services. In this blog post, we will explore how to secure your microservices using Azure B2C authentication in ASP.NET Core API with Ocelot API Gateway. We'll start by configuring Azure B2C for authentication, and then integrate it with our ASP.NET Core API through Ocelot.

**Prerequisites:**   
1. **Azure Subscription:** You'll need an Azure subscription to create and configure Azure B2C resources.
2. **Visual Studio or Visual Studio Code:** We'll use Visual Studio or Visual Studio Code for creating and running the ASP.NET Core API project.
3. **.NET Core SDK:** Ensure that you have the .NET Core SDK installed on your development machine.
4. **Azure CLI (Optional):** Azure CLI provides a command-line interface for interacting with Azure resources. It's optional but can be useful for managing Azure resources.

**Step 1: Create Azure B2C Tenant**   
1. Sign in to the Azure portal (https://portal.azure.com) using your Azure account credentials.
2. Navigate to the **Azure Active Directory** service and select **App registrations**.
   ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhvpYZlUU51yvFo8nUnJNjn2ZH_3QwDyMz9u9PDS-_hTMsPS7-e7xzvpUMTAcBsanBfbdjTiS0YrrJnt1HgCH2n266I6RdiT08rO8XBgQIi6tpERL81Q0a1MsaeiddkfdCvU7eYAYVvGa6VkDLVAsaUjFjkof5OHV2QXviIMpW_RCbWATVia2PmVo5rebZk/w640-h400/1_Azure_App_Registration.png)  
3. Click on "**+ New registration**" to create a new application registration.
4. Provide a name for your application, select the appropriate account type, and specify the redirect URI for authentication callbacks.
5. After creating the application registration, note down the Application (client) ID and Directory (tenant) ID.

**Step 2: Configure User Flows (Policies)**
1. In the Azure portal, go to the **Azure Active Directory** service and select **User flows (policies)**.
2. Click on "**+ New user flow**" and choose the "**Sign up and sign in**" user flow type.
3. Follow the prompts to configure the user flow settings, including identity providers, attributes to collect, and user experience customization.
4. After creating the user flow, navigate to **App registrations** in the Azure B2C service.
5. Register a new application by clicking on "**+ New registration**".
6. Provide a name for your application, select the appropriate account type, and specify the redirect URI for authentication callbacks.
7. Once the application is registered, note down the Application (client) ID and Directory (tenant) ID.
8. Under "**Certificates & secrets**", generate a new client secret and save it securely.

**Step 3: Set Up Azure B2C Authentication in ASP.NET Core API**
1. Create a new ASP.NET Core Web API project in Visual Studio or Visual Studio Code.
2. Install the necessary NuGet packages for Azure B2C authentication:

```bash
dotnet add package Microsoft.AspNetCore.Authentication.AzureADB2C
```

3. Configure Azure B2C authentication in your `Startup.cs` file:

```csharp
services.AddAuthentication(AzureADB2CDefaults.AuthenticationScheme)
    .AddAzureADB2C(options => Configuration.Bind("AzureAdB2C", options));
```

4. Add the Azure B2C settings to your `appsettings.json` file:

```json
"AzureAdB2C": {
  "Instance": "https://<your-tenant-name>.b2clogin.com",
  "ClientId": "<your-client-id>",
  "Domain": "<your-tenant-name>.onmicrosoft.com",
  "SignUpSignInPolicyId": "<your-signup-signin-policy>",
  "ResetPasswordPolicyId": "<your-reset-password-policy>",
  "EditProfilePolicyId": "<your-edit-profile-policy>"
}
```

5. Ensure that the authentication middleware is added to the request processing pipeline in the `Configure` method of `Startup.cs`:

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

**Conclusion:**    
In this blog post, we've covered the first part of securing your microservices architecture using Azure B2C authentication. We walked through the process of configuring Azure B2C for authentication, including creating a tenant, setting up user flows (policies), and integrating Azure B2C authentication into an ASP.NET Core API project. In the next part of this series, we'll explore how to integrate Azure B2C authentication with Ocelot API Gateway for centralized authentication and authorization management across microservices. Stay tuned for the next installment!