---
layout: post
title: ReSharper Downfalls and Anti-Patterns
date: 2014-03-25 13:47:14.000000000 +01:00
type: post
published: true
status: publish
categories:
- Development
tags:
- JetBrains Resharper
- Refactoring
- ReSharper
- Visual Studio
meta:
  _edit_last: '54045106'
  _publicize_pending: '1'
author:
  login: sirars
  email: sirars@gmail.com
  display_name: Sirar Salih
  first_name: ''
  last_name: ''
---
<p>As part of a dedicated refactoring team in a big customer project, I get to use <a href="http://www.jetbrains.com/resharper/" title="JetBrains ReSharper">JetBrains ReSharper</a> quite heavily on a daily basis. If you didn't know already, ReSharper is the best refactoring tool made for <a href="http://www.visualstudio.com/" title="Visual Studio">Visual Studio</a>. Not only does it increase programming efficiency by multitudes, it also changes the way you think as a programmer. <em>Especially</em> when you've just started out on your programming career. I've been using ReSharper professionally for at least three years, and today I can't even imagine how I survived as a (.NET) programmer without it. </p>
<p>Ironically, it wasn't until I joined a professional refactoring team that I discovered the downfalls and anti-patterns of using ReSharper. Without doubt, the tool is continuously being developed by a brilliant team, so the downfalls that I see today may very well be investigated in the next versions of the tool. There are also some features that ReSharper simply lack today, that I hope will be added in the future. I will explain what I am talking about in the next sections.</p>
<p><strong>Helper Methods are Extracted as Static by Default</strong></p>
<p>If you press <code>Ctrl+RM</code> you'll get the option to extract a method. A local helper method is by default extracted as <code>static</code>:</p>
<p><a href="http://sirars.files.wordpress.com/2014/03/capture.png"><img src="http://sirars.files.wordpress.com/2014/03/capture.png" alt="Capture" width="440" height="313" class="alignnone size-full wp-image-353" /></a></p>
<p>You get to choose whether to make it static, but it is <em>static by default</em>. You'll find that this is extremely annoying, as there is a chance that you have to insert non-static content in the method at a later time. So, the fix? Remove the static part manually (!). Note, in the following animation below, the lack of intellisense as I start typing the <code>_webRole</code> object:</p>
<p><img src="http://sirars.files.wordpress.com/2014/03/webrole1.gif" alt="animation" width="527" height="112" /></p>
<p>This may not be a big deal in a small project, but it quickly becomes cumbersome in a big legacy application.</p>
<p><strong>Lack of Listing Multiple Object Properties Feature</strong></p>
<p>Very often you will create objects and want to set their properties. There is no way to list all the properties of the created object, leaving you having to manually type and set each property:</p>
<p><img src="http://sirars.files.wordpress.com/2014/03/properties.gif" alt="properties" width="568" height="194" /></p>
<p>Wouldn't it be nice to have ReSharper list all the properties for us?</p>
<p><strong>Lack of "Find Usages in Multiple Solutions" Feature</strong></p>
<p>One of the most fundamental things when you are refactoring a huge application with many solutions, is the ability to locate the usage of a component across those solutions. If you right click on a class or method and choose "Find Usages Advanced...":</p>
<p><a href="http://sirars.files.wordpress.com/2014/03/find_usages.png"><img src="http://sirars.files.wordpress.com/2014/03/find_usages.png" alt="find_usages" width="440" height="499" class="alignnone size-full wp-image-370" /></a></p>
<p>ReSharper will show you a dialog where you can choose where to search:</p>
<p><a href="http://sirars.files.wordpress.com/2014/03/find_usages_2.png"><img src="http://sirars.files.wordpress.com/2014/03/find_usages_2.png" alt="find_usages_2" width="440" height="354" class="alignnone size-full wp-image-372" /></a></p>
<p>What would be nice is to have the option "Solutions..." where you can specify solutions or simply choose all. This makes our life easier, and saves us the need to open each solution manually and perform the search per solution.</p>
<p><strong>Refactoring Overkill</strong></p>
<p>This one falls under anti-patterns. Every now and then, ReSharper will suggest refactorings that actually break readability of your code. Inverted ifs and complicated LINQ expressions fall under this category. How many times has ReSharper asked you to invert your if statement when it looked perfectly readable? Or how many refactorings were you asked to do by ReSharper on your one single LINQ expression? Chances are, many times. There is no need to invert your if, if you and your code reviewer agree that it looks fine. Although you can probably turn off this type of suggestion, ReSharper should be intelligent enough <em>not</em> to ask you to do this.</p>
<p>Complicated LINQ expressions, where do I start? Once you write a LINQ expression that does something, ReSharper will often suggest to write it differently. As the expression gets more complicated, so does ReSharper. It will ask you to keep refactoring, sometimes up to 3 times (!) on a single LINQ expression. The ending result is a hideous piece of code that takes time to understand. So again, ReSharper should be intelligent enough to take readability into account here.</p>
<p><strong>Different Key Binding Schemes?!</strong></p>
<p>Something that has annoyed me recently is that the key binding schemes of ReSharper seem to vary from environment to another. Ever since I installed ReSharper 8.1 (which I <a href="http://www.jetbrains.com/resharper/download/" title="upgraded to 8.2">upgraded to 8.2 today</a>, by the way), the schema on my development machine has changed, and I have to memorize different key bindings. This is a hassle when I do pair programming with another developer using his machine, as he would have the older schema. Ultimately I memorized both schemes in order to work efficiently. You would think that simply applying the Visual Studio schema would make things consistent in all developer machines:</p>
<p><a href="http://sirars.files.wordpress.com/2014/03/capture1.png"><img src="http://sirars.files.wordpress.com/2014/03/capture1.png" alt="Capture" width="440" height="330" class="alignnone size-full wp-image-376" /></a></p>

In final words, ReSharper is a wonderful refactoring tool that I will be using for years and years to come. That doesn't by any means make it perfect, and there are still features that I see lacking. Seeing how the ReSharper team is doing an incredible job developing the product (<a href="http://www.jetbrains.com/resharper/download/" title="ReSharper 8.2">ReSharper 8.2 was just released</a>), I'm not worried that they'll look into this and make our favorite tool even better. :)

PS: I used [LICEcap](http://www.cockos.com/licecap/) to create the gif animations in this post.