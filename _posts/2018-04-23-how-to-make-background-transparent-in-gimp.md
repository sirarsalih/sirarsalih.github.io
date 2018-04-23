---
layout: post
title: How to Make Background Transparent in Gimp
tags: [tech]
---
I feel that everytime I want to do this (which is like once or twice a year), I find myself googling. I thought that, once and for all, I'd write a blog post on how to do this.

[Gimp](https://www.gimp.org/) is an awesome, free open source image editor. It's rich with functionality and you can do a great deal with it, I highly recommend using it if you want to do image editing. This is the kind of tool you want to go for which is free and is packed with tons of stuff.

Alright, so on to the ultimate question; how can you make the background of an image transparent? It's a really easy and straightforward process. First, open up the image in Gimp by going to <code>File -> Open as Layers...</code>:

[<img src="{{ site.url }}/public/img/gimp_1.png">]({{ site.url }}/public/img/gimp_1.png)

Click on the background of the image (the area which you want to make transparent):

[<img src="{{ site.url }}/public/img/gimp_2.png">]({{ site.url }}/public/img/gimp_2.png)

Go to <code>Layer -> Transparency -> Add Alpha Channel</code>:

[<img src="{{ site.url }}/public/img/gimp_3.png">]({{ site.url }}/public/img/gimp_3.png)

Press the <code>Delete</code> keyboard button:

[<img src="{{ site.url }}/public/img/gimp_4.png">]({{ site.url }}/public/img/gimp_4.png)

Go to <code>File -> Export As...</code>:

[<img src="{{ site.url }}/public/img/gimp_5.png">]({{ site.url }}/public/img/gimp_5.png)

Choose PNG file format and click on <code>Export</code>:

[<img src="{{ site.url }}/public/img/gimp_6.png">]({{ site.url }}/public/img/gimp_6.png)

Click <code>Export</code> again:

[<img src="{{ site.url }}/public/img/gimp_7.png">]({{ site.url }}/public/img/gimp_7.png)

And that's it! The image now has a transparent background.