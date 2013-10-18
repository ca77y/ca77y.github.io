---
layout: post
title: Angular bootstrap on document ready
---

Going through the angular docs for bootstrap of an app you can find out this is the way to do it:
{% highlight javascript %}
angular.element(document).ready(function() {
 angular.module('myApp', []);
 angular.bootstrap(document, ['myApp']);
});
{% endhighlight %}
This is a correct way to do it... unless you are using Coffeescript in which case it might so happen the ready event is fired before you Coffeescript gets compiled (given you are using in-browser compilation for dev).
Funny thing is, you will not get any errors. The script will never fire and so your app won't even bootstrap to give you any errors.
This ugly piece of code takes care of it:
{% highlight coffeescript %}
if document.readyState != 'complete'
  angular.element(document).ready ->
    angular.bootstrap(document, ['wawelowe'])
else
  angular.bootstrap(document, ['wawelowe'])
{% endhighlight %}