---
layout:     post
title:      Simple measuring of Rails RSS
date:       2018-05-04 11:21:29 -0700
summary:    I toy around with monitoring memory in Rails applications.
categories: ruby rails
---
![Ram](https://source.unsplash.com/BHQrJv34sw4/600)

Recently, I was working on a project for my company that involved parsing an extremely large (roughly 10Gb) XML file in our Rails application. There were a lot of methods used behind the scenes handling Elasticsearch, PostgreSQL, and the XML data that I had a sneaking suspicion might be eating up RAM.

I develop on my MacBook most of the time, so I tried using the `htop` CLI tool, but the results were a bit too verbose for what I needed. What I was looking for was a tool that could tell me how much RAM rails was using while I was running a particular function. It seemed simple enough so I built it myself:

<script src="https://gist.github.com/frankolson/366fe419f00413d263863e8a01715341.js"></script>

The premise is pretty simple: while the function is running, I want to log Rail’s RSS (resident set size) and stop when the function has completed.

For those who are unfamiliar with RSS, it is the memory your computer sets aside for a particular process. This seemed like a simple starting point for determining if my code was consuming a lot of memory.

The output of my development log ended up looking like this:

```
[MemoryLogger] Start time: 2018-05-04 10:00:00 -0700
[MemoryLogger] Resident memory size: 1.224Mb
[MemoryLogger] Resident memory size: 2.224Mb
[MemoryLogger] Resident memory size: 3.224Mb
[MemoryLogger] Resident memory size: 5.224Mb
[MemoryLogger] Resident memory size: 8.224Mb
```

This was my first attempt at a solution, and it worked pretty well for my use case. I would love to hear other’s thoughts on how this could be expanded into a cool CLI tool for visualizing Rails and ruby memory consumption.

---

I wrapped the functionality above into a gem which you can checkout here: [https://github.com/frankolson/memory_logr](https://github.com/frankolson/memory_logr)
