---
title: "[How Do I:] Map an ASP.NET Server Control to the Adaptor Used to Render It | Microsoft Docs"
author: rick-anderson
description: "In this video Chris Pels will show how to use a control adaptor to provide different renderings for an ASP.NET server control without actually changing the c..."
ms.author: riande
manager: wpickett
ms.date: 06/19/2008
ms.topic: article
ms.assetid: 
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-map-an-aspnet-server-control-to-the-adaptor-used-to-render-it
---
[Edit .md file](C:\Projects\msc\dev\Msc.Www\Web.ASP\App_Data\github\web-forms\videos\how-do-i\how-do-i-map-an-aspnet-server-control-to-the-adaptor-used-to-render-it.md) | [Edit dev content](http://www.aspdev.net/umbraco#/content/content/edit/26476) | [View dev content](http://docs.aspdev.net/tutorials/web-forms/videos/how-do-i/how-do-i-map-an-aspnet-server-control-to-the-adaptor-used-to-render-it.html) | [View prod content](http://www.asp.net/web-forms/videos/how-do-i/how-do-i-map-an-aspnet-server-control-to-the-adaptor-used-to-render-it) | Picker: 33499

[How Do I:] Map an ASP.NET Server Control to the Adaptor Used to Render It
====================
by [Chris Pels](https://twitter.com/chrispels)

In this video Chris Pels will show how to use a control adaptor to provide different renderings for an ASP.NET server control without actually changing the control itself. In this video, an ASP.NET BulletList control will be adapted to display each list item horizontally using DIV elements instead of the traditional UL elements. First, see how to create a class that inherits WebControlAdaptor and then implements the code to render the new list format. Next, learn how to map the new control adaptor to the ASP.NET server control in the .browser definition file. Then see how to use the new control adaptor on pages in a web site. Finally, learn how a control adaptor can be associated with either all browsers or specific types of browsers.

[&#9654; Watch video (23 minutes)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-map-an-aspnet-server-control-to-the-adaptor-used-to-render-it)