# Projects
## DarkRift Networking
[DarkRift Networking](https://www.darkriftnetworking.com/) is a high performance, multithreaded networking system for Unity designed for speed and flexibility.

Originally a project I started while I was still in sixth form it grew to be one of the top 5 networking systems for Unity and was aquired by [Unordinal](https://unordinal.com/) in 2022 and [open sourced](https://github.com/DarkRiftNetworking/DarkRift) as part of their product family.

## Hazel Networking
Designed as the foundation for DarkRift 2, [Hazel](https://github.com/DarkRiftNetworking/Hazel-Networking) was a low level networking library that handled reliable UDP communication, also supporting ordered messages and fragmentation. While it never made it into DarkRift 2's final codebase it did get [forked](https://github.com/willardf/Hazel-Networking) and used as the networking library for the multi-award winning game (Among Us](https://en.wikipedia.org/wiki/Among_Us).

## Fast Obect Filter & Fast String Format
These two projects were also originally designed for use in DarkRift to provided incredibly fast filtering of messages going through the servers for logging, forwarding etc. [Both](https://github.com/JamJar00/fast-object-filter) [projects](https://github.com/JamJar00/fast-string-format) make extensive used of .NET expressions and provide [near raw .NET levels of performance](https://github.com/JamJar00/fast-string-format#current-benchmarks) ehile being completely runtime configurable. Definitely two projects I'd like to revist!

## Pyash
[Pyash](https://github.com/JamJar00/pyash) was a joke project to see how far you can abuse operator overloading in Python to make it run shell-like syntax. It was accompanied by a more thorough explaination in a blog post on [dev.to](https://dev.to/jamoyjamie/pyash-and-why-i-can-t-have-nice-things-anymore-26lo). It always interested me that Apache Airflow chose to use a similar concept in their DAGs.
