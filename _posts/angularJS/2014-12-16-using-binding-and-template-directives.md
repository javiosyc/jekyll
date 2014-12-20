---
layout: post
title: "Using Binding and Template Directives"
description: ""
category: AngularJS
tags: [AngularJS]
---
{% include JB/setup %}

#Using Binding and Template Directives

Directives are the most powerful AngularJS feature; they allow you to extend HTML to create the foundation for rich and complex web applications in a way that is naturally expressive.

##Why and When to Use Directives

Directives are the signature feature of AngularJS, setting the overall style of AngularJS development and the shape of an AngularJS application.

AngularJS comes with more than 50 built-in directives that provide access to core features that are useful in almost every web application including **data binding**, **form validation**, **template generation**, **event handling**, and **manipulating HTML elements**.

####Why

>Directives expose core AngularJS functionality such as **event handling, form validation, and templates**. You use custom directives to apply your application features to views.

####When

>Directives are used throughout an AngularJS application.

##Using the Data Binding Directives

**The first category of built-in directives** is responsible for performing **data binding**, which is one of the features that elevates AngularJS from a template package into a full-fledged application development framework.


Data binding uses values **from the model and inserts them into the HTML document**.


The Data Binding Directives

|**Directive**   	|**Applied As**		|**Description**  |
|ng-bind		 	|Attribute, class   |Binds the innerText property of an HTML element.   |
|ng-bind-html	 	|Attribute, class   |Creates data bindings using the innerHTML property of an HTML element. This is potentially dangerous because it means that the browser will interpret the content as HTML, rather than content.|
|ng-bind-template   |Attribute, class   |Similar to the ng-bind directive but allows for multiple template expressions to be specified in the attribute value.|
|**ng-model**   	|Attribute, class   |**Creates a two-way data binding.**|
|ng-non-bindable    |Attribute, class   |Declares a region of content for which data binding will not be performed.|



###APPLYING DIRECTIVES


apply directives as **attributes**:

`There are <span ng-bind="todos.length"></span> items`

configure directives using the **standard class attribute**

`There are <span class="ng-bind: todos.length"></span> items`

###Performing One-Way Bindings (and **Preventing Them**)

AngularJS supports two kinds of data binding.

The first, **one-way binding**, means a value is taken from the data model and inserted into an HTML element.

AngularJS bindings are live, which means that when the value associated with the binding is changed in the data model, the HTML element will be **updated to display the new value**.

The **ng-bind directive** is responsible for creating one-way data bindings, but it is **rarely used** directly because AngularJS will also create this kind of binding whenever it encounters **the {{ and }} characters** in the HTML document.


{% raw %}`<div>There are {{todos.length}} items</div>`{% endraw %}

This is the most natural and expressive way of creating data bindings: The bindings are easy to read and fit naturally into the content of HTML elements.

The second data binding uses **the ng-bind directive**, which has the same effect but requires an additional element

`There are <span ng-bind="todos.length"></span> items`

The ng-bind directive replaces the content of the element that it is applied to, which means I **have to add a span element** to create the effect I want.

**I don’t use the ng-bind directive in my own projects; I prefer the inline bindings**.

Aside from being a little awkward to use, the ng-bind directive is **limited to being able to process a single data binding expression**.

If you need to create **multiple data bindings**, then you should use **the ng-bind-template directive**

{% raw %}`<div ng-bind-template="First: {{todos[0].action}}. Second: {{todos[1].action}}"></div>`{% endraw %}


###Preventing Inline Data Binding

The drawback of **the inline bindings** is that AngularJS will **find and process every set of {{ and }} characters in your content**.

The solution is to use **the ng-non-bindable directive**, **which prevents AngularJS from processing inline bindings**.

`<div ng-non-bindable>`

{% raw %}>`AngularJS uses {{ and }} characters for templates`{% endraw %}

`</div>`


###Creating Two-Way Data Bindings

**Two-way data bindings track changes in both directions, allowing elements that gather data from the user to modify the state of the application**.

Two-way bindings are created with **the ng-model directive**, a single data model property can be used for both one- and two-way bindings.

Two-way bindings can be applied **only to elements that allow the user to provide a data value**, which means the **input, textarea, and select elements**.

Changes to data model properties are **disseminated to all of the relevant bindings, ensuring that the application is kept in sync**.


##Using the Template Directives

Data bindings are the core feature of AngularJS views, but on their own they are pretty limited.

Web applications—or any kind of application for that matter—tend to operate on collections of similar data objects and vary the view they present to the user based on different data values.

Fortunately, AngularJS includes **a set of directives that can be used to generate HTML elements using templates**, making it easy to work with data collections and to add basic logic to a template that responds to the state of the data.

The Template Directives

|**Directive**   	|**Applied As**			|**Description**  |
|ng-cloak			|Attribute,class		|Applies a CSS style that hides inline binding expressions, which can be briefly visible when the document first loads|
|ng-include			|Element,Attribute,class|Loads, processes, and inserts a fragment of HTML into the Document Object Model|
|**ng-repeat**		|Attribute,class		|Generates new copies of a single element and its contents for each object in an array or property on an object|
|ng-repeat-start	|Attribute,class		|Denotes the start of a repeating section with multiple top-level elements |
|ng-repeat-end		|Attribute,class		|Denotes the end of a repeating section with multiple top-level elements|
|ng-switch			|Element,Attribute,class|Changes the elements in the Document Object Model based on the value of data bindings|

###Generating Elements Repeatedly

One of the most common tasks in any view is to generate the same content for each item in a collection of data.

In AngularJS this is done with **the ng-repeat directive**, which is applied to the element that should be duplicated.

There are two parts to using the ng-repeat directive.

The first is to specify the source of the data objects and the name by which you want to refer to the object that is being processed from within the template

`<tr ng-repeat="item in todos">`

The basic format of the value for the ng-repeat directive attribute is `<variable>` in `<source>`, where source is an object or array defined by **the controller $scope**, in this example the todos array.

The directive iterates through the objects in the array, creates a new instance of the element and its content, and then processes the templates it contains.

The `<variable>` name assigned in the directive attribute value can be used to refer to the current data object. 


`<tr ng-repeat="item in todos">`

{% raw %}
`<td>{{item.action}}</td>`

`<td>{{item.complete}}</td>`
{% endraw %}

`</tr>`

In my example, I generate a tr element that contains td elements that, in turn, contain inline data bindings that refer to the action and complete properties of the current object.

If you navigate to the directives.html file in the browser, AngularJS will **process the directive and generate the following HTML elements**:

`<tbody>`

>`<!-- ngRepeat: item in todos -->`

>`<tr ng-repeat="item in todos" class="ng-scope">`

>>`<td class="ng-binding">Get groceries</td>`

>>`<td class="ng-binding">false</td>`

>`</tr>`

>`<tr ng-repeat="item in todos" class="ng-scope">`

>>`<td class="ng-binding">Call plumber</td>`

>`<td class="ng-binding">false</td>`

>`</tr>`

>`<tr ng-repeat="item in todos" class="ng-scope">`

>>`<td class="ng-binding">Buy running shoes</td>`

>>`<td class="ng-binding">true</td>`

>`</tr>`

>`<tr ng-repeat="item in todos" class="ng-scope">`

>>`<td class="ng-binding">Buy flowers</td>`

>>`<td class="ng-binding">false</td>`

>`</tr>`

>`<tr ng-repeat="item in todos" class="ng-scope">`

>>`<td class="ng-binding">Call family</td>`

>>`<td class="ng-binding">false</td>`

>`</tr>`

`</tbody>`

###Repeating for Object Properties

The previous example used the ng-repeat directive to **enumerate the objects in an array**, but you can also **enumerate the properties of an object**.

`<tr ng-repeat="item in todos">`

{% raw %}
>>`<td ng-repeat="prop in item">{{prop}}</td>`
{% endraw %}

`</tr>`

###Working with Data Object Keys

There is an alternative syntax for the ng-repeat directive configuration that allows you to **receive a key with each property or data object that is processed**.

`<tr ng-repeat="item in todos">`

>`<td ng-repeat="(key, value) in item">`

{% raw %}
>`{{key}}={{value}}`
{% endraw %}

>`</td>`
`</tr>`


...

`<tr ng-repeat="item in todos" class="ng-scope">`

>`<!-- ngRepeat: (key, value) in item -->`

>`<td ng-repeat="(key, value) in item" class="ng-scope ng-binding">`

>>`action=Get groceries`

>`</td>`

>`<td ng-repeat="(key, value) in item" class="ng-scope ng-binding">`

>>`complete=false`

>`</td>`

`</tr>`

...

###Working with the Built-in Variables

The ng-repeat directive assigns the current object or property to the variable you specify, but there is also a set of built-in variables that provide context for the data being processed. 

|**Variable**	|**Description**|
|$index			|Returns the position of the current object or property|
|$first			|Returns true if the current object is the first in the collection|
|$middle		|Returns true if the current object is neither the first nor last in the collection
|$last			|Returns true if the current object is the last in the collection
|$even			|Returns true for the even-numbered objects in a collection
|$odd			|Returns true for the odd-numbered objects in a collection


###Repeating Multiple Top-Level Elements

The ng-repeat directive repeats a **single top-level element** and its contents for each object or property that it processes.

There are times, however, when you need to **repeat multiple top-level elements for each data object**.

I encounter this problem most often when I need to generate multiple table rows for each data item that I am processing—something that is difficult to achieve with ng-repeat because no intermediate elements are allowed between tr elements and their parents.

`<table class="table">`

`<tbody>`

>`<tr ng-repeat-start="item in todos">`

{% raw %}
>>`<td>This is item {{$index}}</td>`

>`</tr>`

>`<tr>`

>>`<td>The action is: {{item.action}}</td>`

>`</tr>`

>`<tr ng-repeat-end>`

>>`<td>Item {{$index}} is {{$item.complete? '' : "not "}} complete</td>`
{% endraw %}

>`</tr>`

`</tbody>`

`</table>`

The **ng-repeat-start directive** is configured just like ng-repeat, but **it repeats all of the top-level elements (and their contents) until (but including) the element to which the ng-repeat-end attribute has been applied**.

###Working with Partial Views

The **ng-include directive** **retrieves a fragment of HTML content from the server**, compiles it to process any directives that it might contain, and adds it to the Document Object Model.

These fragments are known as partial views.

`<div id="todoPanel" class="panel" ng-controller="defaultCtrl">`

>`<h3 class="panel-header">To Do List</h3>`

>`<ng-include src="'table.html'"></ng-include>`

`</div>`

The Configuration Parameters of the ng-include Directive

|Name		|Description																		|
|src		|Specifies the URL of the content to load											|
|onload		|Specifies an expression to be evaluated when the content is loaded					|
|autoscroll	|Specifies whether AngularJS should scroll the viewport when the content is loaded	|


###Selecting Partial Views Dynamically

`angular.module("exampleApp", [])`

>>`.controller("defaultCtrl", function ($scope) {`

>>`...`

>>>`$scope.viewFile = function () {`

>>>>`return $scope.showList ? "list.html" : "table.html";`

>>>`};`

>>`});`

>....

>`<ng-include src="viewFile()"></ng-include>`


###Using the ng-include Directive as an Attribute

...

`<div ng-include="viewFile()" onload="reportChange()"></div>`

...

The **ng-include attribute can be applied to any HTML element**, and **the value of the src parameter is taken from the attribute value**, which is viewFile() in this case.

The **other directive configuration parameters are expressed as separate attributes**, which you can see with the onload attribute.

###Conditionally Swapping Elements

The ng-include directive is excellent for managing more **significant fragments of content in partial**, but often you need to **switch between smaller chucks of content** that are already within the document—and for this, AngularJS provides the **ng-switch directive**.


`<div class="well">`

>`<div class="radio" ng-repeat="button in ['None', 'Table', 'List']">`

>>`<label>`

>>>`<input type="radio" ng-model="data.mode"`

{% raw %}
>>>>>`value="{{button}}" ng-checked="$first" />`

>>>>`{{button}}`

{% endraw %}

>>`</label>`

>`</div>`

`</div>`

`<div ng-switch on="data.mode">`

>`<div ng-switch-when="Table">`

>>`<table class="table">`

>>>`<thead>`

>>>>`<tr><th>#</th><th>Action</th><th>Done</th></tr>`

>>>`</thead>`

>>>`<tr ng-repeat="item in todos" ng-class="$odd ? 'odd' : 'even'">`

{% raw %}
>>>>`<td>{{$index + 1}}</td>`

>>>>`<td ng-repeat="prop in item">{{prop}}</td>`
{% endraw %}

>>>`</tr>`

>>`</table>`

>`</div>`

>`<div ng-switch-when="List">`

>>`<ol>`

>>>`<li ng-repeat="item in todos">`

{% raw %}
>>>>`{{item.action}}<span ng-if="item.complete"> (Done)</span>`
{% endraw %}

>>>`</li> </ol>`

>`</div>`

>`<div ng-switch-default>`

>>>`Select another option to display a layout`

>`</div>`

`</div>`

###Hiding Unprocessed Inline Template Binding Expressions


When working with **complex content on slow devices**, there can be a moment when the browser displays the HTML in the document while AngularJS is still parsing the HTML, processing the directives, and generally getting ready. 

A better alternative is to use **the ng-cloak directive**, which has **the effect of hiding content until AngularJS has finished processing it**.

The ng-cloak directive **uses CSS to hide the elements to which it is applied**, and the AngularJS library removes the CSS class when the content has been processed, ensuring that the user never sees the {% raw %}{{ and }}{% endraw %} characters of a template expression.

A common approach is to **apply the directive to the body element**, but that just means that the user sees an empty browser window while AngularJS processes the content.

I prefer to be more selective and **apply the directive only to the parts of the document where there are inline expressions*.


`<div class="well">`

>`<div class="radio" ng-repeat="button in ['None', 'Table', 'List']">`

>>`<label ng-cloak>`

>>>`<input type="radio" ng-model="data.mode"`

{% raw %}
>>>>`value="{{button}}" ng-checked="$first">`

>>>>>`{{button}}`
{% endraw %}

>>`</label>`

>`</div>`

`</div>`

`<div ng-switch on="data.mode" ng-cloak>`

>`<div ng-switch-when="Table">`

>>`<table class="table">`

>>>`<thead>`

>>>>`<tr><th>#</th><th>Action</th><th>Done</th></tr>`

>>>`</thead>`

>>>`<tr ng-repeat="item in todos" ng-class="$odd ? 'odd' : 'even'">`

{% raw %}
>>>>`<td>{{$index + 1}}</td>`

>>>>`<td ng-repeat="prop in item">{{prop}}</td>`
{% endraw %}

>>>`</tr>`

>>`</table>`

>`</div>`

....


