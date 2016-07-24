---
layout: post
title: Published a new package to npm
tags: [js]
---
I want to share with you all that [I published a new package to npm today](https://www.npmjs.com/package/node-server-ar-drone). This is a component that I wrote over 2 years ago, that I just now finally decided to publish as a module. The initial idea was experimenting with REST and drones, but it grew and I realized that it could benefit many developers. 

2 years ago I was really into drones, I did talks (one of them [here](https://abakus.no/event/1405-itera-internet-of-things-rise-of-the-drones/)) about drones and wrote lots of fun code for them. A few tweets from my twitter feed show some of the history:

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Controlling an Arma 3D drone from .NET and soon WinPhone CC: <a href="https://twitter.com/IteraASA">@IteraASA</a> <a href="https://twitter.com/hashtag/Arma3?src=hash">#Arma3</a> <a href="https://twitter.com/hashtag/Drone?src=hash">#Drone</a> <a href="http://t.co/Z1amobGpN7">pic.twitter.com/Z1amobGpN7</a></p>&mdash; Sirar Salih (@sirarsalih) <a href="https://twitter.com/sirarsalih/status/478180839592058880">June 15, 2014</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">A Node.js server for your Parrot AR Drone <a href="https://t.co/e4bFeRPBKU">https://t.co/e4bFeRPBKU</a> <a href="https://twitter.com/hashtag/NodeJS?src=hash">#NodeJS</a> <a href="https://twitter.com/hashtag/ExpressJS?src=hash">#ExpressJS</a> <a href="https://twitter.com/hashtag/Parrot?src=hash">#Parrot</a> <a href="https://twitter.com/hashtag/ParrotARDrone?src=hash">#ParrotARDrone</a> CC: <a href="https://twitter.com/IteraASA">@IteraASA</a></p>&mdash; Sirar Salih (@sirarsalih) <a href="https://twitter.com/sirarsalih/status/501031083681800192">August 17, 2014</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Updated source code of my REST drone, the position based GET is inelegantly working. <a href="https://t.co/HcVcWGYilp">https://t.co/HcVcWGYilp</a> <a href="https://twitter.com/hashtag/Victory?src=hash">#Victory</a> <a href="https://twitter.com/hashtag/NodeJS?src=hash">#NodeJS</a> <a href="https://twitter.com/hashtag/ParrotARDrone?src=hash">#ParrotARDrone</a></p>&mdash; Sirar Salih (@sirarsalih) <a href="https://twitter.com/sirarsalih/status/502565216421687298">August 21, 2014</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

The first tweet shown here was prior to actually obtaining a drone. So what do you do if you don't have a drone? You use a virtual one! During the Summer of 2014, a colleague of mine and I worked on [arma-command-console](https://github.com/sirarsalih/arma-command-console), [azure-cloud-service-ga](https://github.com/sirarsalih/azure-cloud-service-ga) and [winphone-map-path-finder](https://github.com/sirarsalih/winphone-map-path-finder) to fly a virtual Parrot AR Drone in a map path finding concept. This project was extremely fun and certainly deserves a blog post by its own.

When I wrote [node-server-ar-drone](https://www.npmjs.com/package/node-server-ar-drone), I have to admit that I did it at home, which means I flew a [Parrot AR Drone 2.0](http://www.parrot.com/usa/products/ardrone-2/) indoors (!) I definitely recommend *not* to do that. So the idea here is simple, in our Internet connected world, we see more and more a need of connecting things. I thought drones are really fun, so why not make it possible to remotely control a drone from anywhere - by anyone - in the world? Connect to your drone, install the module and fly with REST.