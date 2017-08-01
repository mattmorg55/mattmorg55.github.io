---
layout: post
title:  "ASP.NET Core Web Application OpenID Connect Keycloak"
date:   2017-08-01 15:45:00 -0400
categories: aspnetcore dotnet openid-connect keycloak
---
This worked with the Standard Flow in Keycloak with an ASP.NET Core(1.1.2) application. Downloaded NuGet package Microsoft.AspNetCore.Authentication.OpenIdConnect (1.1.2).

{% highlight csharp %}
//First line of Configure method in Startup.cs
JwtSecurityTokenHandler.DefaultInboundClaimTypeMap.Clear();

...

app.UseStaticFiles();

app.UseCookieAuthentication(new CookieAuthenticationOptions
{
	AutomaticAuthenticate = true,
	AuthenticationScheme = CookieAuthenticationDefaults.AuthenticationScheme
});

app.UseOpenIdConnectAuthentication(new OpenIdConnectOptions
{
	AutomaticAuthenticate = true,
	ClientId = "sample-web-app",
	ClientSecret = "8eb92690-8c0c-42ba-b1ac-106dd2d06a22",
	Authority = "https://titanoboa.ajboggs.com/auth/realms/ajboggs",
	ResponseType = OpenIdConnectResponseType.Code,
	GetClaimsFromUserInfoEndpoint = true,
	AuthenticationScheme = OpenIdConnectDefaults.AuthenticationScheme,
	SignInScheme = CookieAuthenticationDefaults.AuthenticationScheme
});

app.UseMvc(routes =>
{
	routes.MapRoute(
						name: "default",
						template: "{controller=Home}/{action=Index}/{id?}");
});
{% endhighlight %}