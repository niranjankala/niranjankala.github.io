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
2. If you haven't already created your own [Azure AD B2C Tenant](https://learn.microsoft.com/en-us/azure/active-directory-b2c/tutorial-create-tenant), create one now. You can use an existing Azure AD B2C tenant.
3. **Visual Studio or Visual Studio Code:** We'll use Visual Studio or Visual Studio Code for creating and running the ASP.NET Core API project.
4. **.NET Core SDK:** Ensure that you have the .NET Core SDK installed on your development machine.
5. **Azure CLI (Optional):** Azure CLI provides a command-line interface for interacting with Azure resources. It's optional but can be useful for managing Azure resources.

**Step 1: App registrations**   
1. Sign in to the Azure portal (https://portal.azure.com) using your Azure account credentials.
2. Navigate to the **Azure Active Directory** service and select **App registrations**.
   ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhvpYZlUU51yvFo8nUnJNjn2ZH_3QwDyMz9u9PDS-_hTMsPS7-e7xzvpUMTAcBsanBfbdjTiS0YrrJnt1HgCH2n266I6RdiT08rO8XBgQIi6tpERL81Q0a1MsaeiddkfdCvU7eYAYVvGa6VkDLVAsaUjFjkof5OHV2QXviIMpW_RCbWATVia2PmVo5rebZk/w640-h400/1_Azure_App_Registration.png)  
3. Click on "**+ New registration**" to create a new application registration.
4. Provide a name for your application, select the appropriate account type, and specify the redirect URI for authentication callbacks.
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi7VVhOZ2AVn51MYF1hxy0yjm0SUswQXok4y0hMexu64TH1NN70pY7XRKEuYHkbZ9pE2-P8aRHMBGsONUAraAk0Ly18p8WHptHaJV7ty-8WMTDv7td2bHEaEro7I4ncRCmS7ZtK3rlbIs8L4eU-Hh_65Uzq9y9PXC-CSbTv2DNDtLSBpxvfa_cIpksWdb2r/w640-h492/2_Azure_App_Registration.png)  
5. After creating the application registration, note down the Application (client) ID and Directory (tenant) ID.
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj29H6hcUMWfdpeievdiCLduPaes-zf1ndyV4cdEhS2SJKOZWg_gaO0h7tHnsbRbxExp4FHnvYxWCuAnRK3kqegkbKEGjaPK9YcvLABpoVX76lFUhB1sYUrze-qjHZ1JBvI3yFAUCpkJNnsOAOqhN17Qyoa0M7MTLsg6acyjSL0VNJjTWy9C4FE0qw3H3Oz/w640-h492/3_Azure_App_Registration.png)

**Step 2: Create a client secret**
1. Once the application is registered, note down the Application (client) ID and Directory (tenant) ID.
2. In the Azure AD B2C - App registrations page, select the application you created if you are not on the application management screen.
3. In the left menu, under Manage, select Certificates & secrets.
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEihuacQPWRBdWTQFDMZRFX_n8-FGRt1V1mkxYWBgpkS1v_yhGWY9f2g5F_rvevKXjP42Z_Wmn9ry4TkTkpipuK6cHiLPrxqUdBmCKSMXxdSAN3BQbkwRHXvrYzV-zaUUL-7ccbAoRrfoSgqVrw53ZnwxHu0ORvnHsfEOUHBTzPDjSXSoIulEEJGXg-VZAlj/w640-h492/4_Azure_App_Secret_Key_Registration.png)
4. Under "**Certificates & secrets**", generate a new client secret by clicking on **New client secret**.
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj6X7abePPiPSrrFTcevI9LPOW9R3M5tRkZyZKQCNBRAGjxKifvzjR5pbcJOzCzHy_VARiKODmQpk6cQqAWO2BDNUNoKgkyd53Y4UByTxEFnghqRkDXMTkHWxRP0bptCNliZLox-Myzz-SE__c0hYj3jS0YWAyP1SfrWeb6XWxJCrDZjcCIua7a2d6DUjHz/w640-h492/5_Azure_App_Secret_Key_Registration.png)
5. Enter a description for the client secret in the Description box. For example, Ocelotsecret.
6. Under **Expires**, select a duration for which the secret is valid, and then click **Add**.
7. Copy the secret's Value for use in your client application code and save it securely.
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi5UN68jBJDwMcgwiMzyjIHTLbJ0OBg948ICNitH1S9V3fMuFn1zmXUPatOPliDtpKTOy6Mxz9Ix__tc-2myW-Gbs59tGayHqlQspWDdXOEUrW9AH2TNoZd0uEcr4-RGX3xVdi-HzktBsNnzIMIuNCMQ5lhEJJ6umIwWCyOY9nv8PupaLijJi1Yj7N06A8D/w640-h492/6_Azure_App_Secret_Key_Registration.png)


**Step 3: Configure scopes** 
1. In the Azure AD B2C - App registrations page, select the application you created if you are not on the application management screen.
2. Select App registrations. Select the OcelotTutorials application to open its Overview page.
3. Under Manage, select Expose an API.
4. Next to Application ID URI, select the **Add** link.
5. I have not changed default guid with my api but you can replace the default value (a GUID) with api, and then select Save. The full URI is shown, and should be in the format https://your-tenant-name.onmicrosoft.com/api. When your web application requests an access token for the API, it should add this URI as the prefix for each scope that you define for the API.
6. Under Scopes defined by this API, select **Add a scope**.
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjM_OTTocO2e3uCwKpk-qGK18mSH1_IPUzYp2Lu3LIHVy4iwTksdgBPVXbC0meT9dHAjBPmIUMUwe9UWRJR8x-1EcLnl6cdaXVEPfROHB4HFluZNhoHtT4azrbu9SLWy9REGCnLv7KTMM0RtHrm2fxq2RtO3AqCQ29cV-s-DNVxGS8ZR8vmgLe2ket_55GH/w640-h493/7_Azure_App_Scope.png)

7. Enter the following values to create a scope that defines read access to the API, then select **Add scope**:   

   Scope name: ocelottutorial.read   
   Admin consent display name: Read access to API Gateway API     
   Admin consent description: Allows read access to the API Gateway API    

   ![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjukxGJSCXvixNq_firUXIZv8NRAX8yBv4K7sbH4-LsrXHXcOF8RhDUHPuBQeOjNOknQ5_BaYpcLGjpET-Iy6M0npi7vEpoPvLvBfgUbN3iv1ybauOe1UJzOSoCpyoJ6naoFOuREbsdskwcn9jFT29MHfbbxkECbzJaCvOR3qVA9uS-IpPHwdmOE7dKVqvu/w640-h492/8_Azure_App_Scope.png)


**Step 4: Grant permissions**
1. Select App registrations, and then select the web application that should have access to the API. For example, OcelotTutorials.
2. Under Manage, select API permissions.
3. Under Configured permissions, select Add a permission.
4. Select the My APIs tab.
5. Select the API to which the web application should be granted access. For example, webapi1.
6. Under Permission, expand demo, and then select the scopes that you defined earlier. For example, ocelottutorial.read and ocelottutorial.write.
7. Select Add permissions.
8. Select Grant admin consent for (your tenant name).
9. If you're prompted to select an account, select your currently signed-in administrator account, or sign in with an account in your Azure AD B2C tenant that's been assigned at least the Cloud application administrator role.
10. Select Yes.
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgt-SV-xVKxdJi-d4JfF57ji3AYwLRk-f8UnEGBHRz4PC8Ce8VxRmjW_JT8T6Nr7tPC6gtgo97haMSt60VXIQKicV7aZyiI_mAsomselOT2VxvnLPQbzLHGIur6icGCYv8U5O8mW8WlFEPIznyLHMpVGbbMBWaSO1pxCy1vPw10ZR2gBlhr1s_Z1eSAizVI/w640-h492/9_Azure_App_API_Permission.png)
11. Select Refresh, and then verify that "Granted for ..." appears under Status for both scopes. 


**Step 5: Enable ID token implicit grant**

If you register this app and configure it with https://jwt.ms/ app for testing a user flow or custom policy, you need to enable the implicit grant flow in the app registration:

1. In the left menu, under **Manage**, select **Authentication**.
2. Under Implicit grant and hybrid flows, select both the Access tokens (used for implicit flows) and ID tokens (used for implicit and hybrid flows) check boxes.
3. Select Save.
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjnjJLLqmXhV-Z5kG1VK9hb_HfhV51berx-2Ul0uKuTDslNYBW6ccX2W3KC_2Zwq3XY4LbOvjyM4cS8V5bkc42XSSRPP6wUtmyh0v0AHEzIa0abKu3Ho52Lvy7p6gm3m9PY8fHarH5ThJdegBf5-OvZAqIlJrUbtuL4cadCpQfHtdU_hn1nFK9-TMFjf8kE/w640-h610/10_Azure_App_Authentication_Token_Settings.png)



**Step 6: Set Up Azure B2C Authentication in ASP.NET Core API**
1. Create 3 new ASP.NET Core Web API project in Visual Studio or Visual Studio Code.   
Accounting.API   
Inventory.API   
ApiGateway   
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg_bbU7_KWSYJbbe7r6QMhyphenhyphenDf1ruCibO0SLZ5lLtOjPp2JoMuKhAUFLzeqG8X71t_e83b6SqCSBXSlgm2Q8q5AmqwqPoCAOrYiZcm4AVb6GX7evYnKb9DtSbgIKwGB7bnj0MipYHIw3PSozgGEfEi7XXQbs6sdZmXIJwRHTxFKaAtd507sAnbAwtnMorFSg/s16000/Project%20Structure.png)

2. Assign the ports to the APi. ApiGateay 9000, Accounting.API 9001, Inventory.API 9002
    ```json
    {
      "Urls": "http://localhost:9001",
      "Logging": {
        "LogLevel": {
          "Default": "Information",
          "Microsoft.AspNetCore": "Warning"
        }
      },
      "AllowedHosts": "*"
    }
    ```
3. Install the necessary NuGet packages for Azure B2C authentication. Install the below packages in the ApiGateway project

    ```bash
    dotnet add package Microsoft.Identity.Web
    dotnet add package Ocelot
    ```
3. Configure Azure B2C authentication in your `Startup.cs` file:

    ```csharp
    builder.Services.AddMicrosoftIdentityWebApiAuthentication(builder.Configuration);
    ```

4. Add the Azure B2C settings to your `appsettings.json` file:

    ```json
    {
      "Urls": "http://localhost:9000",
      "Logging": {
        "LogLevel": {
          "Default": "Information",
          "Microsoft.AspNetCore": "Warning"
        }
      },
      "AllowedHosts": "*",
      "AzureAd": {
        "Instance": "https://login.microsoftonline.com/",
        "Domain": "http://localhost:9000/",
        "TenantId": "",
        "ClientId": ""
      }
    }
    ```

5. Ensure that the authentication middleware is added to the request processing pipeline in the `Configure` method of `Startup.cs`:

    ```csharp
    app.UseAuthentication(); // Place UseAuthentication before UseOcelot
    app.UseAuthorization(); // Place UseAuthorization before UseAuthentication
    ```
6. Add `ocelot.json` file to the the ApiGateway with the below configuration
    ```json
    {
      "Routes": [
        {
          "DownstreamPathTemplate": "/api/values",
          "DownstreamScheme": "http",
          "DownstreamHostAndPorts": [
            {
              "Host": "localhost",
              "Port": 9001
            }
          ],
          "UpstreamPathTemplate": "/accounting",
          "UpstreamHttpMethod": [ "GET" ],
          "AuthenticationOptions": {
            "AuthenticationProviderKey": "Bearer",
            "AllowedScopes": []
          }
        },
    
        {
          "DownstreamPathTemplate": "/api/values",
          "DownstreamScheme": "http",
          "DownstreamHostAndPorts": [
            {
              "Host": "localhost",
              "Port": 9002
            }
          ],
          "UpstreamPathTemplate": "/inventory",
          "UpstreamHttpMethod": [ "GET" ],
          "AuthenticationOptions": {
            "AuthenticationProviderKey": "Bearer",
            "AllowedScopes": []
          }
        }    
      ],
      "GlobalConfiguration": {
        "BaseUrl": "http://localhost:9000"
      }
    } 
    ```
7.  Added ocelot configuration to the services
    ```csharp
    // Ocelot configuration
    builder.Configuration.AddJsonFile("ocelot.json", optional: false, reloadOnChange: true);
    builder.Services.AddOcelot(builder.Configuration);
    ```

8.  Added Ocelot to the middleware pipeline in the end.
    ```csharp
    app.UseAuthentication(); // Place UseAuthentication before UseOcelot
    app.UseAuthorization(); // Place UseAuthorization before UseAuthentication
    app.MapControllers();
    app.UseOcelot().Wait();
    app.Run();
    ```
  

**Step 7: Testing authentication**
To Test this refer this tutorial
[OAuth 2.0 authorization code flow in Azure Active Directory B2C](https://learn.microsoft.com/en-us/azure/active-directory-b2c/authorization-code-flow)

1. Replace the required fields and use the below URL in the browser to get the code to fetch the token.
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?client_id={client id}&response_type=code&response_mode=query&scope={scope uri}&state=007
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjq04Vq5bRqkcZu3c-CRxVN7f5NsflQ7I0k3E2DeO1NIP8wfZ0oiy3j7o6MBlLCkNvJcGpYMUYPUTDD53oIr78DnZSOYNREgutyF-3alqj6draHyoq3r7KBb9WwEttLFE_ORzKYi5Pud93f8E5rIWqKwutwjcfkQwTL662lNr-1hCUC2usvRvLI-9ItLvPO/s16000/11_Authentication_Get_Code.png)
2. Open Postman and use the returned code to generate the token. See the below image to check URL and the required fields to get the token.

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgkTwZqIJ3G4xFjLBl0f3I9HoavwWwukp186gqbP6WgDh6EQTDTa0Vn5ro-P3pRB_ynWJdVJeBhwI6U9Ib-UnA-GeS9StJfRjHtpCdZQnRqB2yt22MfcyNGeut1icbG3vvbhRyw3BtoRntyHc8G2GI6gsV2qd0xz_LuuPojxcrU4r8lU4NPS8UFaVDwV6bj/s16000/12_Authentication_Get_Token.png)
3. Now we are ready to call our API Gateway with the token.
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjJdNnhSuxyo0PHqV0ijCBoFnry1HXG1fNPCIVXdgFXnmoTR003IT0uashDnR04zcqxngZ7fM7Lg4gF_6C6_NAb5FfdXCH7ppM7eJ8aG1-F7JjQfLHr-CtUKaJirlmkuyXhPPKfPQ4uDU0cde3xey6Ymyp5EQpMww01YWtLv_ckwPz9G7-dG9THE4UnbfDH/s16000/13_Authentication_Use_Token.png)


**Conclusion:**    
In this blog post, we've covered the first part of securing your microservices architecture using Azure B2C authentication. We walked through the process of configuring Azure B2C for authentication, including creating a tenant, setting up user flows (policies), and integrating Azure B2C authentication into an ASP.NET Core API project. In the next part of this series, we'll explore how to integrate Azure B2C authentication with Ocelot API Gateway for centralized authentication and authorization management across microservices. Stay tuned for the next installment!


**References:**  
[Tutorial: Register a web application in Azure Active Directory B2C](https://learn.microsoft.com/en-us/azure/active-directory-b2c/tutorial-register-applications)
[Add a web API application to your Azure Active Directory B2C tenant](https://learn.microsoft.com/en-us/azure/active-directory-b2c/add-web-api-application?tabs=app-reg-ga)
