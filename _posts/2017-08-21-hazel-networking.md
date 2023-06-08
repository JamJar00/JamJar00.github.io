---
title: Hazel Networking
tags: showdev, networking, dotnet, opensource
---

It's been a while since I've done much on it but I feel if there's anything I should showcase here it should be this!

## Hazel
Hazel is networking library for C# that aims to simplify networking and take away some of the unneeded complexities and poor interfaces of sockets.

While I've only really advertised it on the Unity game engine forums it's fully compatible with .NET 3.5 and above (though not .NET core yet) so anyone who needs networking in .NET should be able to use it straight off.

It supports communication over TCP, UDP and its own RUDP and provides 3 major guarantees/abstractions for all of them:
* Connection oriented
* Message based
* Thread safe

By providing a standard interface it becomes really trivial to switch protocol and I've aimed to make it as simple and lightweight to use as possible.

## Code and Packages

You can get the source on [GitHub](https://github.com/DarkRiftNetworking/Hazel-Networking) or as a NuGet package [here](https://www.nuget.org/packages/DarkRiftNetworking.Hazel/) (MIT license, yay!)

##Future (and a little past)

Hazel was a great little project for me and I had a great few weeks trying to invent and implement the RUDP algorithms and make them as efficient as possible. I really recommend it as a challenge to anyone who wants to learn more about networking or get a little experience in algorithmic thinking.

I originally intended Hazel to be the core of one of my more commercial side projects but sadly I've since changed that to a TCP/UDP socket pair in order to simplify the code base a bit. I still plan on maintaining Hazel as it's pretty much complete, so if anyone wants to use it or submit pull requests/issues etc. then I'll make sure to put some extra time into it with you!

One of the other visions I had for Hazel was to implement it in as many languages as possible in order to create a truly cross-platform networking library, hence the lightweightness of it, but that's perhaps something for the far future.

Any discussion, improvements or thoughts are more than welcome, I'd love for more people to give it a try!
