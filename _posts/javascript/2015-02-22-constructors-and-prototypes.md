---
layout: post
title: "CONSTRUCTORS AND PROTOTYPES"
description: "CONSTRUCTORS AND PROTOTYPES"
category: javascript
tags: [javascript]
---
{% include JB/setup %}

##Constructors

A constructor is simply a function that is used with **new** to create an object.

ex: Object, Array, and Function.

The advantage of constructors is that objects created with the same constructor contain the same properties and methods.

Because a constructor is **just a function**, you define it in the same way.

The only difference is that constructor names should **begin with a capital letter**, to distinguish them from other functions.

`function Person() {`

`    // intentionally empty`

`}`

`var person1 = new Person();`

`var person2 = new Person();`

When you have no parameters to pass into your constructor, you can even omit the parentheses:

`var person1 = new Person;`

`var person2 = new Person;`



