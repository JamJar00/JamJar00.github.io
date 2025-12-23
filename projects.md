# Projects
## DarkRift Networking
[DarkRift Networking](https://www.darkriftnetworking.com/) was a high performance, multithreaded networking system for Unity designed for speed and flexibility.

Originally a project I started to learn C#, it grew to be one of the top five networking systems for Unity and was acquired by [Unordinal](https://unordinal.com/) in 2022 and [open sourced](https://github.com/DarkRiftNetworking/DarkRift) as part of their product family.

## Hazel Networking
Designed as the foundation for DarkRift 2, [Hazel](https://github.com/DarkRiftNetworking/Hazel-Networking) was a low level networking library that handled reliable UDP communication, supporting ordered messages and fragmentation. While never getting incorporated into DarkRift's final codebase, it did get [forked](https://github.com/willardf/Hazel-Networking) and used as the networking library for the multi-award winning game [Among Us](https://store.steampowered.com/app/945360/Among_Us/).

## Compute Cost
AWS compute costs scale differently with load. To make this easier to visualise and compare (roughly at least) I put together an interactive dashboard you can try for yourself [here](https://compute-cost.jread.dev)

## SpanishDictionary Shortcut
While practising Spanish I regularly find myself looking up words by copying them, opening up [SpanishDictionary.com](https://spanishdict.com), pasting them and hitting enter. In search of an afternoon's entertainment and a few less clicks, [I developed my first Firefox add-on](https://github.com/JamJar00/spanishdictionary-shortcut): a simple button in the context menu that gets you straight to your highlighted word's definition and translation. Sure enough, this is saved me countless hours since building it. You can install it yourself [here](https://addons.mozilla.org/en-GB/firefox/addon/spanishdictionary-shortcut/)!

## Japr
[Jamie's Awesome Project Rater](https://github.com/JamJar00/japr) is a language-agnostic tool that rates and lints projects by looking at the project's tooling and language setup. It checks things like the content of your readme, whether you've got a linter installed and that you've setup git well. It's a hard linter to satisfy, but your projects become all the better for it. This still receives updates from time to time as I uncover new rules to add.

## Fast Object Filter & Fast String Format
These two projects were also originally designed for use in DarkRift to provide incredibly fast filtering of messages for logging, forwarding etc. [Both](https://github.com/JamJar00/fast-object-filter) [projects](https://github.com/JamJar00/fast-string-format) make extensive used of .NET expressions and provide [near raw .NET levels of performance](https://github.com/JamJar00/fast-string-format#current-benchmarks) while remaining completely runtime configurable.

## Are my Projects in Git?
Another simple afternoon's entertainment developing, and another tool I regularly return to, [Are my Projects in Git?](https://github.com/JamJar00/are-my-projects-in-git) tells you at a glance what folders aren't setup in git, have untracked files or uncomitted changes.

## Current Projects
Many of my current projects are still private however in my spare time I continue to develop tools including an AWS architecture diagrammer, a personal todo app and a custom build system.
