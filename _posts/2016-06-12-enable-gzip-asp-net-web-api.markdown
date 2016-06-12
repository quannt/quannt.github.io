---
layout: post
title: "Enable gzip in ASP.NET Web API"
categories: [programming]
fullview: false
---

Enabling gzip to reduce the response size is a good technique to speed up your ASP.NET Web API application. This works wonder if your API has to produce some heavy json result.

There are generally two ways to enable gzip compression in an ASP.NET Web API application.

### First apporach: Modifying the host ApplicationHost.config file
The first approach is to modify the ApplicationHost config file which is located at `%windir%\system32\inetsrv\config`. Find the `httpCompression -> dynamicTypes` tag then add 
{% highlight XML %}
 <add mimeType="application/json" enabled="true" />
 {% endhighlight %}

However, this does not work in my case as the `gzip` header for some reason is stripped which leads me to the second approach


### Second apporach: Using the plugin Microsoft.AspNet.WebApi.MessageHandlers.Compression
[This plugin](https://github.com/azzlack/Microsoft.AspNet.WebApi.MessageHandlers.Compression) does the job and is very simple to use.
First install it using Nuget `Install-Package Microsoft.AspNet.WebApi.Extensions.Compression.Server`

Then add to your `App_Start\WebApiConfig.cs`
{% highlight C# %}
GlobalConfiguration.Configuration.MessageHandlers.Insert(0, new ServerCompressionHandler(new GZipCompressor(), new DeflateCompressor()));
 {% endhighlight %}

To make sure that the compression is working, inspect the response and make sure `Content-Encoding: gzip` is presented.
