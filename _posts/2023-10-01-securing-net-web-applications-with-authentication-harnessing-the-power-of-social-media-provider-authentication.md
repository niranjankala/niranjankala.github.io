---
title: "Securing .NET Web Applications with Authentication: Harnessing the Power of Social Media Provider Authentication"
published: true
description: "Securing .NET Web Applications with Authentication: Harnessing the Power of Social Media Provider Authentication"
tags: aspdotnetcore
cover_image: 
canonical_url: https://niranjankala.in/post/securing-net-web-applications-with-authentication-harnessing-the-power-of-social-media-provider-authentication
layout: post
---


**Title:** Integrating Google Authentication into Your ASP.NET Core App

**Introduction:**

In today's interconnected world, users expect a seamless and secure login experience on websites and applications. Social media provider authentication, such as Google Authentication, offers a convenient and trusted way for users to access your application using their existing social media accounts. In this tutorial, we'll explore how to integrate Google Authentication into your ASP.NET Core application step by step.

**Prerequisites:**

Before we dive into the implementation, make sure you have the following:

1. Visual Studio or Visual Studio Code installed on your system.
2. An ASP.NET Core web application project created.

**Step 1: Install the Required NuGet Package**

The first step is to install the `Microsoft.AspNetCore.Authentication.Google` NuGet package into your ASP.NET Core project. This package provides the necessary tools for integrating Google Authentication.

Open the NuGet Package Manager in Visual Studio, search for `Microsoft.AspNetCore.Authentication.Google`, and install the latest stable version.

```bash
Install-Package Microsoft.AspNetCore.Authentication.Google
```

**Step 2: Configure Google Authentication**

To enable Google Authentication in your application, you need to set up the necessary credentials on the Google Developer Console. Follow these steps:

1. Navigate to the [Google Developer Console](https://console.cloud.google.com/).

2. Create a new project or select an existing one.

3. Set up the consent screen by providing the required information, including the application name, support email, and domain.

4. Add authorized JavaScript origins and redirect URIs for your application. Make sure to include both the development and production URLs.

5. Under the "Scopes" section, add the necessary scopes, such as `email`, `profile`, and `openid`.

6. Save your changes and publish the app.

7. Create OAuth 2.0 credentials by going to "Credentials" and selecting "Create credentials" > "OAuth client ID." Choose the application type as "Web application" and configure the redirect URIs.

8. After creating the OAuth client ID, note down the "Client ID" and "Client Secret" values.

**Step 3: Configure Your ASP.NET Core Application**

Now, let's configure your ASP.NET Core application to use Google Authentication. Open your `appsettings.json` file and add the Google Authentication settings:

```json
{
  "Authentication": {
    "Google": {
      "ClientId": "YOUR_CLIENT_ID",
      "ClientSecret": "YOUR_CLIENT_SECRET"
    }
  },
  // Other application settings...
}
```

Replace `"YOUR_CLIENT_ID"` and `"YOUR_CLIENT_SECRET"` with the values you obtained from the Google Developer Console.

**Step 4: Enable Google Authentication in Your App**

In your ASP.NET Core application, navigate to the `Program.cs` file. Inside the `CreateHostBuilder` method, add the following code to enable Google Authentication:

```csharp
builder.Services.AddAuthentication()
    .AddGoogle(options =>
    {
        options.ClientId = Configuration["Authentication:Google:ClientId"];
        options.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
    });
```

This code configures the authentication services to use Google Authentication and sets the client ID and client secret from your `appsettings.json` file.

**Step 5: Run Your Application**

With Google Authentication configured, start your ASP.NET Core application. You should now see the "Use other service to log in" option on your login page, with Google as one of the available choices.

Click the "Google" button, and a popup will appear, prompting you to sign in with your Google account. After signing in, you'll be redirected back to your application, logged in and authenticated via Google.

**Conclusion:**

By integrating Google Authentication into your ASP.NET Core application, you've provided your users with a convenient and secure way to access your services without the need to create additional accounts. This enhances the user experience and can boost user engagement on your platform.