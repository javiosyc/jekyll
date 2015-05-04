---
layout: post
title: "Working with Forms"
description: ""
category: [AngularJS]
tags: [AngularJS form]
---
{% include JB/setup %}

|Problem|Solution|Listing|



###Using Form Elements with Two-Way Data Bindings

..

`<td>`

>`<input type="checkbox" ng-model="item.complete"`

`</td>`

###Implicitly Creating Model Properties

The previous example operates on model properties that I explicitly defined when I set up the controller, but you can also **use two-way data bindings to implicitly create properties in the data model**—a feature that is useful when you are using form elements to gather data from the user in order to create a new object or property in the data model.

`<div class="row">`

>`<div class="col-xs-6">`

>>`<div class="well">`

>>>`<div class="form-group row">`

>>>>`<label for="actionText">Action:</label>`

>>>>`<input id="actionText" class="form-control"`

>>>>>`ng-model="newTodo.action">`

>>>`</div>`

>>>`<div class="form-group row">`

>>>>`<label for="actionLocation">Location:</label>`

>>>>`<select id="actionLocation" class="form-control"`

>>>>>`ng-model="newTodo.location">`

>>>>>`<option>Home</option>`

>>>>>`<option>Office</option>`

>>>>>`<option>Mall</option>`

>>>>`</select>`

>>>`</div>`

>>>`<button class="btn btn-primary btn-block"`

>>>>`ng-click="addNewItem(newTodo)">`

>>>>>`Add`

>>>`</button>`

>>`</div>`

>`</div>`





...

`<input id="actionText" class="form-control" ng-model="newTodo.action">`

...


and this select element:

`<select id="actionLocation" class="form-control" ng-model="newTodo.location">`

>>`<option>Home</option>`

>>`<option>Office</option>`

>>`<option>Mall</option>`

`</select>`

They both use the ng-model directive, **configured to update model properties that I have not explicitly defined: the newTodo.action and newTodo.location properties**.

###Checking That the Data Model Object Has Been Created

Using an implicitly defined object on which properties are defined has some benefits, **such as being able to call the behavior that processes the data in a clean and simple way**.

But it has a drawback as well, which you can see **if you reload the forms.html file in the browser and click the Add button without editing the input element or selecting an option for the select element**.

The interface won’t change when you click the button, but you’ll see an error message like this one in the JavaScript console:


TypeError: Cannot read property 'action' of undefined


**The problem is that my controller behavior is trying to access properties on an object that AngularJS won’t create until the one of the form controls has been modified, triggering the ng-model directive.**

When relying on implicit definition, **it is important to write your code to cater for the possibility that the objects or properties you are going to use do not yet exist**.

`$scope.addNewItem = function (newItem) {`

>`if (angular.isDefined(newItem) && angular.isDefined(newItem.action) && angular.isDefined(newItem.location)) {`

>>`$scope.todos.push({`

>>>`action: newItem.action + " (" + newItem.location + ")",`

>>>>`complete: false`

>`}); }`

`};`

##Validating Forms

###Performing Basic Form Validation

AngularJS provides basic form validation by honoring the standard HTML element attributes, such as type and required, and adding some directives.

###Adding the Form Element

...

`<form name="myForm" novalidate ng-submit="addUser(newUser)">`

There are some attributes you must set on the form element to get the best results from AngularJS form validation.

The first is the **name attribute**; the directive that replaces the form element defines some useful variables that report on the validity of the form data and accesses these values via the value of the name property.

**To disable the browser validation support and enable the AngularJS features**, I have **added the novalidate attribute to my form element**, which is defined by the HTML5 specification and tells the browser not to try to validate the form itself, allowing AngularJS to work unhindered.

The last addition to the **form element is the ng-submit directive**.

As a reminder, this directive specifies a custom response to a submit event, which is triggered when the user submits the form (in this example by clicking the OK button).

###Using Validation Attributes

`<input name="userEmail" type="email" class="form-control"`
>`required ng-model="newUser.email">`

The type Attribute Values for Input Elements

|Type Value	|Description|
|checkbox	|Creates a check box (pre-dates HTML5)|
|email		|Creates a text input that accepts an e-mail address (new in HTML5)|
|number		|Creates a text input that accepts a number address (new in HTML5)|
|radio		|Creates a radio button (pre-dates HTML5)|
|text		|Creates a standard text input that accepts any value (pre-dates HTML5)|
|url		|Creates a text input that accepts a URL (new in HTML5)|

...

`<input name="userName" type="text" class="form-control" required ng-model="newUser.name">`

...

...

`<input name="agreed" type="checkbox" ng-model="newUser.agreed" required>`

...

###Monitoring the Validity of the Form

The directives that AngularJS uses to replace the standard form elements define special variables that you can use to check the validation state of individual elements or the form as a whole.

|Variable	|Description|
|$pristine	|Returns true if the user has **not interacted** with the element/form|
|$dirty		|Returns true if the user has interacted with the element/form|
|$valid		|Returns true if the contents of the element/form are valid|
|$invalid	|Returns true if the contents of the element/form are invalid|
|$error		|Provides details of validation errors—see the “Providing Form Validation Feedback” section for details|

...

`<div>Valid: {{myForm.$valid}}</div>`

...

...

`<button type="submit" class="btn btn-primary btn-block" ng-disabled="myForm.$invalid">`

`OK </button>`

...

##Providing Form Validation Feedback

AngularJS performs validation checks as the user is interacting with the form elements, and we can use the information that these checks provide to give the user meaningful feedback in real time, rather than waiting until they are ready to submit the data.

###Using CSS to Provide Feedback

**Each time the user interacts with an element** that is being validated, AngularJS checks its state to see whether it is valid.

The validity checks depend on the element type and how it has been configured.


AngularJS reports on the outcome of these validation checks by adding and removing the elements it validates from a set of classes, which can be combined with CSS to provide feedback to the user by styling the element.

AngularJS uses four basic classes:

|Variable		| Description|
|ng-pristine	|Elements that the user has not interacted are added to this class.|
|ng-dirty		|Elements that the user has interacted are added to this class.|
|ng-valid		|Elements that are valid are in this class.|
|ng-invalid		|Elements that are not valid are in this class.|


...

`<style>`
>`form .ng-invalid.ng-dirty { background-color: lightpink; }`

>`form .ng-valid.ng-dirty { background-color: lightgreen; }`

>`span.summary.ng-invalid { color: red; font-weight: bold; }`

>`span.summary.ng-valid { color: green; }`

`</style>`

`<div class="well">`

{% raw %}
>`Message: {{message}}`

>>`<div>`

>>>`Valid:`

>>>>`<span class="summary" ng-class="myForm.$valid ? 'ng-valid' : 'ng-invalid'">`

>>>>>`{{myForm.$valid}}`

>>>>`</span>`

>>`</div>`

{% endraw %}

`</div>`

###Providing Feedback for Specific Validation Constraints

`<style>`

`form .ng-invalid-required.ng-dirty { background-color: lightpink; }`

`form .ng-invalid-email.ng-dirty { background-color:lightgoldenrodyellow; }`

`form .ng-valid.ng-dirty { background-color: lightgreen; }`

`span.summary.ng-invalid {color: red; font-weight: bold; }`

`span.summary.ng-valid { color: green }`

`</style>`

AngularJS will add the element to the **ng-valid-required** and **ng-invalid-required classes** to signal compliance with the required attribute and **use the ng-valid-email and ng-invalid-email classes* to single compliance with the formatting constraint.

###Using the Special Variables to Provide Feedback

AngularJS provides a set of special variables for form validation that you can use in views to check the validation status of individual elements and of the form as a whole.

I used these variables to control the disabled state of a button in earlier examples by applying the ng-disabled directive, but they can also be used to control visibility of elements that give feedback to the user by applying them with the ng-show directive.

..

`div.error {color: red; font-weight: bold;}`

...

`<div class="error"`

>`ng-show="myForm.userEmail.$invalid && myForm.userEmail.$dirty">`

>>`<span ng-show="myForm.userEmail.$error.email">`

>>>`Please enter a valid email address`

>>`</span>`

>>`<span ng-show="myForm.userEmail.$error.required">`

>>>`Please enter a value`

>>`</span>`

`</div>`

###Reducing the Number of Feedback Elements

`$scope.getError = function (error) {`

>`if (angular.isDefined(error)) {`

>>`if (error.required) {`

>>>`return "Please enter a value";`

>>`} else if (error.email) {`

>>>`return "Please enter a valid email address";`

>>`}}`

`<div class="error" ng-show="myForm.userEmail.$invalid && myForm.userEmail.$dirty">`

{% raw %}
`{{getError(myForm.userEmail.$error)}}`
{% endraw %}

`</div>`

###Deferring Validation Feedback


}
}