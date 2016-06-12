---
layout: post
title: TypeScript Release Candidate
date: 2014-03-02 10:27:26.000000000 +01:00
type: post
published: true
status: publish
categories:
- Development
tags:
- JavaScript
- TypeScript
- TypeScript RC
- Visual Studio
- Visual Studio 2013
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
<p>A release candidate version of <a href="http://www.typescriptlang.org/" title="TypeScript">TypeScript</a> was finally <a href="http://blogs.msdn.com/b/typescript/archive/2014/02/25/announcing-typescript-1-0rc.aspx" title="TypeScript RC">released last week</a>. New features have been added along the many bug fixes that were made. As of the Spring Update CTP2 for Visual Studio 2013, TypeScript is now fully supported making it a first class citizen. That means after getting the Spring Update CTP2, you won't need to download TypeScript as a plugin to Visual Studio 2013. Notable features that have been added are simpler, more generic type declarations and order declaration merging of interfaces. Improvements to the <code>lib.d.ts</code> typings library have also been made, adding typings support for touch and WebGL development, making your life easier when working with HTML 5.</p>
<p><strong>Generic Type System</strong></p>
<p>The typing has been enhanced, making it more flexible. It is now possible to use <code>any</code> more freely, making type checking especially softer when working with inheritance. Consider the class <code>Person</code> that extends <code>Human</code>:<br />
[code language="JavaScript"]<br />
class Human {<br />
    eyeColor: string;<br />
}</p>
<p>class Person extends Human {<br />
    eyeColor: any;<br />
}<br />
[/code]<br />
Giving the property <code>eyeColor</code> type <code>any</code> in the subclass was not possible prior to the RC version. Giving this possibility now, you are not forced to define the specific type found in the super class, which may be inaccessible at times. When it comes to generics, the same thing is possible. Consider the interface <code>IPerson</code> with a generic function and the implementing class <code>Person</code>:<br />
[code language="JavaScript"]<br />
interface IPerson&lt;Value&gt; {<br />
    then&lt;T&gt;(f: (v: Value) =&gt; Person&lt;T&gt;): Person&lt;T&gt;;<br />
}</p>
<p>class Person&lt;Value&gt; implements IPerson&lt;Value&gt; {<br />
    then&lt;T&gt;(f: (v: Value) =&gt; Person&lt;T&gt;): Person&lt;T&gt;;<br />
    then&lt;T&gt;(f: (v: Value) =&gt; any): Person&lt;T&gt; {        // This is also possible<br />
        return undefined;<br />
    }<br />
}<br />
[/code]</p>
<p><strong>Declaration Merging Precedence</strong></p>
<p>In addition, declaration merging of interfaces is now also possible. This makes precedence order easier when you work with external libraries. Say you have external interfaces <code>IExternal</code> with some functions:<br />
[code language="JavaScript"]<br />
interface IExternal {<br />
    a1(): void;<br />
    a2(): number;<br />
}</p>
<p>interface IExternal {<br />
    b1(): string;<br />
    b2(): Date;<br />
}<br />
[/code]<br />
When these two interfaces are merged, they become this interface:<br />
[code language="JavaScript"]<br />
interface IExternal {<br />
    b1(): string;<br />
    b2(): Date;<br />
    a1(): void;<br />
    a2(): number;<br />
}<br />
[/code]<br />
Notice the precedence order of the declared functions. Declaration merging is absolutely recommended, especially when you don't want to change how the external libraries are initially referenced in your web application.</p>
<p>These are awesome features from the TypeScript team! I am happy that the community influenced the team to add these features, and I also would like to congratulate the team on reaching version 1.0. Well done! :)</p>
