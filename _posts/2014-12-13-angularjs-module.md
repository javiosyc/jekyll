---
layout: post
title: "AngularJS module"
description: ""
category: [AngularJS]
tags: [AngularJS ,modules]
---
{% include JB/setup %}


##Working with Modules

###Modules are the top-level components for AngularJS applications. 


Modules have three main roles in an AngularJS app

*	To associate an AngularJS application with a region of an HTML document

*	To act as a gateway to key AngularJS framework features

*	To help organize the code and components in an AngularJS application


###Setting the Boundaries of an AngularJS Application

The first step when creating an AngularJS app is **to define a module and associate it with a region of the HTML document**.

creates the module for the example app.

1. Creating a Module
`var myApp = angular.module("exampleApp", []);`

>module method supports the three arguments

>* name

>>>The name of the new module

>>>The convention is to give the module a name with the suffix App

>* requires

>>>The set of modules that this module depends on

>* config

>>>The configuration for the module, equivalent to calling the Module.config method—see the “Working with the Module Life Cycle” section

2. Applying the ng-app Attribute in the HTML File

`<html ng-app="exampleApp">`

>>The ng-app attribute is used *during the bootstrap phase* of the AngularJS life cycle.

###Using Modules to Define AngularJS Components
The angular.module method returns a *Module object* that provides access to the most important features that AngularJS provides via the properties and methods.

The Members of the Module Object

>* animation(name, factory)

>>>Supports the animation feature

>* config(callback)

>>> Registers a function that can be used to configure a module when it is loaded.

>* constant(key, value)

>>> Defines a service that returns a constant value.

>* **controller(name, constructor)**

>>> Creates a controller. 

>* **directive(name, factory)**

>>> Creates a directive, which extends the standard HTML vocabulary.

>* **factory(name, provider)**

>>> Creates a service.

>* **filter(name, factory)**

>>> Creates a filter that formats data for display to the user.

>* **provider(name, type)**

>>> Creates a service.

>* name

>>> Returns the name of the module.

>* run(callback)

>>> Registers a function that is invoked after AngularJS has loaded and configured all of the modules.

>* service(name, constructor)

>>> Creates a service.

>* value(name, value)

>>> Defines a service that returns a constant value.

The methods defined by the Module object fall into three broad categories

* **define components for an AngularJS application**

* **those that make it easier to create those building blocks**

* **those that help manage the AngularJS life cycle**

###Defining Controllers

To act as **a conduit between the model and the views**.

Most AngularJS projects will have multiple controllers, each of which delivers the data and logic required for one aspect of the application.


Controllers are defined using the Module.controller method, which takes two arguments:

* the name of the controller

* **a factory function**, which is used to set up the controller and get it ready for use

`var myApp = angular.module("exampleApp", []);`

>`myApp.controller("dayCtrl", function (`**$scope**`) {`

>`// controller statements will go here`

`});`

>>>The convention for controller names is to use the suffix **Ctrl**.

>>>The function passed to the Module.controller method is used to **declare the controller’s dependencies**, which are the AngularJS components that the **controller requires**.

>>>AngularJS provides some **built-in services and features** that are specified using argument names that **start with the $ symbol**.

***Applying Controllers to Views

The Controllers must be applied to HTML elements so that AngularJS knows which part of an HTML document forms the view for a given controller.

`<body>`

>`<div class="panel" ng-controller="dayCtrl">`

>>`<div class="page-header">`

>>>`<h3>AngularJS App</h3>`

>>`</div>`

>`<h4>Today is {{day || "(unknown)"}}</h4>`

>`</div>`

`</body>` 

The $scope component that I specified as an argument when I **created the controller is used to provide the view with data**, and only the data configured **via $scope can be used in expressions and data bindings**.

###Creating Multiple Views

**Each controller can support multiple views**, which allows the same data to be presented in different ways or for closely related data to be created and managed efficiently.

###Creating Multiple Controllers

`var myApp = angular.module("exampleApp", []);`

`myApp.controller("dayCtrl", function ($scope) {`

>`...`

`}`

`myApp.controller("tomorrowCtrl", function ($scope) {`

>`...`

`}`

`...`

`<div class="panel">`

>`<div class="page-header">`

>>`<h3>AngularJS App</h3>`

>`</div>`

>`<h4 ng-controller="dayCtrl">Today is {{day || "(unknown)"}}</h4>`

>`<h4 ng-controller="tomorrowCtrl">Tomorrow is {{day || "(unknown)"}}</h4>`

`</div>`

Notice how I am able to use the day property in **both views without the values interfering with each other**.

**Each controller has its own part of the overall application scope**, and the day property of the dayCtrl controller is isolated from the one defined by the tomorrowCtrl controller.

###Defining Directives

Directives are the most powerful AngularJS feature because they **extend and enhance HTML to create rich web applications**.

The short version is that custom directives are created via the Module.directive method.

`myApp.directive("highlight", function () {`

>`return function (scope, element, attrs) {`

>>`if (scope.day == attrs["highlight"]) {`

>>>`element.css("color", "red");`

>>`}`

>`}`

`});`


`<h4 ng-controller="dayCtrl" `**highlight="Monday"**`>`

>`Today is {{day || "(unknown)"}}`

`</h4>`


###Applying Directives to HTML Elements

...

`<h4 ng-controller="dayCtrl" `**`highlight="Monday">`**

...


My custom directive is called **highlight**, and it is applied as an attribute.

I have set the value of the highlight attribute to be **Monday**.

The factory function I passed to the directive method is called when AngularJS encounters the highlight attribute in the HTML.

The directive function that the factory function creates is invoked by AngularJS and is **passed three arguments**:

*	the scope for the view

>>The scope argument lets me inspect the data that is available in the view; in this case, it allows me to get the value of the day property.

*	the element to which the directive has been applied

>>The attrs argument provides me with a complete set of the attributes that have been applied to the element, including the attribute that applies the directive

*	the attributes of that element.

>>**The element argument is a jqLite object**, which is the cut-down version of jQuery that is included with AngularJS.


###Defining Filters

Filters are used in views to **format the data displayed to the user**.

**Once defined, filters can be used throughout a module**, which means you can use them to ensure consistency in data presentation across multiple controllers and views.

`myApp.filter("dayName", function () {`

>>>`var dayNames = ["Sunday", "Monday", "Tuesday", "Wednesday","Thursday", "Friday", "Saturday"];`

>>>`return function (input) {`

>>>>`return angular.isNumber(input) ? dayNames[input] : input;`

>>>`};`

`});`

...

`<h4 ng-controller="dayCtrl" highlight="Monday">`

>>>`Today is { {day || "(unknown)" | dayName} }`

`</h4>`

The **filter method** is used to define a filter, and the arguments:

*	the **name of the new filter**

*	**a factory function** that will create the filter when invoked.

Filters are themselves functions, which receive a data value and format it so it can be displayed.

###Applying Filters

Filters are applied in **template expressions contained in views**.

The data binding or expression is followed by a **bar** (**the | character**) and then **the name of the filter**

`<h4 ng-controller="dayCtrl" highlight="Monday">`

>>>`Today is { {day || "(unknown)" | dayName} }`

`</h4>`

###Defining Services

Services are **singleton objects** that provide any functionality that you want to use throughout an application.

Three of the methods defined by the Module object are used to create services in different ways: **service, factory, and provider**.

###Defining Values

The Module.value method lets you **create services that return fixed values and objects**.

This may seem like an odd thing to do, but it means you can use dependency injection for any value or object, not just the ones you create using module methods like service and filter.

###Using Modules to Organize Code

Any AngularJS module can rely on components defined in other modules, and this is a feature that makes it easier to organize the code in a complex application. 

 One common convention is to organize your application into modules that have **the same type of component** and to make it clear which building block a module contains by using the main module’s name and appending the block type.


###Working with the Module Life Cycle

 The **Module.config and Module.run** methods register functions that are **invoked at key moments in the life cycle of an AngularJS app**.

 A function passed to the **config method** is invoked when **the current module has been loaded**,

 and a function passed to the **run method** is **invoked when all modules have been loaded**.




