---
layout: post
title: 'TodayILearnt: angular.element(elem) was actually an alias to the jQuery constructor
  function'
date: 2015-09-01 00:34:03.000000000 +12:00
---
If you include jQuery in your AngularJS application.

$element in your directive linking function is by default wrapped by jqLite. So you can only `.find` by tag name.

If you want to have full jQuery feature, you need to wrap your element with `angular.element` :)

This is quite tricky to know :)
