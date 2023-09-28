---
title: "Exploring Common Security Vulnerabilities: Defending Against XSS and CSRF Attacks in ASP.NET Core"
published: true
description: "Exploring Common Security Vulnerabilities: Defending Against XSS and CSRF Attacks in ASP.NET Core"
tags: aspdotnetcore
cover_image: 
canonical_url: https://niranjankala.in/post/exploring-common-security-vulnerabilities-defending-against-xss-and-csrf-attacks-in-asp-net-core
layout: post
---

## Introduction:

Security vulnerabilities are weaknesses within software or hardware systems that malicious actors can exploit to gain unauthorized access to sensitive data or compromise the integrity of a system. This article will delve into two of the most prevalent security vulnerabilities: Cross-Site Scripting (XSS) and Cross-Site Request Forgery (CSRF) attacks. We'll explore how these vulnerabilities can affect your ASP.NET Core applications and, more importantly, how to defend against them effectively.

## Understanding XSS (Cross-Site Scripting) Attacks

XSS attacks are among the most common threats to web applications. Hackers inject malicious client-side scripts into a legitimate website or web application in these attacks. These scripts can then execute within the context of a user's browser, potentially leading to advanced attacks, including:

- **Cookie Theft:** Malicious scripts can steal user cookies, compromising session data.
- **Phishing:** Attackers can impersonate trusted sites, tricking users into divulging sensitive information.
- **Key Logging:** Capturing keystrokes can reveal login credentials and other sensitive data.
- **Identity Theft:** Stolen user information can be used for identity theft and fraud.

### Prevention Strategies for XSS Attacks

The key to preventing XSS attacks is thorough input validation and output encoding. In ASP.NET Core, you can implement these strategies using Razor Pages or Views. Here's an example of how to encode user input to prevent XSS:

```csharp
@using Microsoft.AspNetCore.Html
@model string

<!-- Encoding user input to prevent XSS -->
<p>@Html.Encode(Model)</p>
```

By using `@Html.Encode`, you ensure that user input is HTML-encoded, rendering any injected scripts harmless.

## Understanding CSRF (Cross-Site Request Forgery) Attacks

CSRF attacks occur when attackers use authenticated user sessions to send unauthorized requests from unauthenticated users to web applications or sites. These attacks can be challenging to detect because they exploit the trust between the user and the website. Here's a real-world example to illustrate CSRF:

Imagine logging into a website and playing a game where you collect coins. An attacker might trick you into giving up your coins without your knowledge. They could send a link or button that appears to be part of the game but is, in fact, a trick. When you click it, your web app or computer performs actions you didn't intend, such as transferring your coins to the attacker.

### Prevention Strategies for CSRF Attacks

Defending against CSRF attacks requires a combination of techniques:

1. **Same-Site Cookies:** Implement same-site cookie attributes to restrict which cookies can accompany a request. This ensures that cookies are only sent with requests from the same origin.

2. **User Interaction:** After login or when sensitive actions are performed, request reauthentication or present CAPTCHA challenges to users.

3. **One-Time Tokens:** Use one-time tokens to verify the legitimacy of requests. Tokens are generated for each action and can only be used once.

4. **Custom Request Headers:** For API endpoints, require custom request headers. Users can only add these headers using JavaScript and must add them within their origin.

## ASP.NET Core Configuration for Security

Before diving into XSS and CSRF prevention details, let's configure our ASP.NET Core application to enhance security. Open your `Startup.cs` file and locate the `ConfigureServices` method. We'll add the necessary services for security:

```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.DependencyInjection;

public void ConfigureServices(IServiceCollection services)
{
    // Other configurations here

    // Add Antiforgery service for CSRF protection
    services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");

    // Other configurations here
}
```

In this code snippet, we're adding the Antiforgery service to enable CSRF protection. We also specify the header name to be used for CSRF tokens.

Next, locate the `Configure` method in `Startup.cs`. We'll configure CORS to prevent cross-origin security issues:

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.Extensions.Hosting;

public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    // Other middleware configurations here

    // Configure CORS
    app.UseCors(builder =>
    {
        builder.WithOrigins("https://trusted-site.com")
               .AllowAnyHeader()
               .AllowAnyMethod();
    });

    // Other middleware configurations here
}
```

This code configures CORS to allow requests only from the trusted site "https://trusted-site.com." You can adjust the origins as needed for your application.

## Conclusion

This article explored two common security vulnerabilities, XSS and CSRF attacks, and discussed how they can impact your ASP.NET Core applications. By configuring your application for security and implementing preventive measures such as data encoding, anti-forgery tokens, and CORS policies, you can significantly enhance your application's resistance to these threats.

Remember that staying vigilant and proactive in the ever-evolving web security landscape is essential. Implement the recommended strategies and keep your ASP.NET Core applications secure against these common security vulnerabilities.

