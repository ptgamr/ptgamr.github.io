---
layout: post
title: Cordova plugin for Google Play Game Service
date: 2014-09-24 08:56:40.000000000 +12:00
---
Recently, i developed a small hybrid game using `Ionic Framework` and it requires me to have `Leaderboard` feature to display players highscore.

Everything is fine with iOS, for a simple google search, you will find this plugin: [cordova-plugin-game-center](https://github.com/leecrossley/cordova-plugin-game-center) and intergrate it is pretty easy.

When it come to Android, i did the search again, but i do not found any suitable plugin for my purpose. Only one repository on Github and it says 'work in-progress', but i didn't see any progress at all.

Decided to write my own plugin. The first time to write a Cordova plugin, i was struggling for two days to make it build successfully in Eclipse.

You can fork it here: https://github.com/ptgamr/cordova-google-play-game

A Game that i developed using this plugin can be found here, download it and play if you like: 

[![WOW PUZZLES iOS](https://developer.apple.com/app-store/marketing/guidelines/images/badge-download-on-the-app-store.svg)](https://itunes.apple.com/vn/app/wow-puzzles/id916475017?mt=8)

[![WOW PUZZLES Android](https://developer.android.com/images/brand/en_app_rgb_wo_45.png)](https://play.google.com/store/apps/details?id=com.a42.xephinhtuoitho)

Basically, the Game Service concept is similar between iOS and Android. You'll have to get familiar with the concept of Leaderboards & Achivements.

###Installation
To install the plugin, please do the following command in your beloved Terminal:

	cordova plugin add https://github.com/ptgamr/cordova-google-play-game.git --variable APP_ID=711269060910
      
Please note that you have to specify the APP_ID, which is your clientId while creating the game in Google Play Developer Console. If you want to know how to retrieve your appId, please to this [nicely Google's guide](https://developers.google.com/games/services/android/quickstart)

The plugin will install `Google Play Services` as its dependency. It will modify your MANIFEST file & res/strings.xml automatically (to add your APP_ID), follow this guide: https://developer.android.com/google/play-services/setup.html

###Note for using together with Cordova Plugin Admob
For those who use this Admob plugin: https://github.com/floatinghotpot/cordova-plugin-admob together with my plguin, you'll probaly have the error during installation, because it use the old Admob SDK:

	dependency id="com.google.admobsdk-ios"

I give it a FORK [here](https://github.com/ptgamr/cordova-plugin-admob), to remove the Admod SDK as the dependency. Please consider use this version together with `Codova Google Play Game`

If you have any question using the plugin, please wrote me a comment, or creat the issue on Github.
