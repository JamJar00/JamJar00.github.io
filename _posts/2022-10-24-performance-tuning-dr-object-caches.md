---
layout: post
title:  "Performance Tuning DR Object Caches"
tags:   DarkRift
---

DarkRift's object caches are unfortunately a black box to most DR users. Many people know they exist and probably understand why they exist, but I'd hazard a guess only a handful of people know how to tune them and even fewer how to tune them correctly.

The object caches are designed to reduce the number of objects allocations DR has to make and reduce the number of objects .NET has to garbage collect. Both of these operations are costly and take up precious processor cycles as the .NET runtime juggles its memory. In games, the garbage collector (GC) running can cause lag spikes as the entire game stops running while the GC releases memory (.NET is much better at this now, and in recent versions no longer 'stops the world' by using its new background GC. I'm not sure if Unity has got around to adding this yet.)

# What options are there?
The `<cache>` section of the configuration is well documented [here](https://www.darkriftnetworking.com/DarkRift2/Docs/2.10.0/configuration/server/cache.html) where you can see all the different options you can set on it.

Practically, there are actually just two different options but you can set those options on a lot of different objects.
- `maxXYZ` options set the maximum number of that object DarkRift will cache _per thread_. For example, a value of 32 would mean each thread keeps a list of up to 32 instances of that type it can reuse when it needs a new instance.
- `XYZMemoryBlockSize` set the minimum number of bytes to store in a buffer of `XYZ` size. These are closely related to the `maxXYZMemoryBlocks` option which are a subset of the first `maxXYZ` option. The reason these extra options exist for 'memory blocks' (which are just byte arrays) is because byte arrays have a size - it would be no use in requesting a byte array from the object cache if it then returned you a byte array with too little space to store your data - so DarkRift groups these into the five classes of size and lets you configure what sizes are.

# What are good values?
Great question, simple answer: Set all the `maxXYZ` ones much much higher.

I can't really remember why the default values are what they are, but the `maxXYZ` options are all far to small. One of the first things I do when I setup a new DR server is copy the following into the config:
```xml
<cache maxCachedWriters="1024"
       maxCachedReaders="1024
       maxCachedMessages="1024"
       maxCachedSocketAsyncEventArgs="1024"
       maxAutoRecyclingArrays="1024"
       maxExtraSmallMemoryBlocks="1024"
       maxSmallMemoryBlocks="1024"
       maxMediumMemoryBlocks="1024"
       maxLargeMemoryBlocks="1024"
       maxExtraLargeMemoryBlocks="1024"
/>
```

`1024` isn't particularly scientifically based but it seems to performan well whenever I've done benchmarks and keeping that many objects doesn't confuse the GC too much.

For some of the objects, keeping that many instances is very unecessary but the objects are only kept if they're needed in the first place (i.e. DR doesn't 'prewarm' the cache in any way). It's pretty unlikely you'll ever have a single thread using more than one `DarkRiftWriter` at any point, but you'll have plenty of space if you did want to use 1024 of them concurrently on one thread...

What about the ActionDispatcherTask?

# Recycling warnings?

# Future improvements?
