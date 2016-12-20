---
title: "Enabling Windows Authentication in Katana | Microsoft Docs"
author: MikeWasson
description: "This article shows how to enable Windows Authentication in Katana. It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Kat..."
ms.author: riande
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
---
[Edit .md file](C:\Projects\msc\dev\Msc.Www\Web.ASP\App_Data\github\aspnet\overview\owin-and-katana\enabling-windows-authentication-in-katana.md) | [Edit dev content](http://www.aspdev.net/umbraco#/content/content/edit/47988) | [View dev content](http://docs.aspdev.net/tutorials/aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana.html) | [View prod content](http://www.asp.net/aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana) | Picker: 47989

Enabling Windows Authentication in Katana
====================
by [Mike Wasson](https://github.com/MikeWasson)

> This article shows how to enable Windows Authentication in Katana. It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process. Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.


Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET. You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md). The OWIN architecture has several layers:

- Host: Manages the process in which the OWIN pipeline runs.
- Server: Opens a network socket and listens for requests.
- Middleware: Processes the HTTP request and response.

Katana currently provides two servers, both of which support Windows Integrated Authentication:

- **Microsoft.Owin.Host.SystemWeb**. Uses IIS with the ASP.NET pipeline.
- **Microsoft.Owin.Host.HttpListener**. Uses [System.Net.HttpListener](https://msdn.microsoft.com/en-us/library/system.net.httplistener.aspx). This server is currently the default option when self-hosting Katana.

> [!NOTE] Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.


## Windows Authentication in IIS

Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.

Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.

![](enabling-windows-authentication-in-katana/_static/image1.png)

Next, add NuGet packages. From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**. In the Package Manager Console window, enter the following command:

    Install-Package Microsoft.Owin.Host.SystemWeb -pre

Now add a class named `Startup` with the following code:

    using Owin;
    
    namespace KatanaWebHost
    {
        public class Startup
        {
            public void Configuration(IAppBuilder app)
            {
                app.Run(context =>
                {
                    context.Response.ContentType = "text/plain";
                    return context.Response.WriteAsync("Hello World!");
                });
            }
        }
    }

That's all you need to create a "Hello world" application for OWIN, running on IIS. Press F5 to debug the application. You should see "Hello World!" in the browser window.

![](enabling-windows-authentication-in-katana/_static/image2.png)

Next, we'll enable Windows Authentication in IIS Express. From the **View** menu, select **Properties**. Click on the project name in Solution Explorer to view the project properties.

In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.

![](enabling-windows-authentication-in-katana/_static/image3.png)

When you run the application from Visual Studio, IIS Express will require the user's Windows credentials. You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool. Here is an example HTTP response:

[!code[Main](enabling-windows-authentication-in-katana/samples/sample1.xml?highlight=1,5-6)]

The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.

Later, when you deploy the application to a server, follow [these steps](http://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.

## Windows Authentication in HttpListener

If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.

First, create a new console application. Next, add NuGet packages. From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**. In the Package Manager Console window, enter the following command:

    Install-Package Microsoft.Owin.SelfHost -Pre

Now add a class named `Startup` with the following code:

    using Owin;
    using System.Net;
    
    namespace KatanaSelfHost
    {
        class Startup
        {
            public void Configuration(IAppBuilder app)
            {
                HttpListener listener = 
                    (HttpListener)app.Properties["System.Net.HttpListener"];
                listener.AuthenticationSchemes = 
                    AuthenticationSchemes.IntegratedWindowsAuthentication;
    
                app.Run(context =>
                {
                    context.Response.ContentType = "text/plain";
                    return context.Response.WriteAsync("Hello World!");
                });
            }
        }
    }

This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.

Inside the `Main` function, start the OWIN pipeline:

    using Microsoft.Owin.Hosting;
    using System;
    
    namespace KatanaSelfHost
    {
        class Program
        {
            static void Main(string[] args)
            {
                using (WebApp.Start<Startup>("http://localhost:9000"))
                {
                    Console.WriteLine("Press Enter to quit.");
                    Console.ReadKey();
                }        
            }
        }
    }

You can send a request in Fiddler to confirm that the application is using Windows Authentication:

[!code[Main](enabling-windows-authentication-in-katana/samples/sample2.xml?highlight=1,4-5)]

## Related Topics

[An Overview of Project Katana](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/en-us/library/system.net.httplistener.aspx)

[Understanding OWIN Forms Authentication in MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)