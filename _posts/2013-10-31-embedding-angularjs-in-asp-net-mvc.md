---
layout: post
title: Embedding AngularJS in ASP.NET MVC
date: 2013-10-31 15:44:37.000000000 +01:00
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
<p>Ever since the beginning of the web, the basic principal has been; servers doing heavy work while having thin, "dumb" clients displaying information.  We are, however, living in changing times. The need for "smart" and efficient clients is increasing, and the term "good user experience" now plays a big role in web sites. With the immense increase of new JavaScript frameworks, we are definitely getting thicker and richer clients. <a title="AngularJS" href="http://angularjs.org/">AngularJS</a> is a JavaScript framework by Google that's easy to embed into your web site, and easy to test since it was <em>designed to be testable</em>. That means that things like dependency injection is easy to apply when writing unit tests. Embedding AngularJS into our site not only brings us richer clients, but also drastically decreases server bottlenecks as data are retrieved from server once and locally handled on the client side.</p>
<p>In the following guide I explain how you can embed AngularJS into your existing ASP.NET MVC site, which in this case is the <a title="Contoso University" href="http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8?cdn_id=2013-10-07-002">Contoso University sample project by Microsoft</a>.</p>
<p><strong>Install AngularJS through NuGet</strong></p>
<p>After you have downloaded the <a title="Contoso University" href="http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8?cdn_id=2013-10-07-002">Contoso University sample</a> and got it up and running, the first natural step is to install the AngularJS framework by executing the following command in Packet Manager in Visual Studio:</p>
<pre>Install-Package angularjs -Version 1.0.8</pre>
<p>That will download the necessary JavaScript files into the "Scripts" folder in your project. Make sure to place your own AngularJS files (that you'll create in the next steps) inside of this folder.</p>
<p><strong>Module</strong></p>
<p>In order to embed and wire up AngularJS you'd want to define a module (which may contain sub-modules), a module declaratively specifies how your app should be bootstrapped. So you can create a "homeModule.js" file that can look like this:</p>
```javascript 
angular
 .module('myApp', [
 'myApp.controller.student',
 'myApp.service.data'
 ])
 .config(['$routeProvider', '$locationProvider', function ($routeProvider, $locationProvider) {
      //Specify routings here
}]);
```
<p>"myApp" is your main module, with a sub-module "myApp.controller.student" for your student controller and "myApp.service.data" for your data service. You can also configure routings if you'd like to route specific URLs and such.</p>
<p><strong>Controller</strong></p>
<p>Next up is to define a controller where you want to put all your logic, so you can create a "studentController.js" file that can look like this:</p>
```javascript 
angular
    .module('myApp.controller.student', [])
    .controller('StudentController', [
        '$scope',
        '$location',
        'dataService',
        function($scope, $location, dataService) {
            $scope.students = [];
            $scope.delete = function(student) {
                var index = $scope.students.indexOf(student);
                $scope.students.splice(index, 1);
            };
           $scope.create = function() {
                var student = { FirstMidName: "Test", LastName: "Test", EnrollmentDate: new Date() };
                $scope.students.push(student);
            };
            $scope.post = function() {
                dataService
                    .post($scope.students)
                    .success(function(data, status, headers, config) {
                       location.reload();
                    });
            };
           dataService
                .getStudents()
                .success(function (data, status, headers, config) {
                    $scope.students = data;
                });
        }]);
```
<p>Note how AngularJS loads the student module that you defined earlier, and defines a controller called "StudentController" with the necessary parameters. Always use <code>$scope</code> when you write general controller behavior. Note how the <code>post()</code> and <code>getStudents()</code> functions use the data service to post and get our student data.</p>
<p><strong>Data service</strong></p>
<p>In most cases you want to have a service that retrieves data from the server for you, so you can create a "dataService.js" file that can look like this:</p>
```javascript
angular
    .module('myApp.service.data', [])
    .factory('dataService', [
        '$http',
        function ($http) {
           return {
                getStudents: function () {
                    return $http({
                       method: 'GET',
                        url: '/api/student'
                   });
               },
                post: function (students) {
                    return $http({
                        method: 'POST',
                        url: '/api/student',
                        data: students
                    });
                },
            };
        }]);
```
<p>AngularJS loads a factory here called "dataService" which we use in "studentController.js". As you can see, the <code>getStudents()</code> and <code>post()</code> functions are defined in here. Most importantly, the latter and the former point to a GET and POST methods with the URL "/api/student". In ASP.NET MVC, this is understood as pointing to the "StudentController" class (in an "api" folder), which we will create in the next step.</p>
<p><strong>Api controller</strong></p>
<p>The last step before wiring everything up, is to define the api controller for Entity Framework so our data service can communicate through this controller to retrieve our data. Create a "StudentController.cs" class and insert your own logic:</p>
```csharp
public class StudentController : ApiController
{
    public List Get()
    {
        //Get students
    }
    [HttpPost]
    public void Post(List students)
    {
        //Post students
    }
}
```
<p><strong>Wire everything up</strong></p>
<p>Now that we have written all of our components, we are ready to wire everything up. Go to your HTML definition file, "_Layout.cshtml", and add <code>ng-app="myApp"</code> to the HTML tag:</p>
```html
<!DOCTYPE html>
<html ng-app="myApp" lang="en">
```
<p>Go to "BundleConfig.cs" and bundle in the AngularJS files:</p>
```csharp
public class BundleConfig
    {
       public static void RegisterBundles(BundleCollection bundles)
        {
            //Other bundles here
            bundles.Add(new ScriptBundle("~/bundles/angular").Include(
                        "~/Scripts/angular.js"));
            bundles.Add(new ScriptBundle("~/bundles/myApp").Include(
                        "~/Scripts/app/homeModule.js",
                        "~/Scripts/app/studentController.js",
                        "~/Scripts/app/dataService.js"));
        }
}
```
<p>You're now all set to start using AngularJS! So you can go to a view in your project, write some HTML and bind it to your "StudentController". Below is an example code:</p>
```html
<div ng-controller="StudentController">
<input type="submit" value="Create" ng-click="create()" />
<div class="row-fluid">
<table class="table table-condensed table-hover">
<thead>
<tr>
<th></th>
<th class="span2">First Name</th>
<th class="span1">Last Name</th>
<th class="span1">Enrollment Date</th>
</tr>
</thead>
<tbody>
<tr ng-repeat="student in students">
<td><input type="submit" value="Delete" ng-click="delete(student)" /></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<input type="submit" value="Confirm" ng-click="post()" />
</div>
```
Make special note of <code>ng-controller</code> which binds your controller. Then, for instance, by using <code>ng-repeat</code> you can loop through your local students collection and easily access each student and their properties. That's pretty much it, go ahead and test out the code!
## Dependency Injection (unit testing)
As I mentioned earlier, AngularJS was designed to be testable from bottom up. So I thought it would be useful to include an example of testing our student controller, showing how dependency injection is done. We are here using <code>angular-mocks</code> (which comes with AngularJS when you install it) to create our mocks, and [Jasmine](http://pivotal.github.io/jasmine/) for test syntax (check out [my blog post on unit testing JavaScript](http://sirars.com/2013/10/28/test-driving-your-javascript-visual-studio-jasmine-karma-test-runner/)):

```javascript
describe('Test the student controller', function() {
        //Mock variables
        var rootScopeMock, httpBackendMock;
        
        //Controller
        var myController;
        
        //Mock the module (before each test)
        beforeEach(angular.mock.module('myApp'));
        
        //Inject the mocks and controller (into each test)
        beforeEach(angular.mock.inject(function($rootScope, $controller, $httpBackend) {
        
        //Mock default requests
        httpBackendMock = $httpBackend;
        httpBackendMock.whenGET('/api/student').respond({ code: 200, value: "OK" });
        
        rootScopeMock = $rootScope.$new();
        
        myController = $controller('StudentController', {
        $scope: rootScopeMock
        });
    }));
    
    it('Should create new student when user creates a new student', function() {
        rootScopeMock.create();
        expect(rootScopeMock.students.length).toBe(1);
    });
    
    it('Should delete student when user deletes a student', function() {
        var student = createStudentStub();
        rootScopeMock.students.push(student);
        rootScopeMock.delete(student);
        expect(rootScopeMock.students.length).toBe(0);
    });
    
    it('Should post students when user confirms', function() {
        var student = createStudentStub();
        rootScopeMock.students.push(student);
        httpBackendMock.expectPOST('/api/student', rootScopeMock.students).respond();
        rootScopeMock.post();
    });
    
    var createStudentStub = function() {
        var student = {
        FirstMidName: "Test",
        LastName: "Test",
        EnrollmentDate: new Date()
        };
        return student;
    };
});
```
The code is pretty self explanatory, enjoy!