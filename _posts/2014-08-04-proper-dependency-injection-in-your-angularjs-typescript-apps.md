---
layout: post
title: Proper Dependency Injection in Your AngularJS TypeScript Apps
date: 2014-08-04 20:49:25.000000000 +02:00
type: post
published: true
status: publish
categories:
- Development
tags:
- AngularJS
- Dependency Injection
- TypeScript
meta:
  _edit_last: '54045106'
  geo_public: '0'
  _publicize_pending: '1'
  _oembed_3926ccf458d9fcd4991774209fe13798: "{{unknown}}"
  _oembed_61072db9adce36d16acbaf04fa8671d1: "{{unknown}}"
  _oembed_a0068a3d347f4647be5586554755f47d: "{{unknown}}"
author:
  login: sirars
  email: sirars@gmail.com
  display_name: Sirar Salih
  first_name: ''
  last_name: ''
---
I've witnessed quite a lot of legacy AngularJS TypeScript code (<a href="http://sirarsalih.com/2014/01/28/when-two-forces-meet-angularjs-typescript/">yes, including my very own</a>) where dependency injection is done in an impractical way. The most common way of doing dependency injection, is by manually injecting the dependencies while loading your AngularJS components and their accommodating TypeScript classes. What this basically means is that, while injecting a dependency, you have to do this three times (!). Not only that, all three times have to be similar in order for the injection (string matching) to work. Consider the following example:

```javascript
angular.module("myApp", []).controller("MyController", ["$scope", "MyService", ($scope, MyService)
    => new Application.Controllers.MyController($scope, MyService)]);
```

```javascript
module Application.Controllers {
 
    import Services = Application.Services;
 
    export class MyController {
 
        scope: any;
        myService: Services.IMyService;
         
        constructor($scope: ng.IScope, myService: Services.IMyService) {
            this.scope = $scope;
            this.myService = myService;
        }
    }
}
```

<p><code>MyController</code> is a class that takes in two dependencies - <code>$scope</code> and <code>MyService</code>. However, in the first code block, the injection itself is written three times (lines 1 and 2). Not only does this result in a maintenance hell, it greatly affects the readability of the code. </p>
So, how do we solve this issue? Simple, we use AngularJS' <code>$inject</code> property to do the injection in our TypeScript class. In <code>MyController</code>, we statically use <code>$inject</code> and define the dependencies like so (line 10):

```javascript
module Application.Controllers {
 
    import Services = Application.Services;
 
    export class MyController {
 
        scope: any;
        myService: Services.IMyService;
        
        static $inject = ["$scope", "MyService"];

        constructor($scope: ng.IScope, myService: Services.IMyService) {
            this.scope = $scope;
            this.myService = myService;
        }
    }
}
```

And then simply change the wiring like so:

```javascript
angular.module("myApp", []).controller("MyController", Application.Controllers.MyController);
```
<p>Notice how elegant the wiring looks? No explicit injection that we need to worry about, all we need is to inspect the class itself to find the dependencies.</p>
