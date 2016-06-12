---
layout: post
title: Unit Testing JavaScript (Jasmine, Karma Test Runner, Visual Studio)
date: 2013-10-28 19:42:31.000000000 +01:00
type: post
published: true
status: publish
categories:
- Unit Testing
tags:
- Jasmine Testing Framework
- JavaScript
- Karma Test Runner
- Programming
- Tests
- Unit testing
- Visual Studio
meta:
  _edit_last: '54045106'
  _publicize_pending: '1'
  _oembed_28e71bce5c4aff3187a67cfebb14aa6e: "{{unknown}}"
author:
  login: sirars
  email: sirars@gmail.com
  display_name: Sirar Salih
  first_name: ''
  last_name: ''
---
<p>Testing your front-end is just as essential as testing your back-end, that's what I always say. In an ideal system you want to have test coverage on both of those ends, regardless of how many testers you got or how many manual test hours you put in. The matter of fact is, we are human, and humans tend to make mistakes and miss out on things. Automating your tests is key, even on the front-end.</p>
<p>So how do we unit test JavaScript code? In the following guideline, you'll see how you can configure unit testing for JavaScript using the Jasmine testing framework, Karma test runner and Visual Studio on a Windows environment.</p>
<p><strong>Step 1: Setup your Karma test runner</strong></p>
<p>There are several test runners out there for JavaScript, we'll use <a title="Karma" href="http://karma-runner.github.io/0.10/index.html">Karma</a>. Now, setting up this test runner on a Windows environment can be a daunting task. But, if you simply go through the steps I have outlined below then you should be okay:</p>
<p>1. Install <a title="Python 2.7" href="http://www.python.org/download/releases/2.7/">Python 2.7</a></p>
<p>2. Install <a title="Node Packet Manager" href="http://nodejs.org/download/">Node Packet Manager</a></p>
<p>3. Target browser in environment (user) variables</p>
<p>You need to set a target browser (we'll use Chrome) for Karma to use when running your JavaScript tests, you do this by accessing Environment Variables (Control Panel -&gt; Advanced System Settings -&gt; Advanced), and under User Variables create the following:</p>
<pre>CHROME_BIN=%HOME%\AppData\Local\Google\Chrome\Application\chrome.exe</pre>
<p>4. Start up admin cmd and run the following:</p>
<pre>npm install karma</pre>
<p>This will install Karma through Node Packet Manager, make sure that NPM is added to your global path (this should be added automatically once you've installed NPM). The Karma install may generate some errors, check the console and see if you got any "socket.io" errors. If that's the case, then run the following commands:</p>
<pre>npm install node-gyp
npm install socket.io
npm install ws
npm install karma</pre>
<p>5. Navigate to the following path and copy the folder:</p>
<pre>C:\Windows\SysWOW64\node_modules\</pre>
<p>To:</p>
<pre>%HOME%\AppData\Roaming\npm\node_modules\</pre>
<p><span style="line-height:1.5;">Your Karma test runner is now all set.</span></p>
<p>PS: If you cannot find the "SysWOW64" folder, try to search for "node_modules" else where on your machine. It should contain the stuff that you installed through NPM.</p>
<p><strong>Step 2: Setup your Jasmine testing framework</strong></p>
<p>Now that we have a test runner, we need a testing framework so we can write JavaScript test syntax. We'll use <a title="Jasmine" href="http://pivotal.github.io/jasmine/">Jasmine</a> for this. Simply start up Visual Studio, create a new Web project and in the Package Manager Console type:</p>
<pre>install-package jasminetest</pre>
<p>That's it, you now have Jasmine installed in your project (you'll also get some sample JS files). Pretty straight forward compared to the previous step, huh?</p>
<p><strong> Step 3: Configure Karma to run your tests</strong></p>
<p>Karma needs to be configured in order to locate and run your tests. Create a folder in your Visual Studio project called "spec" and add it to "Scripts", navigate to this folder. Make sure that Karma is added to your global path (this should be added automatically once you've installed Karma), start cmd and type:</p>
<pre>karma init</pre>
<p>Now you'll get some questions, answer with the following:</p>
<ul>
<li>jasmine for the test framework</li>
<li>no to Require.js</li>
<li>Chrome as your browser</li>
<li>In order, provide the files that you want to test and their dependencies:
<ul>
<li>Enter relative path to your JavaScript framework and other dependencies</li>
<li>Enter relative path to your source file(s)</li>
<li>Enter relative path to your test file(s), postfix the name of your test files with "Spec"</li>
<li>Here is an example of paths:</li>
</ul>
</li>
</ul>
<pre>../jquery-1.8.2.js
../jquery-ui-1.8.24.js
../app/logic.js
../spec/logicSpec.js</pre>
<p>Karma is now configured, a "karma.conf" file is now generated - with the information that you answered - in your "spec" folder.</p>
<p><strong>Step 4: Write and run your first Canary Test</strong></p>
<p>Now that Karma is configured, we are ready to write and run our first canary test. In Visual Studio, in your "spec" folder, add a new JavaScript file called "logicSpec.js". In this file, write the following canary test:</p>
```javascript 
describe(Testing the logic, function () {
    it(Should be true, function () {
        expect(true).toBe(true);
    });
});
```
<p>Start cmd, and start Karma by typing:</p>
<pre>karma start</pre>
<p>Karma will now parse the "karma.conf" file, launch Chrome and run your test. A green execution means the test passed, red means it failed. Karma runs your tests each time you save your test file. You should get the following:</p>
<p><a href="http://sirars.files.wordpress.com/2013/10/karma_success.png"><img class="alignnone size-medium wp-image-12" alt="karma_success" src="http://sirars.files.wordpress.com/2013/10/karma_success.png?w=300" width="300" height="152" /></a></p>
<p>You have successfully run your first Canary Test. How about testing some logic? Create a folder in Visual Studio called "app" and add it to "Scripts", then add a "logic.js" in this folder. Create the following add function in your "logic.js":</p>
```javascript 
function add(a, b) {
    return a + b;
}
```
<p>Now test this function by adding the following test in your "logicSpec.js":</p>
```javascript 
it(Should return 5 when 2 and 3 are passed, function() {
    var result = add(2, 3);
    expect(result).toEqual(5);
});
```
<p>Save your test file, you should now see that Karma successfully executed both of your tests. That's it! Jasmine is rich with functionality, so you can do lots of stuff. You can check out their site for more <a title="Jasmine" href="http://pivotal.github.io/jasmine/">here</a>.</p>