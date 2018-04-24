---
layout: post
title: Gource; Visualizing Your Source Control History
tags: [tech]
---
I was recently tipped by my good friend [Oddbjørn Bakke](https://twitter.com/oddbeard) about this awesome visualization tool called [Gource](http://gource.io/), which when you run it for your repository, shows you the complete history in an impressive show of "fireworks". The tool generates trees visualizing directories as branches and files as nodes, then shows the changes done by people over time. As my friend Oddbjørn said, running this tool is a good pastime when you are sitting in long and boring non-technical meetings.

[Gource](http://gource.io/) supports [Git](https://git-scm.com/), [Mercurial](https://www.mercurial-scm.org/), [Bazaar](http://bazaar.canonical.com/en/) and [SVN](https://subversion.apache.org/). It's easy to use, simply install it (make sure it gets added to your system path), go to the root folder of your repository, fire up a command line prompt and enter <code>gource</code>. Here is an example of how it looks in action:

<iframe width="560" height="315" src="https://www.youtube.com/embed/NjUuAuBcoqs" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

Pretty neat!