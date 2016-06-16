---
layout: post
title: The World of TypeScript
date: 2013-12-06 23:45:53.000000000 +01:00
type: post
published: true
status: publish
categories:
- Development
tags: [js]
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
Writing object oriented code in TypeScript is straightforward. A class ```Person``` with properties and a method can be written as follows:
    
```javascript
class Person {

firstName: string;
lastName: string;
age: number;

constructor(firstName: string, lastName: string, age: number) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.age = age;
}

GetFullNameAndAge() {
    return this.firstName + " " + this.lastName ", " + this.age;
    }
}
```

Using the ```public``` keyword on properties, you can also inject the properties in the constructor and write the same, above class as follows:

```javascript
class Person {

constructor(public firstName: string, public lastName: string,
public age: number) {
}

GetFullNameAndAge() {
return this.firstName + " " + this.lastName + ", " + this.age;
}
}
```

Inheritance is also quite easy. Consider the base class ```Human```:

```javascript
interface IPerson {
GetFullNameAndAge();
}

class Person implements IPerson {

constructor(public firstName: string, public lastName: string,
public age: number) {
}

GetFullNameAndAge() {
return this.firstName + " " + this.lastName + ", " + this.age;
}
}
```

Now see how this code looks when it gets compiled to JavaScript:

```javascript
var __extends = this.__extends || function (d, b) {
for (var p in b) if (b.hasOwnProperty(p)) d[p] = b[p];
function __() { this.constructor = d; }
__.prototype = b.prototype;
d.prototype = new __();
};
var Human = (function () {
function Human(eyeColor) {
this.eyeColor = eyeColor;
}
Human.prototype.GetEyeColor = function () {
return this.eyeColor;
};
return Human;
})();

var Person = (function (_super) {
__extends(Person, _super);
function Person(firstName, lastName, age, eyeColor) {
_super.call(this, eyeColor);
this.firstName = firstName;
this.lastName = lastName;
this.age = age;
}
Person.prototype.GetFullNameAndAge = function () {
return this.firstName + " " + this.lastName + ", " + this.age;
};
return Person;
})(Human);
```

A bit terrifying, huh?
<p><strong>Modules</strong></p>
<p>It's possible to structure your TypeScript code in modules, which is a way of code isolation. Modules have many advantages, such as scoping (local vs. global scope), encapsulation, testability‎ and many other things. There are two types of modules in TypeScript; internal and external. </p>
<p><strong>Internal modules</strong></p>
An internal module is the code itself that you write in TypeScript, anything you type is globally scoped and available throughout your code. If we instantiate our class ```Person```, it will be globally available throughout our code:

```javascript
var person = new Person("John", "Smith", 26, "Brown");
```

However, if we place our class inside of a module ```Races```, everything inside of it becomes locally scoped. If we then like to instantiate the class out of the local scope, we need to use the keyword ```export``` on the class:

```javascript
module Races {
export class Person extends Human implements IPerson {

constructor(public firstName: string, public lastName: string,
public age: number, eyeColor: string) {
super(eyeColor);
}

GetFullNameAndAge() {
return this.firstName + " " + this.lastName + ", " + this.age;
}
}
}

var person = new Races.Person("John", "Smith", 26, "Brown");
```

Internal modules can also be shared across files, other classes can make a reference to them by typing (at the top of file) for instance:

```javascript
///<reference path="Races.ts"/>
```

<p><strong>External modules</strong></p>
An external module is an outside module that you choose to import into your code in order to use it. Consider the example:

```javascript
import Ethnicities = module('Ethnicities');

class Person {

constructor() {
}

GetEthnicity(country: string) {
return new Ethnicities.Ethnicity(country);
}
}
```

And in a different file called "Ethnicities.ts" we have:

```javascript
export class Ethnicity {

constructor(public country: string) {
}
}
```

I recommend checking out the <a href="http://www.typescriptlang.org/Playground/" title="TypeScript playground">TypeScript playground</a> where you can do some experimentation with the codes above, and be able to view the compiled JavaScript.
<p><strong>Thoughts and the Road Ahead</strong>
<p>TypeScript is an awesome OO scripting language that brings your JavaScript to a new level, it's a language that I will definitely be using further in web development. There are many other typical OO things that you can do in TypeScript than what I've shown. The list of goodies keeps getting bigger, <a href="http://typescript.codeplex.com/wikipage?title=Roadmap" title="Roadmap TypeScript">here</a> you can see a roadmap of upcoming versions of the language. Hope you enjoyed this and happy scripting!</p>
