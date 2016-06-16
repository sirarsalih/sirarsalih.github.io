---
layout: post
title: Updating $scope Data in Asynchronous Functions
date: 2014-10-19 21:34:32.000000000 +02:00
type: post
published: true
status: publish
categories:
- Development
tags: [js]
meta:
  _wpas_skip_facebook: '1'
  _wpas_skip_google_plus: '1'
  _wpas_skip_twitter: '1'
  _wpas_skip_linkedin: '1'
  _wpas_skip_tumblr: '1'
  _wpas_skip_path: '1'
  _publicize_pending: '1'
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
author:
  login: sirars
  email: sirars@gmail.com
  display_name: Sirar Salih
  first_name: ''
  last_name: ''
---
One of the many lovely things about Angular is the fluid two-way data binding that we get with the framework. Although not entirely perfect when it comes down to performance, it saves us quite a lot of time and effort when writing web applications.
When it comes to asynchronousy, however, it should be noted that doing changes to your ```$scope``` data does not propagate to the view implicitly (!). In other words, if you do a change to a ```$scope``` variable from asynchronous context, the change is not reflected in the view. Let's look at an example. Consider the following controller that uses JavaScript's asynchronous ```setInterval``` function to update a counter every second:
    
```javascript
function Ctrl ($scope) {
    $scope.counter = 0;
    
    setInterval(function() {
        $scope.counter++;
    }, 1000);
}
```

This code looks pretty good and dandy, right? Unfortunately, no. Although the counter does get updated every second, it does not propagate the change to the view. There are a couple of ways to solve this issue.
<strong>Invoking $apply Manually</strong>
The simplest and most straightforward way is to invoke ```$apply``` manually in the asynchronous context. Consider the following change to our initial example (lines 5 - 7):
    
```javascript
function Ctrl ($scope) {
    $scope.counter = 0;
    
    setInterval(function() {
        $scope.$apply(function () {
            $scope.counter++;
        });
    }, 1000);
}
```

This change will force the counter updates through to the view.
<strong>Using Angular's Asynchronous Services</strong>
This approach is case specific. For instance, you can use the services ```$interval``` or ```$timeout```, which actually behind the scenes invoke ```$apply``` - relieving you from doing so manually. Considering our initial example, we can therefore inject and use $interval in our controller instead:
    
```javascript
function Ctrl ($scope, $interval) {
    $scope.counter = 0;

    $interval(function () {
        $scope.counter++;
    }, 1000);
}
```

As the previous approach, this will propagate the counter updates to the view. I recommend using Angular's services wherever this is possible and seems appropriate to the case at hand. However, always keep in mind that such services invoke <code>$apply</code> for you behind the scenes.
<p>Hope you enjoyed this Sunday's little reading! :)</p>
