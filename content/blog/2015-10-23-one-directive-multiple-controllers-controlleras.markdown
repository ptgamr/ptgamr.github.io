---
layout: post
title: One directive, multiple controllers & controllerAs
date: 2015-10-23 21:20:20.000000000 +13:00
---
You can pass multiple controller in to the `require` property while declaring a directive

{% highlight javascript %}
app.directive('myDirective', function () {
  return{
    restrict: "A",
    require:['^parentDirective', '^ngModel'],
    link: function ($scope, $element, $attrs, controllersArr) {

      // parentDirective controller
      controllersArr[0].someMethodCall();

      // ngModel controller
      controllersArr[1].$setViewValue();
    }
  }
});
{% endhighlight %}

Want to use `controllerAs` syntax for your child directive, but still want to access parent directive's controller?

{% highlight javascript %}
app.directive('myDirective', function () {
  return{
    restrict: "A",
    require:['^parentDirective', 'myDirective'],
    controller: 'myDirectiveController',
    controllerAs: 'childVm',
    link: function ($scope, $element, $attrs, ctrls) {
		var parentCtrl = ctrls[0];

        var childVm = ctrls[1];

        // and do stuffs
    }
  }
});
{% endhighlight %}
