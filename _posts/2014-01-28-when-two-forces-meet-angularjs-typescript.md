---
layout: post
title: When Two Forces Meet (AngularJS, TypeScript)
date: 2014-01-28 12:38:11.000000000 +01:00
type: post
published: true
status: publish
categories:
- Development
tags:
- AngularJS
- JavaScript
- TypeScript
- Visual Studio
meta:
  _edit_last: '54045106'
  _publicize_pending: '1'
  geo_public: '0'
author:
  login: sirars
  email: sirars@gmail.com
  display_name: Sirar Salih
  first_name: ''
  last_name: ''
---
<p><a href="http://angularjs.org/" title="AngularJS">AngularJS</a> is largely growing in popularity among front-end developers. According to a <a href="http://dailyjs.com/2013/12/12/javascript-survey-results/" title="JavaScript developer survey">JavaScript developer survey</a> conducted in 2013, it was shown that AngularJS was among the top two most used JavaScript frameworks. In a world where there are close to 17 million MVC JavaScript frameworks, this puts AngularJS on top of the game. On the other end of the scale, while not as widely used as AngularJS, <a href="http://www.typescriptlang.org/" title="TypeScript">TypeScript</a> is slowly becoming the de facto compiler language for writing object oriented JavaScript. So what happens when these two forces meet? A great power emerges! We learned from Uncle Ben (Spider-Man movie) that with great power, comes great responsibility. In this tutorial, I show how these two forces can be combined to produce an ultimate web application.</p>
<p><a href="http://sirars.files.wordpress.com/2014/01/capture.png"><img src="{{ site.baseurl }}/assets/capture.png?w=300" alt="Capture" width="300" height="224" class="alignnone size-medium wp-image-279" /></a></p>
<p><strong>Setup</strong></p>
<p>As a .NET developer, I use <a href="http://www.visualstudio.com/" title="Visual Studio 2013">Visual Studio 2013</a> and the <a href="http://www.microsoft.com/en-us/download/details.aspx?id=34790" title="TypeScript plugin for Visual Studio">TypeScript plugin for Visual Studio</a>. This is what I recommend that you use unless you are for some reason against the Windows platform. Then using the NuGet packet manager, you can install the necessary <a href="http://www.nuget.org/packages/angularjs.core" title="AngularJS core files">AngularJS core files</a>. In addition to this, make sure to install the <a href="http://www.nuget.org/packages/angularjs.TypeScript.DefinitelyTyped" title="AngularJS TypeScript definitely typed files">AngularJS TypeScript definitely typed files</a> which provides typed AngularJS components that can be used in your TypeScript code.</p>
<p><strong>Bootstrapper</strong></p>
<p>In AngularJS you create an <code>app.js</code> where you load your modules, controllers, factories and directives. You do almost the same thing in TypeScript, the only difference here is that your controllers, factories and directives are represented as TypeScript classes in an <code>app.ts</code> file. So your <code>app.ts</code> can look like this:</p>
<p>[code language="javascript"]<br />
var appModule = angular.module(&quot;myApp&quot;, []);</p>
<p>appModule.controller(&quot;MyController&quot;, [&quot;$scope&quot;, &quot;MyService&quot;, ($scope, MyService)<br />
    =&gt; new Application.Controllers.MyController($scope, MyService)]);</p>
<p>appModule.factory(&quot;MyService&quot;, [&quot;$http&quot;, &quot;$location&quot;, ($http, $location)<br />
    =&gt; new Application.Services.MyService($http, $scope)]);</p>
<p>appModule.directive(&quot;myDirective&quot;, ()<br />
    =&gt; new Application.Directives.MyDirective());<br />
[/code]</p>
<p>Note the usage of lambda, we do this to reserve lexical scope. Always make sure to use this instead of <code>function()</code> in TypeScript. Now that you've set up your bootstrapper, in the next sections we'll look at how the individual AngularJS components are written in TypeScript.</p>
<p><strong>Controller Classes</strong></p>
<p>Controllers are written as classes, so your <code>MyController.ts</code> class can look like this:</p>
<p>[code language="javascript"]<br />
module Application.Controllers {</p>
<p>    import Services = Application.Services;</p>
<p>    export class MyController {</p>
<p>        scope: any;<br />
        myService: Services.IMyService;<br />
	    data: any;</p>
<p>        constructor($scope: ng.IScope, myService: Services.IMyService) {<br />
            this.scope = $scope;<br />
            this.myService = myService;<br />
	        this.data = [];<br />
        }</p>
<p>        private GetAll() {<br />
            this.myService.GetAll((data) =&gt; {<br />
                this.data = data;<br />
            });<br />
        }<br />
	}<br />
}<br />
[/code]</p>
<p><strong>Factory Classes</strong></p>
<p>Similarly, factories or services are written as classes. So your <code>MyService.ts</code> class can look like this:</p>
<p>[code language="javascript"]<br />
module Application.Services {</p>
<p>    export interface IMyService {<br />
        GetAll(successCallback: Function);<br />
    }</p>
<p>    export class MyService {</p>
<p>        http: ng.IHttpService;<br />
        location: ng.ILocationService;</p>
<p>        constructor($http: ng.IHttpService, $location: ng.ILocationService) {<br />
            this.http = $http;<br />
            this.location = $location;<br />
        }</p>
<p>        GetAll(successCallback: Function) {<br />
            this.http.get(this.location.absUrl()).success((data, status) =&gt; {<br />
                successCallback(data);<br />
            }).error(error =&gt; {<br />
                successCallback(error);<br />
            });<br />
        }<br />
	}<br />
}<br />
[/code]</p>
<p>Note the interface <code>IMyService</code> here. Always use interfaces to abstract your classes in TypeScript, just as you would usually do in a typed language.</p>
<p><strong>Directive Classes</strong></p>
<p>Directives are also written as classes. So your <code>MyDirective.ts</code> class can look like this:</p>
<p>[code language="javascript"]<br />
module Application.Directives {</p>
<p>    export class MyDirective {</p>
<p>        constructor() {<br />
			return this.CreateDirective();<br />
        }</p>
<p>        private CreateDirective():any {<br />
            return {<br />
                restrict: 'E',<br />
                template: '&lt;div&gt;MyDirective&lt;/div&gt;<br />
            };<br />
        }<br />
    }<br />
}<br />
[/code]</p>
<p><strong>Databinding with Alias</strong></p>
<p>Finally, to be able to use your TypeScript classes from your HTML, you need to databind using alias. So your HTML can look like this:</p>
<p>[code language="html"]<br />
&lt;html data-ng-app=&quot;myApp&quot;&gt;<br />
&lt;head&gt;<br />
&lt;meta charset=&quot;utf-8&quot;&gt;<br />
&lt;meta http-equiv=&quot;X-UA-Compatible&quot; content=&quot;IE=edge&quot;&gt;<br />
&lt;/head&gt;<br />
&lt;body&gt;<br />
	&lt;div data-ng-controller=&quot;MyController as mc&quot;&gt;<br />
		&lt;div data-ng-repeat=&quot;element in mc.data&quot;&gt;<br />
			&lt;label&gt;{{element}}&lt;/label&gt;<br />
		&lt;/div&gt;<br />
		&lt;my-directive&gt;&lt;/my-directive&gt;<br />
	&lt;/div&gt;<br />
&lt;/body&gt;<br />
&lt;/html&gt;<br />
[/code]</p>
<p><strong>Databinding <em>without</em> Alias</strong></p>
<p>Unfortunately, databinding with alias does not work in Internet Explorer 8. Neither do directive elements(!). To make it work in IE 8, you need to change your HTML so it looks like this:</p>
<p>[code language="html"]<br />
&lt;html xmlns:ng=&quot;http://angularjs.org&quot; id=&quot;ng-app&quot; data-ng-app=&quot;myApp&quot;&gt;<br />
&lt;head&gt;<br />
&lt;meta charset=&quot;utf-8&quot;&gt;<br />
&lt;meta http-equiv=&quot;X-UA-Compatible&quot; content=&quot;IE=edge&quot;&gt;<br />
&lt;!--[if lte IE 8]&gt;<br />
            &lt;script&gt;<br />
                document.createElement('ng-include');<br />
                document.createElement('ng-pluralize');<br />
                document.createElement('ng-view');<br />
                document.createElement('my-directive');<br />
                document.createElement('ng:include');<br />
                document.createElement('ng:pluralize');<br />
                document.createElement('ng:view');<br />
            &lt;/script&gt;<br />
            &lt;script src=&quot;libs/es5-shim/es5-shim.js&quot;&gt;&lt;/script&gt;<br />
            &lt;script src=&quot;libs/JSON/json3.js&quot;&gt;&lt;/script&gt;<br />
        &lt;![endif]--&gt;<br />
&lt;/head&gt;<br />
&lt;body&gt;<br />
	&lt;div data-ng-controller=&quot;MyController&quot;&gt;<br />
		&lt;div data-ng-repeat=&quot;element in data&quot;&gt;<br />
			&lt;label&gt;{{element}}&lt;/label&gt;<br />
		&lt;/div&gt;<br />
		&lt;my-directive&gt;&lt;/my-directive&gt;<br />
	&lt;/div&gt;<br />
&lt;/body&gt;<br />
&lt;/html&gt;<br />
[/code] </p>
<p>Now if you want to call your TypeScript class methods from your HTML in IE 8 without an alias, then you need to hook your methods (and everything else they use) onto the Angular <code>scope</code>. This can be done like the following, in <code>MyController.ts</code>:</p>
<p>[code language="javascript"]<br />
module Application.Controllers {</p>
<p>    import Services = Application.Services;</p>
<p>    export class MyController {</p>
<p>        scope: any;</p>
<p>        constructor($scope: ng.IScope, myService: Services.IMyService) {<br />
            this.scope = $scope;<br />
            this.scope.myService = myService;<br />
			this.scope.data = [];<br />
			this.scope.GetAll = this.GetAll;<br />
        }</p>
<p>        GetAll() {<br />
            this.scope.myService.GetAll((data) =&gt; {<br />
                this.scope.data = data;<br />
            });<br />
        }<br />
	}<br />
}<br />
[/code]</p>
<p>Notice how everything is hooked onto <code>scope</code>. That way, databinding is correctly achieved in IE 8, and you are able to call your <code>GetAll()</code> method from the HTML by simply typing <code>GetAll()</code> without alias.</p>
<p>Hope you enjoyed this tutorial! I'm planning on holding a workshop about this and doing TDD at this year's <a href="http://www.ndcoslo.com/" title="NDC 2014">Norwegian Developers Conference 2014</a>, so make sure to book your tickets! :)</p>
