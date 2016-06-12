---
layout: post
title: Scripting with C#
date: 2013-12-22 22:07:41.000000000 +01:00
type: post
published: true
status: publish
categories:
- Development
tags:
- C#
- Command-line interface
- Integrated development environment
- ScriptCS
- Text editor
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
<p>Have you ever wondered if you could one day write C# code without the need of a fully featured IDE? Well, with <a href="http://scriptcs.net/" title="ScriptCS">ScriptCS</a> you can do just that. ScriptCS makes it possible to write C# without the need of an IDE such as Visual Studio, this is very useful if you want to do fast writes of C# without having to launch up VS each time. With ScriptCS, you can write C# in any text editor you like - including command line. And you can still use NuGet for third party libraries. Pretty cool, isn't it? You can also do things like bootstrapping external frameworks and so on. I just recently got to play around with ScriptCS, so naturally I thought to write up a tutorial on how to set up and start using this.</p>
<p><strong>Chocolatey Gallery</strong></p>
<p>ScriptCS is installed through the <a href="http://chocolatey.org/" title="Chocolatey">Chocolatey</a> gallery. To install Chocolatey, start admin cmd and type the following command:</p>
<pre>@powershell -NoProfile -ExecutionPolicy Unrestricted -Command "iex ((New-Object Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" &amp;&amp; SET PATH=%PATH%;%systemdrive%\chocolatey\bin</pre>
<p>Once Chocolatey is installed, you can install ScriptCS by typing in cmd:</p>
<pre>cinst scriptcs</pre>
<p>That's it! To use ScriptCS, fire up cmd and simply type <code>scriptcs</code>. Then you can start writing C# code:</p>
<p><a href="http://sirars.files.wordpress.com/2013/12/capture.png"><img src="{{ site.baseurl }}/assets/capture.png?w=300" alt="Capture" width="300" height="152" class="alignnone size-medium wp-image-180" /></a></p>
<p>Now, let's create something more interesting. Create a new script file called <code>Person.csx</code> and save it in an empty folder. You can use any text editor you like, I prefer <a href="http://notepad-plus-plus.org/download/v6.5.2.html" title="Notepad++">Notepad++</a>. Write the following C# code in your script:</p>
<p>[code language="csharp"]<br />
class Person {</p>
<p>	private string _firstName;<br />
	private string _lastName;</p>
<p>	public Person(string firstName, string lastName){<br />
		_firstName = firstName;<br />
		_lastName = lastName;<br />
	}</p>
<p>	private string GetName(){<br />
		return _firstName + &quot; &quot; + _lastName;<br />
	}</p>
<p>	public string Greet() {<br />
		return &quot;Hello, &quot; + GetName();<br />
	}</p>
<p>}</p>
<p>var person = new Person(&quot;Jon&quot;, &quot;Stark&quot;);<br />
Console.WriteLine(person.Greet());<br />
[/code]</p>
<p>Note how the object is created outside the class, and printed out to console. Now fire up cmd and execute your script by typing <code>scriptcs Person.csx</code>:</p>
<p><a href="http://sirars.files.wordpress.com/2013/12/capture2.png"><img src="{{ site.baseurl }}/assets/capture2.png?w=300" alt="Capture2" width="300" height="152" class="alignnone size-medium wp-image-183" /></a></p>
<p>Isn't this cool? Imagine the possibilities. You can now write clean C# code in script form in any text editor you like, without binding yourself to a heavy IDE. Hope you enjoyed this tutorial, and good luck!</p>
<p><strong>ScriptCS-GUI</strong></p>
<p>It's also worth mentioning that I stumbled upon a toolkit for ScriptCS that allows you to create GUI forms, this is quite useful when you're writing Windows applications. ScriptCS-GUI is a pretty nice framework, I recommend checking out <a href="http://hemme.github.io/scriptcs-gui/" title="ScriptCS-GUI">their site here</a>.</p>
