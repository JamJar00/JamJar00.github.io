---
layout: post
title: Running DarkRift in Docker
date: 2022-07-23 14:20 -0000
categories: DarkRift
---

# Running DarkRift in Docker
I don't think there's any good resources for this yet, and if there are one more won't hurt, so this is a quick guide to putting DarkRift in a Docker image.

## Option 1 - DarkRift CLI
The easiest way to get DarkRift (note, console-based only) is to be using the [DarkRift CLI](https://github.com/DarkRiftNetworking/darkrift-cli). If you're not already using it I'd really recommend having a play around with it as it makes general DarkRift life easier.

There's already a Docker image you can make use of [here](https://hub.docker.com/r/darkriftnetworking/darkrift-cli). To run your DarkRift CLI project in Docker just requires you to build a Dockerfile off the CLI image and add your content into it!

Something like:
```Dockerfile
FROM darkriftnetworking/darkrift-cli:latest

COPY ./Project.xml /project
COPY ./Server.config /project
COPY ./plugins /project/plugins
```

Make sure you don't disable the healthcheck in your `Server.config` as the Docker image uses that to check the server is still alive.

## Option 2 - From Scratch
The other option is to write your own Dockerfile from scratch for it. This is necessary for Unity servers and if you're not using DarkRift CLI.

For a .NET Core console-based server, a basic Dockerfile for this might look like:
```Dockerfile
FROM mcr.microsoft.com/dotnet/runtime:3.1-alpine

COPY . /darkrift

WORKDIR /darkrift

HEALTHCHECK --interval=10s --timeout=3s \
  CMD curl -f http://localhost:10666/health || exit 1

ENTRYPOINT ["dotnet"]
CMD ["run", "/darkrift/Lib/DarkRift.Server.Console.dll"]
```

(Full disclosure, I've not tested that Dockerfile)
