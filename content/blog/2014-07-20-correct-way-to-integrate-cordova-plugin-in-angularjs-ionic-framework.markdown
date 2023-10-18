---
layout: post
title: Correct way to integrate Cordova Plugin in AngularJS, Ionic Framework
date: 2014-07-20 05:54:10.000000000 +12:00
---
If you are using Ionic Framework to develop your mobile app, even if it's the simplest app, you need at least some cordova plugin to get your app work nicely.

The first and most important is the [Device Plugin](https://github.com/apache/cordova-plugin-device), which will inform your Javascript application when the Device is ready. This is luckily, already handle by Ionic Framework itself, in your app.js file, just inject **$ionicPlatform** to your controller. For example:
```language-javascript 
	app.run(function($ionicPlatform) {
        $ionicPlatform.ready(function() {

            console.log('### APP READY!!!');

            setTimeout(function() {
                navigator.splashscreen.hide();
            }, 100);

            if(window.StatusBar) {
                StatusBar.styleDefault();
            }
            
            //other logic here
            ...
        });
    })
```
A very convinient way to integrate Cordova to your app is to make use of [ngCordova Project](http://ngcordova.com/). It's a set of already made AngularJS Services which are wrappers for some popular Cordova Plugins like: **$cordovaCamera**, **$cordovaContacts**, **$cordovaSocialSharing** ... Just include the library, and inject the above service whenever you need it.

However, when your app require the integration of some plugins that has not been under the protect of ngCordova, you certainly have to write your own wrapper!

And here is the way i'm using to write it (I learned it from the way ngCordova did):
						   
```language-javascript                            angular.module('CordovaWrapper.screenshot', [])

	.factory('$cordovaScreenshot', ['$q', function ($q) {

        return {

            capture: function() {

                   var q = $q.defer();     	
                   	navigator.screenshot.save(function(error,res){
                    if(error){
                        console.error(error);
                        q.reject(error);
                    }else{
                        console.log('screenshot capture ok: ',res.filePath);
                        q.resolve(res.filePath);
                    }
                },'jpg',50);

                return q.promise;

            }
        };

    }]);
```
