---
layout: post
title:  "VS 2017 ASP.NET Core Web Application (.NET Framework)"
date:   2017-05-16 15:45:00 -0400
categories: vs2017 aspnetcore dotnet
---
And note that VS 2017 .NET 4.6.2 new ASP.NET Core Web Application (.NET Framework) defaults to x86 platform … I had to switch this to AnyCPU to use our NuGet packages.

You may get a CodeTaskFactory error when trying to use common NuGet packages in a Core project using .NET 4.6.2. I had to update an internal-ish NuGet package to fix this:

![Update Baseclass.Contrib.Nuget.Output NuGet package]({{ site.url }}/assets/nuget.jpg)

This was the error:

The task factory "CodeTaskFactory" could not be loaded from the assembly "C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\MSBuild\15.0\Bin\Microsoft.Build.Tasks.v15.0.dll". Could not load file or assembly 'file:///C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\MSBuild\15.0\Bin\Microsoft.Build.Tasks.v15.0.dll' or one of its dependencies. The system cannot find the file specified.

I had to jump through a number of hoops to get a ASP.NET Core Web Application (.NET Framework) working with our existing NuGet packages (libraries like AJBoggs.UAS.Security.Public).

Until I changed my project to AnyCPU and removed the runtime identifier I wasn't able to use Ninject. Which doesn't support Core, but I should be able to load it and use it, if I'm building against .NET 4.6.2 right? 

Removed this: <RuntimeIdentifier>win7-x86</RuntimeIdentifier> 

Added this: <PropertyGroup Condition="'$(Configuration)|$(Platform)'=='Debug|AnyCPU'"> <PlatformTarget>AnyCPU</PlatformTarget> </PropertyGroup> 
                                
Doing the above (switching to AnyCPU and not specifying the runtime identifier) broke Kestrel. I got my project to run by switching to WebListener as described here:
[WebListener web server implementation in ASP.NET Core][web-listener]

{% highlight csharp %}
public static void Main(string[] args)
{
	var host = new WebHostBuilder()
		//.UseKestrel()
		.UseWebListener(options =>
		{
			options.ListenerSettings.Authentication.Schemes = AuthenticationSchemes.None;
			options.ListenerSettings.Authentication.AllowAnonymous = true;
		})
		.UseContentRoot(Directory.GetCurrentDirectory())
		//.UseIISIntegration()
		.UseStartup<Startup>()
		.UseApplicationInsights()
		.Build();

	host.Run();
}
{% endhighlight %}

I also changed Default package management format: PackageReference … in NuGet Package Manager settings

![Debug Settings 1]({{ site.url }}/assets/vs.jpg)

![Debug Settings 2]({{ site.url }}/assets/debug-settings.jpg)




[web-listener]: https://docs.microsoft.com/en-us/aspnet/core/fundamentals/servers/weblistener