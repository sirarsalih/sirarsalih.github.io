---
layout: post
title: Continuous JavaScript Testing with Karma Test Runner and TeamCity
date: 2013-10-30 15:18:59.000000000 +01:00
type: post
published: true
status: publish
categories:
- Unit Testing
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
<p>In <a title="my recent blog post" href="http://sirars.com/2013/10/28/test-driving-your-javascript-visual-studio-jasmine-karma-test-runner/">my recent blog post</a> I explained how you can set up Karma test runner and install Jasmine in Visual Studio so you can unit test your JavaScript code. Once you got your test runner up and running locally, you would like to have continuous testing of your JavaScript code. What that means is that you want to have the test runner running on your build server as well, such that each time you check-in/commit your code, the build server will automatically start Karma and check your JavaScript.</p>
<p><a title="TeamCity" href="http://www.jetbrains.com/teamcity/">TeamCity</a> is a popular and widely used continuous integration solution by JetBrains, and it's quite easy to configure Karma with it. After you have installed Karma (by <a title="following my guide" href="http://sirars.com/2013/10/28/test-driving-your-javascript-visual-studio-jasmine-karma-test-runner/">following my guide</a>) on your build server, all you need to do is add a build step in TeamCity to start up the Karma test runner. You do this by adding the following command:</p>
<pre>karma start --reporters teamcity --single-run</pre>
<p>In the "Command executable" field in the TeamCity build step:</p>
<p><a href="http://sirars.files.wordpress.com/2013/10/picture1.png"><img class="alignnone size-medium wp-image-21" alt="Picture1" src="http://sirars.files.wordpress.com/2013/10/picture1.png?w=300" width="300" height="166" /></a></p>
<p>That's pretty much it, let me know how that works out for you!</p>
