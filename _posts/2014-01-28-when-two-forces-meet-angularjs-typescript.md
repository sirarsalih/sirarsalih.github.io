---
layout: post
title: When Two Forces Meet (AngularJS, TypeScript)
date: 2014-01-28 12:38:11.000000000 +01:00
type: post
published: true
status: publish
categories:
tags: [js]
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
<p><a href="https://sirars.files.wordpress.com/2014/01/capture.png"><img src="https://sirars.files.wordpress.com/2014/01/capture.png?w=300" alt="Capture" width="300" height="224" class="alignnone size-medium wp-image-279" /></a></p>
<p><strong>Setup</strong></p>
<p>As a .NET developer, I use <a href="http://www.visualstudio.com/" title="Visual Studio 2013">Visual Studio 2013</a> and the <a href="http://www.microsoft.com/en-us/download/details.aspx?id=34790" title="TypeScript plugin for Visual Studio">TypeScript plugin for Visual Studio</a>. This is what I recommend that you use unless you are for some reason against the Windows platform. Then using the NuGet packet manager, you can install the necessary <a href="http://www.nuget.org/packages/angularjs.core" title="AngularJS core files">AngularJS core files</a>. In addition to this, make sure to install the <a href="http://www.nuget.org/packages/angularjs.TypeScript.DefinitelyTyped" title="AngularJS TypeScript definitely typed files">AngularJS TypeScript definitely typed files</a> which provides typed AngularJS components that can be used in your TypeScript code.</p>
<strong>Bootstrapper</strong></p>
In AngularJS you create an <code>app.js</code> where you load your modules, controllers, factories and directives. You do almost the same thing in TypeScript, the only difference here is that your controllers, factories and directives are represented as TypeScript classes in an <code>app.ts</code> file. So your <code>app.ts</code> can look like this:

```javascript
var appModule = angular.module("myApp", []);

appModule.controller("MyController", ["$scope", "MyService", ($scope, MyService)
    => new Application.Controllers.MyController($scope, MyService)]);

appModule.factory("MyService", ["$http", "$location", ($http, $location)
    => new Application.Services.MyService($http, $scope)]);

appModule.directive("myDirective", ()
    => new Application.Directives.MyDirective());
```
    
<p>Note the usage of lambda, we do this to reserve lexical scope. Always make sure to use this instead of <code>function()</code> in TypeScript. Now that you've set up your bootstrapper, in the next sections we'll look at how the individual AngularJS components are written in TypeScript.</p>
<p><strong>Controller Classes</strong></p>
<p>Controllers are written as classes, so your <code>MyController.ts</code> class can look like this:</p>

```javascript
module Application.Controllers {

    import Services = Application.Services;

    export class MyController {

        scope: any;
        myService: Services.IMyService;
	    data: any;
		
        constructor($scope: ng.IScope, myService: Services.IMyService) {
            this.scope = $scope;
            this.myService = myService;
	        this.data = [];
        }

        private GetAll() {
            this.myService.GetAll((data) => {
                this.data = data;
            });
        }
	}
}
```

<p><strong>Factory Classes</strong></p>
Similarly, factories or services are written as classes. So your <code>MyService.ts</code> class can look like this:

```javascript
module Application.Services {

    export interface IMyService {
        GetAll(successCallback: Function);
    }

    export class MyService {

        http: ng.IHttpService;
        location: ng.ILocationService;

        constructor($http: ng.IHttpService, $location: ng.ILocationService) {
            this.http = $http;
            this.location = $location;
        }

        GetAll(successCallback: Function) {
            this.http.get(this.location.absUrl()).success((data, status) => {
                successCallback(data);
            }).error(error => {
                successCallback(error);
            });
        }
	}
}
```

<p>Note the interface <code>IMyService</code> here. Always use interfaces to abstract your classes in TypeScript, just as you would usually do in a typed language.</p>
<p><strong>Directive Classes</strong></p>
<p>Directives are also written as classes. So your <code>MyDirective.ts</code> class can look like this:</p>

```javascript
module Application.Directives {

    export class MyDirective {

        constructor() {
			return this.CreateDirective();
        }

        private CreateDirective():any {
            return {
                restrict: 'E',
                template: '<div>MyDirective</div>
            };
        }
    }
}
```

<p><strong>Databinding with Alias</strong></p>
<p>Finally, to be able to use your TypeScript classes from your HTML, you need to databind using alias. So your HTML can look like this:</p>

```html
<html xmlns:ng="http://angularjs.org" id="ng-app" data-ng-app="myApp">
<head>
<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<!--[if lte IE 8]>
            <script>
                document.createElement('ng-include');
                document.createElement('ng-pluralize');
                document.createElement('ng-view');
                document.createElement('my-directive');
                document.createElement('ng:include');
                document.createElement('ng:pluralize');
                document.createElement('ng:view');
            </script>
            <script src="libs/es5-shim/es5-shim.js"></script>
            <script src="libs/JSON/json3.js"></script>
        <![endif]-->
</head>
<body>
	<div data-ng-controller="MyController">
		<div data-ng-repeat="element in data">
			<label>{{element}}</label>
		</div>
		<my-directive></my-directive>
	</div>
</body>
</html>
```

<p>Now if you want to call your TypeScript class methods from your HTML in IE 8 without an alias, then you need to hook your methods (and everything else they use) onto the Angular <code>scope</code>. This can be done like the following, in <code>MyController.ts</code>:</p>

```javascript
module Application.Controllers {

    import Services = Application.Services;

    export class MyController {

        scope: any;
		
        constructor($scope: ng.IScope, myService: Services.IMyService) {
            this.scope = $scope;
            this.scope.myService = myService;
			this.scope.data = [];
			this.scope.GetAll = this.GetAll;
        }

        GetAll() {
            this.scope.myService.GetAll((data) => {
                this.scope.data = data;
            });
        }
	}
}
```

<p>Notice how everything is hooked onto <code>scope</code>. That way, databinding is correctly achieved in IE 8, and you are able to call your <code>GetAll()</code> method from the HTML by simply typing <code>GetAll()</code> without alias.</p>
<p>Hope you enjoyed this tutorial! I'm planning on holding a workshop about this and doing TDD at this year's <a href="http://www.ndcoslo.com/" title="NDC 2014">Norwegian Developers Conference 2014</a>, so make sure to book your tickets! :)</p>
