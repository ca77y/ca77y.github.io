---
layout: post
title: AngularJS loves Coffee
tags: [angularjs, coffeescript]
---

... or not. At least it seems CoffeeScript is not very popular with the AngularJS crowd. Which is a shame really because I feel alone in my CoffeeScript love.
To change that here is how I write AngularJS in CoffeeScript.

### Service

{% highlight coffeescript %}
class AppService
  ping: (input) ->
    "#{input} BOOM!!!"
{% endhighlight %}

Clean, beautiful and fast to write. Couldn't be simpler.

### Controller

First a small hack to make things easier.

{% highlight coffeescript %}
class AngularCtrl
  constructor: (scope) ->
    for name of @
      if name in scope
        throw new Error("'#{name}' already exists in scope")
      if name isnt 'constructor' and name[0] not in ['_', '$']
        scope[name] = @[name]
{% endhighlight %}

This will register all properties not starting with '_' or '$' on the scope. This way we don't have to copy them manually.

{% highlight coffeescript %}
class AppCtrl extends AngularCtrl
  constructor: (@_scope, @_appService) ->
    super(_scope)
  ping: =>
    @_appService.ping 'KA'
{% endhighlight %}

I don't particularly like this part but it saves a lot of typing and makes the actual controller more readable as long as you remember which properties are going to be copied. The really bad part is => everywhere. Since the methods will be copied on the scope it is necessary, I don't know a better way :(

Ideally I would like to have the scope separate from the controller. Maybe an inner class or something. Since my controllers tend to be < 100 lines of code I don't mind for now.

### Directive
{% highlight coffeescript %}
class AppDir
  restrict: 'AE'
  template: '<span>ping->{{ping()}}</span>'
  controller: 'appCtrl'
{% endhighlight %}

Easy stuff.

### Wiring
{% highlight coffeescript %}
angular.module('app', [])
  .service('appService', [AppService])
  .controller('appCtrl', ['$scope', 'appService', AppCtrl])
  .directive('appdir', [-> new AppDir()])
{% endhighlight %}

Putting our parts together. One really cool advantage for Intellij users, like me, is you can search for classes with instead of files. This helps to keep file names related to the package/component and class names to the actual code.

So there... time to rewrite the entire universe in CoffeeScript... NOW!!!