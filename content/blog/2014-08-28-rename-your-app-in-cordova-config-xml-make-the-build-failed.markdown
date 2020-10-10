---
layout: post
title: Rename your App in Cordova config.xml make the Build Failed
date: 2014-08-28 23:04:52.000000000 +12:00
---
I've been struggling with XCode to make my app running after i renamed the project.

####To rename your iOS build:

	1. run command: cordova platform rm ios

	2. change your app name in *config.xml*

	3. run command cordova platform add ios

####The platform will be create normally, plugins are installed. But XCode build states that there are some files missing

You need this step to solved the problem:

	copy the ios src of the plugin to 
    projektname/platforms/ios/Appname/Plugins/plugin.name.space


It works for me!!
