---
layout: post
title: Yet Another AngularJS treeview - Use cases
tags: [angular, treeview, yet another]
---

Some examples of ya.treeview in action.

### Multiselect

Controller:

{% highlight javascript %}
$scope.options = {
    context: {
        selectedNodes: []
    },
    onSelect: function ($event, node, context) {
        if ($event.ctrlKey) {
            var idx = context.selectedNodes.indexOf(node);
            if (context.selectedNodes.indexOf(node) === -1) {
                context.selectedNodes.push(node);
            } else {
                context.selectedNodes.splice(idx, 1);
            }
        } else {
            context.selectedNodes = [node];
        }
    }
};
{% endhighlight %}

HTML:

{% highlight html %}
<div ya-treeview ya-id="myTree" ya-model="model" ya-options="options">
    <span ng-class="{selected: context.selectedNodes.indexOf(node) > -1}">{{ node.$model.label }}</span>
</div>
{% endhighlight %}

CSS:

{% highlight css %}
.selected {
  background-color: #aaddff;
  font-weight: bold;
}
{% endhighlight %}

### Async children loading

There are two ways to do it given _options.lazy_ is _true_. Either _children_ is a Function in which case it will be
executed when the new part of a virtual model is constructed (on node expand) or onExpand is implemented to add
children to the node.

If it's ok for you to have a function in your model the first option works out of the box. The second one is below.

Controller:

{% highlight javascript %}
$scope.options = {
    onExpand: function($event, node, context) {
        Restangular.one('parents', node.$model.id).getList('children')
            .then(function (result) {
                node.$model.children = result;
                node.$children = context.nodifyArray(result);
            });
    }
};
{% endhighlight %}

HTML:

{% highlight html %}
<div ya-treeview ya-id="myTree" ya-model="model" ya-options="options">
    <span>{{ node.$model.label }}</span>
</div>
{% endhighlight %}

### Preselecting a node

The tree should be loaded with a node already selected for the user.

If you have a reference to the virtual node you want to select this is easy.

{% highlight javascript %}
context.selectedNode = node;
var parent = node.$parent;
while(parent) {
    parent.collapsed = false;
    parent = parent.$parent;
}
{% endhighlight %}

Except you probably won't have it. After loading a model you would like to get a reference to the node you want to
preselect and tell the treeview 'look, when you create a virtual node for this node I want it to be selected and path
to it expanded'. Fair enough.

Every time a virtual node is created YaTreeviewService.nodify is called. This is the same nodify which is in the
context. You can decorate a this service and check which node is processed.

This is shitty I know.

What happens if you have 2 treeviews with the same model and want only one to preselect a node?
I can add a treeview id to the call for that but that's going in the wrong direction imho.

What happens if you're loading nodes asynchronously?
No idea :)

I am thinking how to improve this, for it is what it is :(