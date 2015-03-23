---
layout: post
title: "Asynchronous Load of AngularJS Controllers"
modified:
categories: angular js
excerpt: The problem is simple, you have a lot of controllers and you don't need all the code downloaded in the first page load. This post is about how I resolved the problem.
comments: true
tags: []
image:
  feature:
date: 2015-03-23T09:16:33+01:00
---

![angular](/images/angularjs.png)

The problem is simple, you have a lot of controllers and you don't need all the code downloaded in the first page load.

This post is about how I resolved the problem. It is a trick, so maybe there is a better approach to do this. Let me know if you have a better option!

#### How it works

In angular we define the routes like this:

{% highlight js %}
$routeProvider.
      when('/some/route', someRouteConfig).
      when('/other/route', otherRouteConfig)
{% endhighlight %}

The `someRouteConfig` and `otherRouteConfig` must have the `controller` property and, optionally, the `templateUrl`.

{% highlight js %}
var someRouteConfig = {
  templateUrl: '/url/to/template.html',
  controller: controllerFunction
}
{% endhighlight %}

The templateUrl is the path to our view file and the controller is the function to execute when the route matches.

I usually see in all the how-to and tutorials over the internet all the angular loaded like this, all the files are placed in the head tag.

{% highlight html %}
<!doctype html>
<html lang="en" ng-app="phonecatApp">
<head>
  ....
  <script src="js/app.js"></script>
  <script src="js/controllers/users.js"></script>
  <script src="js/controllers/settings.js"></script>
  <script src="js/controllers/reports.js"></script>
  <script src="js/controllers/feed.js"></script>
</head>
<body>
  <div ng-view></div>
</body>
</html>
{% endhighlight %}

But this makes the browser to download every .js file. Even those you're never going to use in the current user session.


#### The async way

Just looking at the `route.js` code, I found that we can use a promise instead of a function when defining our routes. [Take a look here](https://github.com/angular/angular.js/blob/master/src/ngRoute/route.js#L573)

So that means we can make our route definition like this:

{% highlight js %}
$routeProvider.
      when('/some/route', someRouteConfigPromise).
      when('/other/route', otherRouteConfigPromise)
{% endhighlight %}


But we still don't have access to `$q` or any angular service because we're in a `config` method. So, what is the easiest way to create a promise with our controller config?

This is the most controversial part of this "hack", we can emulate a promise behaviour easily.

{% highlight js %}
  var someRouteConfigPromise = {
    then: function (done) {
        var self = this;

        require(['js/controllers/users.js'], function (ctrl) {
            self.controller  = ctrl;
            self.resolve     = ctrl.resolve;
            self.templateUrl = ctrl.templateUrl;

            done();
        });
    }
  }
{% endhighlight %}

We are doing a few things here:

1. Defining the property `then` of our object, which will be used by `$q` when trying to resolve the promise.
2. Loading our controller with `require`
3. Setting the expected keys of the controller config
4. Resolving the promise

As you might seen, the file `js/controllers/users.js` is returning the `ctrl` object, which have a `resolve` and a `templateUrl` properties defined.

#### Refactor the code

Probably this is not the best solution for this, but with a small refactor we can create an asynchronous controller factory.


{% highlight js %}
function controllerFactory(ctrl) {
    return {
        then: function (done) {
            var self = this;

            require(['./controller/' + ctrl], function (ctrl) {
                self.controller  = ctrl;
                self.resolve     = ctrl.resolve;
                self.templateUrl = ctrl.templateUrl;

                done();
            });
        }
    };
}
{% endhighlight %}

And use it in our route definition like this:

{% highlight js %}
$routeProvider.
  when('/some/route', controllerFactory('some/route')).
  when('/other/route', controllerFactory('other/route'))
{% endhighlight %}

----

Notes:

- Maybe there is a way to create "real" promises in a `config` function but I don't find how.
- We should create the `controllerFactory function as an angular service/factory
- This is my first post completly in english, please, tell me if you found any grammar issues.. or just one of them ;)
