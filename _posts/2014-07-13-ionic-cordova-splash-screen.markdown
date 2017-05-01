---
layout: post
title: Adding Splash Screen in Ionic Framework
date: 2014-07-13 08:24:21.000000000 +12:00
---
When writing a mobile application using Cordova, you probaly want to display your beautiful design splash screen while your app is opening instead of the default Cordova splashscreen. You can do it by copying your splashscreens into the XCode's project **/Resources/splash** folder.

Yes, after that, run the application on your device, you will have the splash screen displayed. Oh Yeah!!! I made it!!!

However, you will notice a little "disrupted white screen" after your splashscreen disappear and application opening it's browser and your "webapp" initialized.

To have the control on this, you will have to use Splashscreen Plugin for Cordova. Hide it when your application "really ready" :-)  

I'm sharing with you a solution that is working for me.

1 - Install cordova.splashscreen plugin in your app. Example:

	$ cd myapp
    $ cordova plugin add org.apache.cordova.splashscreen

output:

    Fetching plugin "org.apache.cordova.splashscreen" via plugin registry
    Installing "org.apache.cordova.splashscreen" for ios


2 - change the defaults.xml in myapp/platforms/ios/cordova/

    <preference name="AutoHideSplashScreen" value="false" />
    <preference name="ShowSplashScreenSpinner" value="false" />


3 - add in your controllers.js:

    angular.module('starter.controllers', [])

    .run(function($ionicPlatform) {
      $ionicPlatform.ready(function() {
        setTimeout(function() {
            navigator.splashscreen.hide();
        }, 100);
     });
    })

After make these changes you must rebuild the app: `$ ionic build ios`

And try it `$ ionic emulate ios`
