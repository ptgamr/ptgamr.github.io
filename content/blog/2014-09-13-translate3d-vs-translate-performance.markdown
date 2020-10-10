---
layout: post
title: 'CSS3: Translate3d vs Translate performance'
date: 2014-09-13 20:30:32.000000000 +12:00
---
Recently i'm develop a small game using [Ionic Framework](http://ionicframework.com) and there are a bit of movement inside the game that require me to use CSS3 transition.

At first, i think using translate(x,y) will be faster because '3d' is somehow overheaded for me. What i need is just '2d'.

After experience the lagging in mobile device, i did some research on the internet and very supprised that translate3d(x,y,z) is faster in most of browser. After i apply this into my game, smooth transition is detected :-)

####REASON:

> The use of translate3d pushes CSS animations into hardware acceleration. Even if you're looking to do a basic 2d translation, use translate3d for more power! So 'T3d' is just better because it tells the CSS animations to push the animations in 3d power! That's why it is faster. (The answer of Cameron Little is the proof)
http://stackoverflow.com/questions/22111256/translate3d-vs-translate-performance

And here is the test page: http://jsperf.com/translate3d-vs-xy/28



 
