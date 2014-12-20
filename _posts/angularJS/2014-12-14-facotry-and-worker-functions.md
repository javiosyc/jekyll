---
layout: post
title: "Facotry and worker functions"
description: ""
category: [AngularJS]
tags: [AngularJS]
---
{% include JB/setup %}

###FACTORY AND WORKER FUNCTIONS

These are often **factory functions**, so called because they are **responsible for creating the object that AngularJS will employ to perform the work itself**.

Often, factory functions will return **a worker function**, which is to say that the object that AngularJS will use to perform some work is a function, too.

The second argument to the directive method is a factory function

...

myApp.directive("highlight", **function** () {

>**return** function (scope, element, attrs) {

>>if (scope.day == attrs["highlight"]) {

>>>element.css("color", "red");

>>}

>}

});

The return statement in the factory function returns another function, which AngularJS will **invoke each time it needs to apply the directive**, and this is the **worker function**:

...

myApp.directive("highlight", function () {

>return **function (scope, element, attrs) {

>>if (scope.day == attrs["highlight"]) {**

>>>**element.css("color", "red");**

>>**}**

>**}**

});

AngularJS will **call the factory function when it wants to set up the building block** and then **calls the worker function when it needs to apply the building block**, and these **three events won’t occur in an immediate sequence** (in other words, other Module methods will be called before your factory function is invoked, and other factory functions will be invoked before your worker function is called).
