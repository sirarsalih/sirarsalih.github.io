---
layout: post
title: The World of TypeScript
date: 2013-12-06 23:45:53.000000000 +01:00
type: post
published: true
status: publish
categories:
- Development
tags:
- JavaScript
- TypeScript
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
<p>So I finally got around to play with <a href="http://www.typescriptlang.org/" title="TypeScript">TypeScript</a>, a (optionally) typed scripting language that compiles to JavaScript. TypeScript is great especially because of how easy it is to use to bring object oriented design into your JavaScript code. TypeScript code is written in TS formatted files that get compiled into JS. It's quite easy to learn the syntax and get started with the language. It's open source and you can get it as a <a href="http://www.microsoft.com/en-us/download/details.aspx?id=34790" title="Plugin for Visual Studio">plugin for Visual Studio</a>, which gives full debugging capabilities and rich editor tooling. After installing the plugin, you can either start a TypeScript project in Visual Studio or add a TypeScript file into an existing web solution. Once saved, the TS file gets compiled and a JS file gets added inside your project folder (outside of solution explorer).</p>
<p><strong>Object Orientation</strong></p>
<p>Writing object oriented code in TypeScript is straightforward. A class <code>Person</code> with properties and a method can be written as follows:<br />
[code language="javascript"]<br />
class Person {</p>
<p>    firstName: string;<br />
    lastName: string;<br />
    age: number;</p>
<p>    constructor(firstName: string, lastName: string, age: number) {<br />
        this.firstName = firstName;<br />
        this.lastName = lastName;<br />
        this.age = age;<br />
    }    </p>
<p>    GetFullNameAndAge() {<br />
        return this.firstName + &quot; &quot; + this.lastName &quot;, &quot; + this.age;<br />
    }<br />
}<br />
[/code]<br />
Using the <code>public</code> keyword on properties, you can also inject the properties in the constructor and write the same, above class as follows:<br />
[code language="javascript"]<br />
class Person {</p>
<p>    constructor(public firstName: string, public lastName: string,<br />
                public age: number) {<br />
    }    </p>
<p>    GetFullNameAndAge() {<br />
        return this.firstName + &quot; &quot; + this.lastName + &quot;, &quot; + this.age;<br />
    }<br />
}<br />
[/code]<br />
You can easily create an interface to hide implementation. Consider an interface <code>IPerson</code>:<br />
[code language="javascript"]<br />
interface IPerson {<br />
    GetFullNameAndAge();<br />
}</p>
<p>class Person implements IPerson {</p>
<p>    constructor(public firstName: string, public lastName: string,<br />
                public age: number) {<br />
    }    </p>
<p>    GetFullNameAndAge() {<br />
        return this.firstName + &quot; &quot; + this.lastName + &quot;, &quot; + this.age;<br />
    }<br />
}<br />
[/code]<br />
Inheritance is also quite easy. Consider the base class <code>Human</code>:<br />
[code language="javascript"]<br />
class Human {</p>
<p>    constructor(public eyeColor: string) {<br />
    }</p>
<p>    GetEyeColor() {<br />
        return this.eyeColor;<br />
    }<br />
}</p>
<p>interface IPerson {<br />
    GetFullNameAndAge();<br />
}</p>
<p>class Person extends Human implements IPerson {</p>
<p>    constructor(public firstName: string, public lastName: string,<br />
                public age: number, eyeColor: string) {<br />
        super(eyeColor);<br />
    }    </p>
<p>    GetFullNameAndAge() {<br />
        return this.firstName + &quot; &quot; + this.lastName + &quot;, &quot; + this.age;<br />
    }<br />
}<br />
[/code]<br />
Now see how this code looks when it gets compiled to JavaScript:<br />
[code language="javascript"]<br />
var __extends = this.__extends || function (d, b) {<br />
    for (var p in b) if (b.hasOwnProperty(p)) d[p] = b[p];<br />
    function __() { this.constructor = d; }<br />
    __.prototype = b.prototype;<br />
    d.prototype = new __();<br />
};<br />
var Human = (function () {<br />
    function Human(eyeColor) {<br />
        this.eyeColor = eyeColor;<br />
    }<br />
    Human.prototype.GetEyeColor = function () {<br />
        return this.eyeColor;<br />
    };<br />
    return Human;<br />
})();</p>
<p>var Person = (function (_super) {<br />
    __extends(Person, _super);<br />
    function Person(firstName, lastName, age, eyeColor) {<br />
        _super.call(this, eyeColor);<br />
        this.firstName = firstName;<br />
        this.lastName = lastName;<br />
        this.age = age;<br />
    }<br />
    Person.prototype.GetFullNameAndAge = function () {<br />
        return this.firstName + &quot; &quot; + this.lastName + &quot;, &quot; + this.age;<br />
    };<br />
    return Person;<br />
})(Human);<br />
[/code]<br />
A bit terrifying, huh?</p>
<p><strong>Modules</strong></p>
<p>It's possible to structure your TypeScript code in modules, which is a way of code isolation. Modules have many advantages, such as scoping (local vs. global scope), encapsulation, testability‎ and many other things. There are two types of modules in TypeScript; internal and external. </p>
<p><strong>Internal modules</strong></p>
<p>An internal module is the code itself that you write in TypeScript, anything you type is globally scoped and available throughout your code. If we instantiate our class <code>Person</code>, it will be globally available throughout our code:<br />
[code language="javascript"]<br />
var person = new Person(&quot;John&quot;, &quot;Smith&quot;, 26, &quot;Brown&quot;);<br />
[/code]</p>
<p>However, if we place our class inside of a module <code>Races</code>, everything inside of it becomes locally scoped. If we then like to instantiate the class out of the local scope, we need to use the keyword <code>export</code> on the class:</p>
<p>[code language="javascript"]<br />
module Races {<br />
	export class Person extends Human implements IPerson {</p>
<p>	    constructor(public firstName: string, public lastName: string,<br />
	                public age: number, eyeColor: string) {<br />
	        super(eyeColor);<br />
	    }    </p>
<p>	    GetFullNameAndAge() {<br />
	        return this.firstName + &quot; &quot; + this.lastName + &quot;, &quot; + this.age;<br />
	    }<br />
	}<br />
}</p>
<p>var person = new Races.Person(&quot;John&quot;, &quot;Smith&quot;, 26, &quot;Brown&quot;);<br />
[/code]<br />
Internal modules can also be shared across files, other classes can make a reference to them by typing (at the top of file) for instance:<br />
[code language="javascript"]<br />
///&lt;reference path=&quot;Races.ts&quot;/&gt;<br />
[/code]</p>
<p><strong>External modules</strong></p>
<p>An external module is an outside module that you choose to import into your code in order to use it. Consider the example:<br />
[code language="javascript"]<br />
import Ethnicities = module('Ethnicities');</p>
<p>class Person {</p>
<p>    constructor() {<br />
    }</p>
<p>    GetEthnicity(country: string) {<br />
        return new Ethnicities.Ethnicity(country);<br />
    }<br />
}<br />
[/code]<br />
And in a different file called "Ethnicities.ts" we have:<br />
[code language="javascript"]<br />
export class Ethnicity {</p>
<p>    constructor(public country: string) {<br />
    }<br />
}<br />
[/code]<br />
I recommend checking out the <a href="http://www.typescriptlang.org/Playground/" title="TypeScript playground">TypeScript playground</a> where you can do some experimentation with the codes above, and be able to view the compiled JavaScript.</p>
<p><strong>Thoughts and the Road Ahead</strong></p>
<p>TypeScript is an awesome OO scripting language that brings your JavaScript to a new level, it's a language that I will definitely be using further in web development. There are many other typical OO things that you can do in TypeScript than what I've shown. The list of goodies keeps getting bigger, <a href="http://typescript.codeplex.com/wikipage?title=Roadmap" title="Roadmap TypeScript">here</a> you can see a roadmap of upcoming versions of the language. Hope you enjoyed this and happy scripting!</p>
