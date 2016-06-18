---
layout: post
title: Anonymous Functions in JavaScript are Evil
date: 2014-01-10 14:24:20.000000000 +01:00
type: post
published: true
status: publish
categories:
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
Anonymous functions in JavaScript can cause a lot of headache and pain. If you want to write testable JavaScript, you will spend days scratching your head to find a way to assert logic that resides in an anonymous function. The problem is that we have no control over what happens inside the anonymous function, unless we stub the sucker out and choose whatever happens after the function is called. But that's not really a smooth way to do it, is it? There are ways to get around this issue, but some of them are messy in my opinion. There isn't really a perfect way to do it, but I will show you two different ways that I know are possible.

Consider ```studentsController.js``` which has a function ```getStudents()``` that calls an injected service ```studentService```:

```javascript
var getStudents = function() {
    var _this = this;
    this.studentService.getAll(function (data) {
	_this.handle(data);
    });
};
```

<p>We are testing the <code>getStudents()</code> function, how do we assert that the <code>handle()</code> function was called or that the data was updated correctly? It's happening in an anonymous function call.</p>
<strong>"Testing hooks"</strong>

One way to do it is to use so called "testing hooks". This is in my opinion a messy way, since you have to insert code into the logic just to handle testing. What is possible however, is to make a post script that removes all testing hooks prior to production release. That's still extra work to do though. You attach a hook onto the ```studentService.getAll()``` function like the following (line 6):

```javascript
var getStudents = function() {
    var _this = this;
    this.studentService.getAll(function (data) {
	_this.handle(data);
    });
    arguments.callee.anonymous = this.studentService.getAll;
};
```

Now, in your test you need to call ```getStudents()``` once. Then you can control the data that goes inside the anonymous function like so:

```javascript
getStudents.anonymous(data)
```

<p><strong>Stubbing Your Way Down the Dependency Rabbit Hole</strong></p>
Another possible way is to literally stub your way down the dependency hierarchy and spy on dependencies that get called. This is a painful way. You need an API to create fake objects, assume that we use <a href="http://pivotal.github.io/jasmine/" title="Jasmine">Jasmine</a>. In this case, you mock an object - http - in your test that gets called by ```studentService.getAll()```, like so:

```javascript
var httpObject = jasmine.createSpyObj('http', ['get']);
httpObject.get.andReturn("data");
```

Then you call ```getStudents()``` once and expect that the mock was called:

```javascript
expect(getStudents()).toBe("data");
```
  
<p>There are probably other ways to get around this, but I have found that option one is the simplest way. I'm also quite comfortable with it, as long as those hooks get removed pre-production. :)</p>
