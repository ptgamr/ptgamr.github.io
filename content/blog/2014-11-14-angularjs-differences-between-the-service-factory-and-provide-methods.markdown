---
layout: post
title: AngularJS - Differences between the service(), factory(), and provide() methods
date: 2014-11-14 07:23:06.000000000 +13:00
---
In Angular, services are singleton objects that are created when necessary and are never cleaned up until the end of the application life-cycle (when the browser is closed). Controllers are destroyed and cleaned up when they are no longer needed.

This is why we can't dependably use controllers to share data across our application, especially when using routing

>Services are designed to be the glue between controllers, the minions of data, the slaves of functionality, the worker-bees of our application.

###3 ways to creat a service in AngularJS

###factory() 
The `factory()` method allows us to define a service by returning an object that contains service functions and service data. The service definition function is where we place our injectable services, such as $http and $q. 
```language-javascript 
  angular.module('myApp.services')
  	.factory('User', function($http) { // injectables go here
        var backendUrl = "http://localhost:3000";
        var service = {
          // our factory definition
          user: {},
          setName: function(newName) { 
          service.user['name'] = newName; 
          },
       	  setEmail: function(newEmail) {
            service.user['email'] = newEmail;
          },
          save: function() {
            return $http.post(backendUrl + '/users', {
            	user: service.user
          	});
          }
        };
        return service;
});
```
####Using the factory() in our app 
It's easy to use the factory in our application as we can simply inject it where we need it at run-time 
```language-javascript 
    angular.module('myApp')
    .controller('MainCtrl', function($scope, User) {
      $scope.saveUser = User.save;
    });
```
####When to use the factory() method 
The `factory()` method is a great choice to use to build a factory when we just need a collection of methods and data and don't need to do anything especially complex with our service.

We cannot use the `factory()` method when we need to configure our service from the `.config()` function.

###service()
The `service()` method, on the other hand allows us to create a service by defining a constructor function. We can use a prototypical object to define our service, instead of a raw javascript object. 
```language-javascript 
	angular.module('myApp.services')
    .service('User', function($http) { // injectables go here
      var self = this; // Save reference
      this.user = {};
      this.backendUrl = "http://localhost:3000";
      this.setName = function(newName) {
        self.user['name'] = newName;
      }
      this.setEmail = function(newEmail) {
        self.user['email'] = newEmail;
      }
      this.save = function() {
        return $http.post(self.backendUrl + '/users', {
          user: self.user
        })
      }
    });
```
####Using the factory() in our app 
It's easy to use the factory in our application as we can simply inject it where we need it at run-time 
```language-javascript 		
    angular.module('myApp')
    .controller('MainCtrl', function($scope, User) {
      $scope.saveUser = User.save;
    }); 
```
####When to use the service() method 
The `service()` method is great for creating services where we need a bit more control over the functionality required by our service. It's also mostly guided by preference to use this instead of referring to the service

We cannot use the `service()` method when we need to configure our service from the `.config()` function.

###provide()
The lowest level way to create a service is by using the provide() method. This is the only way to create a service that we can configure using the .config() function.

Unlike the previous to methods, we'll set the injectables in a defined this.$get() function definition. 
```language-javascript 		
    angular.module('myApp.services')
    .provider('User', function() {
      this.backendUrl = "http://localhost:3000";
      this.setBackendUrl = function(newUrl) {
        if (url) this.backendUrl = newUrl;
      }
      this.$get = function($http) { // injectables go here
        var self = this;
        var service = {
          user: {},
          setName: function(newName) {
            service.user['name'] = newName;
          },
          setEmail: function(newEmail) {
            service.user['email'] = newEmail;
          },
          save: function() {
            return $http.post(self.backendUrl + '/users', {
              user: service.user
            })
          }
        };
        return service;
      }
    });
```
####Using the provider() in our app 
In order to configure our service, we can inject the provider into our `.config()` function.
```language-javascript 
    angular.module('myApp')
    .config(function(UserProvider) {
    UserProvider.setBackendUrl("http://myApiBackend.com/api");
    })
```
We can use the service in our app just like any other service now 
```language-javascript 
    angular.module('myApp')
    .controller('MainCtrl', function($scope, User) {
      $scope.saveUser = User.save;
    });
```

####When to use the `provider()` method 

The `provider()` method is required when we want to configure our service before the app runs. For instance, if we need to configure our services to use a different back-end based upon different deployment environments (development, staging, and production).

It's the preferred method for writing services that we intend on distributing open-source as well as it allows us to configure services without needing to hard-code configuration data.

Source: [ng-newsletter](http://www.ng-newsletter.com/advent2013/#!/day/1)
