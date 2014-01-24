---
layout: post
title: On RESTful API tutorials
tags: [restful, api, rage]
---

I've been asked to create a RESTful API during an interview process... a [todo app](http://my-todo-sample.herokuapp.com/) with a RESTful API. Simple stuff, better than taking about what I want to do in 5 years so why not.
Since I should have done it from scratch [yeoman](http://yeoman.io/) was a no-brainer.

1. npm install -g angular-fullstack
2. yo angular-fullstack

At this point 80-90% of the application is ready :) or is it?
Yeoman generates an Express based application. This is not a RESTful API. Those are URLs which will throw JSON at you.
I've spent the next hour browsing through tutorials which have RESTful only in their name. I don't understand why so many ppl try to show me how to setup a basic node.js environment and title it RESTful API.

When I search RESTful I want to know how hard it is to set self link to resources. How to handle nested resources like `/employees/123/cars/321`. Those are basic stuff which I would like to know, not where to download node.js from.

I've wasted an hour or so and I thought wasting 10min on writing this would make me feel better. It didn't.