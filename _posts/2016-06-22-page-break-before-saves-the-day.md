---
layout: post
title: page-break-before saves the day
tags: [html-css]
---

I was thrust into a bit drama a couple of weeks ago at work, it was basically regarding a PDF document that got generated from our ASP.NET MVC code base. The business wanted a certain content in the document to always appear on a new page. The code that generated the document hadn't been touched for years. We used [Pechkin](https://github.com/gmanny/Pechkin) to convert HTML to PDF. My initial thought was to create a seperate Razor view that contained the content, merge it with the rest of the HTML and then convert the result to PDF.

It seemed there was an easier way to do this. 

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">page-break-before: always; pretty much just saved my butt cc: <a href="https://twitter.com/tomasekeli">@tomasekeli</a></p>&mdash; Sirar Salih (@sirarsalih) <a href="https://twitter.com/sirarsalih/status/740496400140636160">June 8, 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Amazingly enough, from the HTML itself you can define which parts you want on a new page. HTML is capable of determining a page break, and you can do this in different orders. Say hello to:

```
page-break-before
page-break-after
page-break-inside
```

In my case I was interested in [page-break-before](http://www.w3schools.com/cssref/pr_print_pagebb.asp), as I wanted to break the page just before displaying the content. Setting page-break-before to always will do just that. Here is an example of usage:

```html
<div style="page-break-before: always;">
<!---content--->
</div>
```