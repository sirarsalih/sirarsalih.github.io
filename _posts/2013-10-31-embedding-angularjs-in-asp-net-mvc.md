---
layout: post
title: Embedding AngularJS in ASP.NET MVC
date: 2013-10-31 15:44:37.000000000 +01:00
type: post
published: true
status: publish
categories:
- Development
tags:
- AngularJS
- ASP.NET MVC
- JavaScript
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
<p>Ever since the beginning of the web, the basic principal has been; servers doing heavy work while having thin, "dumb" clients displaying information.  We are, however, living in changing times. The need for "smart" and efficient clients is increasing, and the term "good user experience" now plays a big role in web sites. With the immense increase of new JavaScript frameworks, we are definitely getting thicker and richer clients. <a title="AngularJS" href="http://angularjs.org/">AngularJS</a> is a JavaScript framework by Google that's easy to embed into your web site, and easy to test since it was <em>designed to be testable</em>. That means that things like dependency injection is easy to apply when writing unit tests. Embedding AngularJS into our site not only brings us richer clients, but also drastically decreases server bottlenecks as data are retrieved from server once and locally handled on the client side.</p>
<p>In the following guide I explain how you can embed AngularJS into your existing ASP.NET MVC site, which in this case is the <a title="Contoso University" href="http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8?cdn_id=2013-10-07-002">Contoso University sample project by Microsoft</a>.</p>
<p><strong>Install AngularJS through NuGet</strong></p>
<p>After you have downloaded the <a title="Contoso University" href="http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8?cdn_id=2013-10-07-002">Contoso University sample</a> and got it up and running, the first natural step is to install the AngularJS framework by executing the following command in Packet Manager in Visual Studio:</p>
<pre>Install-Package angularjs -Version 1.0.8</pre>
<p>That will download the necessary JavaScript files into the "Scripts" folder in your project. Make sure to place your own AngularJS files (that you'll create in the next steps) inside of this folder.</p>
<p><strong>Module</strong></p>
<p>In order to embed and wire up AngularJS you'd want to define a module (which may contain sub-modules), a module declaratively specifies how your app should be bootstrapped. So you can create a "homeModule.js" file that can look like this:</p>
<p>[code language="javascript"]angular<br />
 .module('myApp', [<br />
 'myApp.controller.student',<br />
 'myApp.service.data'<br />
 ])<br />
 .config(['$routeProvider', '$locationProvider', function ($routeProvider, $locationProvider) {<br />
      //Specify routings here<br />
}]);<br />
[/code]</p>
<p>"myApp" is your main module, with a sub-module "myApp.controller.student" for your student controller and "myApp.service.data" for your data service. You can also configure routings if you'd like to route specific URLs and such.</p>
<p><strong>Controller</strong></p>
<p>Next up is to define a controller where you want to put all your logic, so you can create a "studentController.js" file that can look like this:</p>
<p>[code language="javascript"]<br />
angular<br />
    .module('myApp.controller.student', [])<br />
    .controller('StudentController', [<br />
        '$scope',<br />
        '$location',<br />
        'dataService',<br />
        function($scope, $location, dataService) {<br />
            $scope.students = [];</p>
<p>            $scope.delete = function(student) {<br />
                var index = $scope.students.indexOf(student);<br />
                $scope.students.splice(index, 1);<br />
            };</p>
<p>           $scope.create = function() {<br />
                var student = { FirstMidName: &quot;New&quot;, LastName: &quot;New&quot;, EnrollmentDate: new Date() };<br />
                $scope.students.push(student);<br />
            };</p>
<p>            $scope.post = function() {<br />
                dataService<br />
                    .post($scope.students)<br />
                    .success(function(data, status, headers, config) {<br />
                       location.reload();<br />
                    });<br />
            };</p>
<p>            dataService<br />
                .getStudents()<br />
                .success(function (data, status, headers, config) {<br />
                    $scope.students = data;<br />
                });<br />
        }]);<br />
[/code]</p>
<p>Note how AngularJS loads the student module that you defined earlier, and defines a controller called "StudentController" with the necessary parameters. Always use <code>$scope</code> when you write general controller behavior. Note how the <code>post()</code> and <code>getStudents()</code> functions use the data service to post and get our student data.</p>
<p><strong>Data service</strong></p>
<p>In most cases you want to have a service that retrieves data from the server for you, so you can create a "dataService.js" file that can look like this:</p>
<p>[code language="javascript"]<br />
angular<br />
    .module('myApp.service.data', [])<br />
    .factory('dataService', [<br />
        '$http',<br />
        function ($http) {</p>
<p>            return {<br />
                getStudents: function () {<br />
                    return $http({<br />
                       method: 'GET',<br />
                        url: '/api/student'<br />
                   });<br />
               },</p>
<p>                post: function (students) {<br />
                    return $http({<br />
                        method: 'POST',<br />
                        url: '/api/student',<br />
                        data: students<br />
                    });<br />
                },<br />
            };<br />
        }]);<br />
[/code]</p>
<p>AngularJS loads a factory here called "dataService" which we use in "studentController.js". As you can see, the <code>getStudents()</code> and <code>post()</code> functions are defined in here. Most importantly, the latter and the former point to a GET and POST methods with the URL "/api/student". In ASP.NET MVC, this is understood as pointing to the "StudentController" class (in an "api" folder), which we will create in the next step.</p>
<p><strong>Api controller</strong></p>
<p>The last step before wiring everything up, is to define the api controller for Entity Framework so our data service can communicate through this controller to retrieve our data. Create a "StudentController.cs" class and insert your own logic:</p>
<p>[code language="csharp"]<br />
 public class StudentController : ApiController<br />
    {<br />
        public List Get()<br />
        {<br />
            //Get students<br />
        }</p>
<p>        [HttpPost]<br />
        public void Post(List students)<br />
        {<br />
            //Post students<br />
        }<br />
    }<br />
[/code]</p>
<p><strong>Wire everything up</strong></p>
<p>Now that we have written all of our components, we are ready to wire everything up. Go to your HTML definition file, "_Layout.cshtml", and add <code>ng-app="myApp"</code> to the HTML tag:</p>
<p>[code language="html"]<br />
&lt;!DOCTYPE html&gt;<br />
&lt;html ng-app=&quot;myApp&quot; lang=&quot;en&quot;&gt;<br />
&lt;!----&gt;<br />
&lt;/html&gt;<br />
[/code]</p>
<p>Go to "BundleConfig.cs" and bundle in the AngularJS files:</p>
<p>[code language="csharp"]<br />
public class BundleConfig<br />
    {<br />
       public static void RegisterBundles(BundleCollection bundles)<br />
        {<br />
            //Other bundles here</p>
<p>            bundles.Add(new ScriptBundle(&quot;~/bundles/angular&quot;).Include(<br />
                        &quot;~/Scripts/angular.js&quot;));</p>
<p>            bundles.Add(new ScriptBundle(&quot;~/bundles/myApp&quot;).Include(<br />
                        &quot;~/Scripts/app/homeModule.js&quot;,<br />
                        &quot;~/Scripts/app/studentController.js&quot;,<br />
                        &quot;~/Scripts/app/dataService.js&quot;));<br />
        }<br />
}<br />
[/code]</p>
<p>You're now all set to start using AngularJS! So you can go to a view in your project, write some HTML and bind it to your "StudentController". Below is an example code:</p>
<p>[code language="html"]<br />
&lt;div ng-controller=&quot;StudentController&quot;&gt;<br />
    &lt;input type=&quot;submit&quot; value=&quot;Create&quot; ng-click=&quot;create()&quot; /&gt;<br />
    &lt;div class=&quot;row-fluid&quot;&gt;<br />
        &lt;table class=&quot;table table-condensed table-hover&quot;&gt;<br />
            &lt;thead&gt;<br />
                &lt;tr&gt;<br />
                    &lt;th&gt;&lt;/th&gt;<br />
                    &lt;th class=&quot;span2&quot;&gt;First Name&lt;/th&gt;<br />
                    &lt;th class=&quot;span1&quot;&gt;Last Name&lt;/th&gt;<br />
                    &lt;th class=&quot;span1&quot;&gt;Enrollment Date&lt;/th&gt;<br />
                &lt;/tr&gt;<br />
            &lt;/thead&gt;<br />
            &lt;tbody&gt;<br />
                &lt;tr ng-repeat=&quot;student in students&quot;&gt;<br />
                    &lt;td&gt;&lt;input type=&quot;submit&quot; value=&quot;Delete&quot; ng-click=&quot;delete(student)&quot; /&gt;&lt;/td&gt;<br />
                    &lt;td&gt;{{student.FirstMidName}}&lt;/td&gt;<br />
                    &lt;td&gt;{{student.LastName}}&lt;/td&gt;<br />
                    &lt;td&gt;{{student.EnrollmentDate}}&lt;/td&gt;<br />
                &lt;/tr&gt;<br />
            &lt;/tbody&gt;<br />
        &lt;/table&gt;<br />
    &lt;/div&gt;<br />
    &lt;input type=&quot;submit&quot; value=&quot;Confirm&quot; ng-click=&quot;post()&quot; /&gt;<br />
&lt;/div&gt;<br />
[/code]</p>
<p>Make special note of <code>ng-controller</code> which binds your controller. Then, for instance, by using <code>ng-repeat</code> you can loop through your local students collection and easily access each student and their properties. That's pretty much it, go ahead and test out the code!</p>
<p><strong>Dependency Injection (unit testing)</strong></p>
<p>As I mentioned earlier, AngularJS was designed to be testable from bottom up. So I thought it would be useful to include an example of testing our student controller, showing how dependency injection is done. We are here using <code>angular-mocks</code> (which comes with AngularJS when you install it) to create our mocks, and <a title="Jasmine" href="http://pivotal.github.io/jasmine/">Jasmine</a> for test syntax (check out <a title="Unit Testing JavaScript" href="http://sirars.com/2013/10/28/test-driving-your-javascript-visual-studio-jasmine-karma-test-runner/">my blog post on unit testing JavaScript</a>):<br />
[code language="javascript"]<br />
describe('Test the student controller', function() {</p>
<p>    //Mock variables<br />
    var rootScopeMock, httpBackendMock;</p>
<p>    //Controller<br />
    var myController;</p>
<p>    //Mock the module (before each test)<br />
    beforeEach(angular.mock.module('myApp'));</p>
<p>    //Inject the mocks and controller (into each test)<br />
    beforeEach(angular.mock.inject(function($rootScope, $controller, $httpBackend) {</p>
<p>        //Mock default requests<br />
        httpBackendMock = $httpBackend;<br />
        httpBackendMock.whenGET('/api/student').respond({ code: 200, value: &quot;OK&quot; });</p>
<p>        rootScopeMock = $rootScope.$new();</p>
<p>        myController = $controller('StudentController', {<br />
            $scope: rootScopeMock<br />
        });<br />
    }));</p>
<p>    it('Should create new student when user creates a new student', function() {<br />
        rootScopeMock.create();<br />
        expect(rootScopeMock.students.length).toBe(1);<br />
    });</p>
<p>    it('Should delete student when user deletes a student', function() {<br />
        var student = createStudentStub();<br />
        rootScopeMock.students.push(student);<br />
        rootScopeMock.delete(student);<br />
        expect(rootScopeMock.students.length).toBe(0);<br />
    });</p>
<p>    it('Should post students when user confirms', function() {<br />
        var student = createStudentStub();<br />
        rootScopeMock.students.push(student);<br />
        httpBackendMock.expectPOST('/api/student', rootScopeMock.students).respond();<br />
        rootScopeMock.post();<br />
    });</p>
<p>    var createStudentStub = function() {<br />
        var student = {<br />
            FirstMidName: &quot;Test&quot;,<br />
            LastName: &quot;Test&quot;,<br />
            EnrollmentDate: new Date()<br />
        };<br />
        return student;<br />
    };<br />
});<br />
[/code]<br />
The code is pretty self explanatory, enjoy!</p>
