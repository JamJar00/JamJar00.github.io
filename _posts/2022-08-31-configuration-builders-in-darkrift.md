---
layout: post
title: Configuration Builders in DarkRift
tags: darkrift
---

I think most people know that you can host a DarkRift server within your own .NET application fairly easily. There's essentially three steps to it:
- Create a `ServerSpawnData` object with any settings you want your server to start with
- Optionally create a `ClusterSpawnData` if you plan on using the server clustering features
- Create a `DarkRiftServer` object with the above objects

This is demonstrated very well in the [console server's code](https://github.com/DarkRiftNetworking/DarkRift/blob/master/DarkRift.Server.Console/Program.cs), which after all is just a console application hosting a DarkRift server.

The two spawn data objects are most commonly created from XML using their `CreateFromXml` methods as it's much easier to read the configuration you're putting into the server in a configuration format rather than in C#, particularly because the horrific structure of the objects tends to mean you end up with some garish code like this:
```csharp
var serverSpawnData = new ServerSpawnData();

serverSpawnData.PluginsSettings.PluginSettings pluginSettings = new ServerSpawnData.PluginsSettings.PluginSettings
{
    Type = "MyPlugin",
    Load = true
};

serverSpawnData.Plugins.Plugins.Add(pluginSettings);

return new DarkRiftServer(serverSpawnData);
```

## Configuration Builder Objects
With the next release of DarkRift (or I suppose current release, if you're building from source) DarkRift adds two new objects to make this all a little easier: `DarkRiftServerConfigurationBuilder` and `DarkRiftClusterConfigurationBuilder`.

These two objects provide a fluent interface to build DarkRift servers with:
```csharp
var builder = DarkRiftServerConfigurationBuilder.Create()
                                                .AddPlugin("MyPlugin");

return new DarkRiftServer(builder.ServerSpawnData);
```

Where the spawn data objects are based around the XML configuration structure, and map almost 1-1 to it, the configuration builders are designed around the actual changes you may want to make to the configuration. For example, there are methods to add a plugin, add a log writer, set the advertised host/port; all of which make configuring a DarkRift server from code less awful:
```csharp
var serverSpawnData = new ServerSpawnData();

serverSpawnData.PluginsSettings.PluginSettings pluginSettings = new ServerSpawnData.PluginsSettings.PluginSettings
{
    Type = "MyPlugin",
    Load = true
};

serverSpawnData.Plugins.Plugins.Add(pluginSettings);

serverSpawnData.LoggingSettings.LogWriterSettings logWriterSettings = new ServerSpawnData.LoggingSettings.LogWriterSettings
{
    Name = "LogWriter1",
    Type = "ConsoleWriter",
    LogLevels = new LogType[] { LogType.Info, LogType.Warning, LogType.Error, LogType.Fatal }
};

serverSpawnData.Logging.LogWriters.Add(logWriterSettings);

serverSpawnData.Server.ServerGroup = "Group1"
serverSpawnData.ServerRegistry.AdvertisedHost = "localhost";
serverSpawnData.ServerRegistry.AdvertisedPort = 4296;

return new DarkRiftServer(serverSpawnData);
```

And more intuitive:
```csharp
var builder = DarkRiftServerConfigurationBuilder.Create()
                                                .AddPlugin("MyPlugin")
                                                .AddLogWriter("LogWriter1", "ConsoleWriter", LogType.Info, LogType.Warning, LogType.Error, LogType.Fatal)
                                                .WithServerGroup("Group1")
                                                .WithAdvertisedHost("localhost")
                                                .WithAdvertisedPort(4296);

return new DarkRiftServer(builder.ServerSpawnData);
```
