---
layout: post
title: Directives in AngularJS
date: 2014-02-17 10:46:30.000000000 +01:00
type: post
published: true
status: publish
categories:
- Development
tags:
- AngularJS
- AngularJS Directives
- Directives
- HTML
- JavaScript
meta:
  _edit_last: '54045106'
  _publicize_pending: '1'
  _oembed_5dcffb1b1a5c2234d86b71eef51d0ec6: "{{unknown}}"
  _oembed_35e09d5a698a3879252451bc19750a30: "{{unknown}}"
  _oembed_6a4d88ffa813aabdc5646ece963091fa: "{{unknown}}"
  _oembed_1bd1edbbf4fa48efb2fe0f476a13e279: "{{unknown}}"
  _oembed_346ef3424dbedd2ec659ffcd1a013d8d: "{{unknown}}"
  _oembed_3e6deb4edf7c9594ecb7c4a089fed209: "{{unknown}}"
  _oembed_dad390e147d6846ffd4015ee66a8d4c7: "{{unknown}}"
  _oembed_73407ac18fe2c1e8b2b8d78d298ac976: "{{unknown}}"
  _oembed_5872f529147dbba62bee05573c6c7e7d: "{{unknown}}"
  _oembed_c12a153fd98a1b7c911e14e81d3dd05e: "{{unknown}}"
  _oembed_feddf3d5154b507a773b8fe70946b6e2: "{{unknown}}"
  _oembed_dd96eccd1bd06b96a15b685f52a5719c: "{{unknown}}"
  _oembed_a7d029c4276ccbfdfb14cf8870960ccd: "{{unknown}}"
  _oembed_d6500821dc3f4e72c334f7ce6a85f711: "{{unknown}}"
  _oembed_24f217088f36ce0debe6d0f4da7973fb: "{{unknown}}"
  _oembed_ac96852435708281cc3eaeca80ab59fa: "{{unknown}}"
  _oembed_817b8826a23b779fc564660f88be076e: "{{unknown}}"
  _oembed_3d6eabf80610826ecfaddccd90c8aaf8: "{{unknown}}"
  _oembed_224f4c1ee49043f1aba2381834402884: "{{unknown}}"
  _oembed_c99b89a4de7b05538c8258bac3d7aa64: "{{unknown}}"
  _oembed_9e463b9433a40a7235be26aaac7370c4: "{{unknown}}"
  _oembed_75ae93638290fe17d4b4d74fb5773972: "{{unknown}}"
  _oembed_3c67ee577ef286e4f7e019695fbb32ad: "{{unknown}}"
  _oembed_d14e9d4c72b7dcbd86b03b1b94b7ab99: "{{unknown}}"
  _oembed_7b00f9a31da55ea0a5f171f19e7eddb9: "{{unknown}}"
  _oembed_f69134add7f83e265198b7e0623eaf0a: "{{unknown}}"
  _oembed_990d239c4e3e041ca91a67b9bde17715: "{{unknown}}"
  _oembed_e22503518d26c15df45323efb85ae86f: "{{unknown}}"
  _oembed_faf641bc6d11f569c2c1df3306ce56eb: "{{unknown}}"
author:
  login: sirars
  email: sirars@gmail.com
  display_name: Sirar Salih
  first_name: ''
  last_name: ''
---
<p>So I thought that I'd make a detailed post about <a href="http://docs.angularjs.org/guide/directive" title="directives">directives</a> in <a href="http://angularjs.org/" title="AngularJS">AngularJS</a>. If you've just started out with AngularJS, chances are you are curious to learn about this powerful feature. Directives are great if you want to create your own custom HTML elements. In fact, directives give us a glimpse of the future. In HTML 5, we got new elements and the goal is to increase this in the future so you won't need to use the <code>class</code> attribute so often. Learning to use directives may seem a bit complex for the AngularJS beginner, but trust me, it's really easy once you get the hang of it. Using directives isn't really mandatory when you work with AngularJS, unfortunately this leads to developers not using this feature. I hope to change that with my post, and make you use it if you don't already.</p>
<p>A directive can be written in four different ways; as an element, attribute, comment or class. The best practice is to write it as an element or attribute. Here are the four ways a directive can be written in HTML:</p>
<p>[code language="HTML"]<br />
&lt;my-directive&gt;&lt;/my-directive&gt;<br />
&lt;div my-directive&gt;&lt;/div&gt;<br />
&lt;!-- directive: my-directive --&gt;<br />
&lt;div class=&quot;my-directive&quot;&gt;&lt;/div&gt;<br />
[/code]</p>
<p>The directive is loaded into Angular at the start up of a web application. Usually in a <code>bootstrap.js</code> where you load your other Angular components, such as controllers and factories. The directive is loaded like such (line 2):<br />
[code language="JavaScript"]<br />
var myApp = angular.module('myApp', []);<br />
myApp.directive('myDirective', myDirective);<br />
function myDirective() {<br />
    return &quot;&quot;<br />
};<br />
[/code]<br />
Note that in the HTML we called the directive <code>my-directive</code>, this is transformed to camel case and therefore we load the directive as <code>myDirective</code> in Angular. As you can see, this directive doesn't do much. It simply returns an empty string. A directive function has up to eleven different attributes that can be used. It has <code>priority</code>, <code>template</code>, <code>templateUrl</code>, <code>replace</code>, <code>transclude</code>, <code>restrict</code>, <code>scope</code>, <code>controller</code>, <code>require</code>, <code>link</code> and <code>compile</code>. By no means will you be using all of those attributes at once, usually you would use three to four attributes depending on your web application. I'm going to give a short description of each attribute. </p>
<p><code>Priority</code> defines the order of how the directives get compiled, this is the case when you have created multiple directives on a single DOM element. Directives with a greater priority number get compiled first. </p>
<p><code>Template</code> is used to add your HTML template code that you want to be shown, and <code>templateUrl</code> is a URL to your HTML template. </p>
<p><code>Replace</code> is used to either replace the directive element with your HTML template (true) or to replace the contents of it (false). This is by default set to false.  </p>
<p>Transclusion is a sibling of the isolate scope (which I will explain soon), which means that it's bound to the parent scope. This makes the content of your directive have its own private state. <code>transclude</code> can be set to true (transclude the content) or to 'element' (transclude the whole directive). </p>
<p><code>Restrict</code> is used to define the type of your directive. It can be set to <code>E</code> (element), <code>A</code> (attribute), <code>M</code> (comment) or <code>C</code> (class). The default value is <code>A</code>. </p>
<p>By using <code>scope</code>, you can define an isolated scope for your directive content. If scope is not used, the directive will use the parent scope by default. <code>Scope</code> can be set to true (new scope created for the directive), false (use parent scope), pass in scope for two-way binding, pass in type and other custom attributes. </p>
<p><code>Controller</code> is used if you want your directive to have its own controller. You simply set it with the name of the controller that you've loaded in Angular. </p>
<p>Use <code>require</code> if you want to have a dependency on other directives, in that case inject the dependency directive's controller as the fourth argument in the <code>link</code> function (which we will get to shortly). <code>require</code> is set to the name of the dependency directive's controller, if there are several dependency directives, then an array is used containing the controller names. </p>
<p><code>Link</code> is used to define directive logic, it is responsible for registering DOM listeners and updating the DOM. <code>Link</code> takes in several arguments, and it is here where you define your directive logic. However, this attribute is unnecessary if you are using the <code>controller</code> attribute and got the directive logic inside a controller.</p>
<p><code>Compile</code> is used to transform the template DOM. <code>ngRepeat</code> is an example of a template DOM that needs to be transformed into HTML. Since most directives do not transform DOM templates, this attribute is not often used. <code>Compile</code> takes in several arguments. </p>
<p>For more information on each of the directive attributes, I suggest checking the <a href="http://docs.angularjs.org/api/ng.$compile" title="AngularJS documentation">AngularJS documentation</a>.</p>
<p>Time for examples of usage, let's assume we have written the following directive in the HTML:<br />
[code language="HTML"]<br />
&lt;my-directive&gt;&lt;/my-directive&gt;<br />
[/code]<br />
Now, in the JavaScript this directive is implemented like this (line 3):<br />
[code language="JavaScript"]<br />
var myApp = angular.module('myApp', []);<br />
myApp.directive('myDirective', myDirective);<br />
function myDirective() {<br />
    return {<br />
    restrict: 'EA',<br />
    template: '&lt;div&gt;Hello World!&lt;/div&gt;'<br />
    }<br />
};<br />
[/code]<br />
I set <code>restrict</code> to <code>'EA'</code> on purpose, to show you that you can make the directive implementation work on both an element and an attribute if you wish. This is a simple directive that returns a template div with the text "Hello World!". Now, we can extract this template to a file:<br />
[code language="JavaScript"]<br />
function myDirective() {<br />
    return {<br />
    restrict: 'EA',<br />
    templateUrl: 'helloWorld.html'<br />
    }<br />
};<br />
[/code]<br />
And the content of <code>helloWorld.html</code>:<br />
[code language="HTML"]<br />
&lt;div&gt;Hello World!&lt;/div&gt;<br />
[/code]<br />
Let's put our directive inside a controller:<br />
[code language="HTML"]<br />
&lt;div ng-controller=&quot;myController&quot;&gt;<br />
&lt;my-directive&gt;&lt;/my-directive&gt;<br />
&lt;/div&gt;<br />
[/code]<br />
The controller is simple, it looks like this:<br />
[code language="JavaScript"]<br />
myApp.controller('myController', myController);<br />
function myController($scope) {<br />
    $scope.message = &quot;Hello World!&quot;;<br />
};<br />
[/code]<br />
Now, we change the directive:<br />
[code language="JavaScript"]<br />
function myDirective() {<br />
    return {<br />
    restrict: 'EA',<br />
    template: '{{message}}'<br />
    }<br />
};<br />
[/code]<br />
Can you guess what happens? The directive uses the parent controller and scope, and returns the message defined in the controller - "Hello World!". Now let's create a controller specifically for our directive, and call it <code>myDirectiveController</code>:<br />
[code language="JavaScript"]<br />
myApp.controller('myDirectiveController', myDirectiveController);<br />
function myDirectiveController($scope) {<br />
    $scope.directiveMessage = &quot;Hello World!&quot;;<br />
};<br />
[/code]<br />
And change the directive to use this controller and an isolated scope:<br />
[code language="JavaScript"]<br />
function myDirective() {<br />
    return {<br />
    restrict: 'EA',<br />
    scope: true,<br />
    controller: 'myDirectiveController',<br />
    template: '{{directiveMessage}}'<br />
    }<br />
};<br />
[/code]<br />
Again, the directive will return the message "Hello World!", this time from its own controller. If you want to pass parameters from the parent controller, that's possible. Let's assume that the parent controller <code>myController</code> has a <code>messages</code> collection, that we loop through in the HTML:<br />
[code language="HTML"]<br />
&lt;div ng-controller=&quot;myController&quot;&gt;<br />
&lt;div ng-repeat=&quot;message in messages&quot;&gt;<br />
&lt;my-directive message=&quot;message&quot;&gt;&lt;/my-directive&gt;<br />
&lt;/div&gt;<br />
&lt;/div&gt;<br />
[/code]<br />
We loop through the collection and pass each <code>message</code> to the directive. The directive then looks like this:<br />
[code language="JavaScript"]<br />
function myDirective() {<br />
    return {<br />
    restrict: 'EA',<br />
    scope: {<br />
    message: '='<br />
    },<br />
    controller: 'myDirectiveController',<br />
    template: '{{message}}'<br />
    }<br />
};<br />
[/code]<br />
The <code>message</code> is passed to the directive controller, by setting the message attribute in the <code>scope</code> to '='. This can be done differently, you can type <code>message="myMessage"</code> in the div (instead of <code>message="message"</code>), in that case you need to add <code>message: '=myMessage'</code> in the <code>scope</code>. Using '=?' means that a parameter is optional. Another thing you can do is send in <code>type</code>. You use the <code>type</code> attribute in the HTML, then add <code>type: '@'</code> in the <code>scope</code>. </p>
<p>Another thing you can do of course, is to use the <code>link</code> attribute to define your logic there. Let's do that:<br />
[code language="JavaScript"]<br />
function myDirective() {<br />
    return {<br />
    restrict: 'EA',<br />
    link: function(scope, element, attributes) {<br />
        element.addClass('alert');<br />
    }<br />
};<br />
[/code]<br />
<code>Link</code> takes in current scope as argument, the directive element itself and custom attributes that you can pass in. <code>Link</code> uses jQuery Lite, a lighter version of <a href="http://jquery.com/" title="jQuery">jQuery</a>. You can do jQuery operations inside of this function, in this case we are adding a css class <code>alert</code> to the directive element. You can do a lot of other different things with directives, for more I suggest checking out the <a href="http://docs.angularjs.org/api/ng.$compile" title="AngularJS documentation">AngularJS documentation</a>.</p>
<p>I've explained most of what you need to know about directives. When you write directives, I recommend that you create controllers specifically for the directives. Put all logic in the controllers, and do not use the <code>link</code> function. Avoid jQuery operations and stick to pure Angular. That way you keep your code clean and nicely isolated. I hope that this gives you a good introduction to directives, and helps you get started using them. As you can see, it's not that hard, is it? :)</p>
