---
layout: post
title:  "Publishing and Running ASP.NET Core Applications with IIS"
date:   2017-07-12 08:00:00 -0400
categories: aspnetcore iis
---
"The most important thing to understand about hosting ASP.NET Core is that it runs as a standalone, out of process Console application. It's not hosted inside of IIS and it doesn't need IIS to run. ASP.NET Core applications have their own self-hosted Web server and process requests internally using this self-hosted server instance."

"You can however run IIS as a front end proxy for ASP.NET Core applications, because Kestrel is a raw Web server that doesn't support all features a full server like IIS supports. This is actually a recommended practice on Windows in order to provide port 80/443 forwarding which kestrel doesn't support directly. For Windows IIS (or another reverse proxy) will continue to be an important part of the server even with ASP.NET Core applications."

"While the IIS Site/Virtual still needs an IIS Application Pool to run in, the Application Pool should be set to use No Managed Code. Since the App Pool acts merely as a proxy to forward requests, there's no need to have it instantiate a .NET runtime."

<https://weblog.west-wind.com/posts/2016/Jun/06/Publishing-and-Running-ASPNET-Core-Applications-with-IIS>

