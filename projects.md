# Projects
## DarkRift Networking
[DarkRift Networking](https://www.darkriftnetworking.com/) is a high performance, multithreaded networking system for Unity designed for speed and flexibility.

Originally a project I started to learn C#, it grew to be one of the top five networking systems for Unity and was acquired by [Unordinal](https://unordinal.com/) in 2022 and [open sourced](https://github.com/DarkRiftNetworking/DarkRift) as part of their product family.

## Hazel Networking
Designed as the foundation for DarkRift 2, [Hazel](https://github.com/DarkRiftNetworking/Hazel-Networking) was a low level networking library that handled reliable UDP communication, supporting ordered messages and fragmentation. While it never made it into DarkRift 2's final codebase it did get [forked](https://github.com/willardf/Hazel-Networking) and used as the networking library for the multi-award winning game [Among Us](https://store.steampowered.com/app/945360/Among_Us/).

## Fast Object Filter & Fast String Format
These two projects were also originally designed for use in DarkRift to provide incredibly fast filtering of messages for logging, forwarding etc. [Both](https://github.com/JamJar00/fast-object-filter) [projects](https://github.com/JamJar00/fast-string-format) make extensive used of .NET expressions and provide [near raw .NET levels of performance](https://github.com/JamJar00/fast-string-format#current-benchmarks) while being completely runtime configurable.

## Japr
[Jamie's Awesome Project Rater](https://github.com/JamJar00/japr) is a language-agnostic tool that rates and lints projects by looking at the project's tooling and language setup. It checks things like the content of your readme, whether you've get a linter installed and that you've setup git well. It's a hard linter to satisfy, but your projects become all the better for it

## loggrep
[loggrep](https://github.com/JamJar00/loggrep) is grep but specifically for logs. It was built out of furustration trying to filter down a terminal full of ASP.NET logs and allows you to intelligently filter raw log files or stdout where you don't have something like ELK to rely on. It's written in Rust too so it's pretty speedy.

## restdir
[A simple project to serve a directory.](https://github.com/JamJar00/restdir) This is something I've needed a few times and was always a little sad there wasn't a simple and lightweight tool for it. I can't say I've actually used it since writing it, but at least I know I have it now.

## SpanishDictionary Shortcut
While practising Spanish I regularly find myself looking up words by copying them, opening up [SpanishDictionary.com](https://spanishdict.com), pasting them and hitting enter. In search of an afternoon's entertainment and a few less clicks [I wrote my first Firefox add-on](https://github.com/JamJar00/spanishdictionary-shortcut) to add a simple button in the context menu that gets you straight to your highlighted word's definition and translation. Sure enough, this is saved me countless hours since building it. You can even install it yourself [here](https://addons.mozilla.org/en-GB/firefox/addon/spanishdictionary-shortcut/)!
