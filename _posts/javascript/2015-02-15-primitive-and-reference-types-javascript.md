---
layout: post
title: "Primitive and Reference Types - Javascript"
description: ""
category: javascript 
tags: [javascript]
---
{% include JB/setup %}

##Type

1.primitive types
	
>are stored as simple data types.

2.Reference types

>are stored as objects, which are really just references to locations in memory.


Javascript lets you treat primitives types like reference type in order to make the language more consistent for the developer.

Javascript tracks variables for a **particular scope with a variable object**.

Primitive values are stored directly on the variable object, while reference values are placed as **a pointer in the variable object**, which serves as a reference to a location in memory where the object is stored.



### Primtive type 

| **Type** 		| |
|Boolean		| true or false|
|Number			| Any integer or floating-point numeric value |
|String			| A character or sequence of character delimited by either single or double quotes (Javascript has no separate character type)|
|Null			| A primitive type that has only one value, null|
|Undefined		| A primitive type that has only one value, undefined (undefined is the value assigned to a variable that is not initialized) |


A variable holding a primitive directly contains the primitive value (rather than a pointer to an objects).

When you assign a primitive value to a variable, the value is copied into that variable.


|Variable Object|
|color1 | red   |
|color2 | blue  |



#### Identifying Primitive Types

The best way to identfiy primitive types is with the **typeof operator**, which works on any variable and returns **a string indicating the type of data**.

console.log(typeof "Nicholas"); // "string"

console.log(typeof 10); // "number"

console.log(typeof 5.1); // "number"

console.log(typeof true); // "boolean"

console.log(typeof undefined); // "undefined"


***When you run typeof null, the result is "object"***

console.log(typeof null);           // "object"

The best way to determined if a value is null is compare it against null directly, like this:

**console.log (value === null) **

The triple equals operator (===) does the comparison without coercing the variable to another type.


#### Primitive Methods

String	toLowerCase(), charAt(0), substring(2,5)

Number	toFixed(2), toString()

Boolean	toString()



### Reference Types

**Reference types represent objects in JavaScript** and are the closest things to classes that you will find in the language.

Reference values are instances of reference types and are synonymous with objects.

An object is **an unordered list of properties consisting of a name (always a string) and a value**.

When the value of a property is a function, it is called a method.

Functions themselves are actually referece values in JavaScript, so there's little difference between a property that contains an array and one that contains a function except that a function can be executed.

#### Create Objects

|Object |
|name|value|
|name|value|

Structure of an object

There are a couple of ways to create ,or instantiate, objects.

1.The first is to use the **new operator with a constructor**. (A contructor is simply a function that users new to create an object- any function can be a constructor.)

By convention, **constructors in JavaScript begin with a capital letter** to distinguish them from nonconstructor functions.

var object = new Object();

Object variable holds **a pointer** (or reference) to the location in memory where the object exists.

That means if you assign one variable to another, each variable gets a copy of 
the pointer, and both still reference the same object in memory.


### Dereferencing Objects

JavaScript is **a garbage-collected language**, so you don’t really need to worry about memory allocations when you use reference types.

However, it’s best to dereference objects that you no longer need so that the garbage collector can free up that memory.

**The best way to do this is to set the object variable to null.**

var object1 = new Object();

// do something

object1 = null;     // dereference

### Adding or Removing Properties

**Another interesting aspect of objects in JavaScript is that you can add and remove properties at any time**.


### Instantiating Built-in Types

The Object type is just one of a handful of built-in reference types that JavaScript provides. 

The built-in types are:

|Array	|An ordered list of numerically indexed values|
|Date	|A date and time|
|Error	|A runtime error (there are also several more specific error subtypes)|
|Function|A function	|
|Object	|A generic object 	|
|RegExp |A regular expression|

var items = new Array();

var now = new Date();

var error = new Error("Something bad happened.");

var func = new Function("console.log('Hi');");

var object = new Object();

var re = new RegExp("\\d+");


### Literal Forms

A literal is **syntax that allows you to define a reference value without explicitly creating an object, using the new operator and the object’s constructor.**


### Object and Array Literals

To create an object with object literal syntax, you can **define the properties of a new object inside braces**.

Properties are made up of **an identifier or string, a colon, and a value, with multiple properties separated by commas**.

var book = {

    name: "The Principles of Object-Oriented JavaScript",

    year: 2014

};

You can also use **string literals as property names**, which is useful when you want a property name to have **spaces or other special characters**.

var book = {

    "name": "The Principles of Object-Oriented JavaScript",

    "year": 2014

};



You can define an array literal in a similar way by enclosing any num- ber of comma-separated values inside **square brackets**.

var colors = [ "red", "blue", "green" ];

console.log(colors[0]);     // "red"

This code is equivalent to the following:

var colors = new Array("red", "blue", "green")

console.log(colors[0]);     // "red"


### Function Literals


In fact, using the Function constructor is typically discouraged given the challenges of maintaining, reading, and debugging a string of code rather than actual code, so you’ll rarely see it in code.

function reflect(value) {

            return value;

}

// is the same as

var reflect = new Function("value", "return value;");

### Regular Expression Literals

JavaScript also has regular expression literals that allow you to define regular expressions without using the RegExp constructor.

**Regular expression literals look very similar to regular expressions in Perl**: **The pattern is contained between two slashes**, **and any additional options are single characters following the second slash**.

var numbers = /\d+/g;

// is the same as

var numbers = new RegExp("\\d+", "g");

The literal form of regular expressions in JavaScript is a bit easier
to deal with than the constructor form because you don’t need to worry about escaping characters within strings.

### Property Access

Properties are name/value pairs that are stored on an object.

**Dot notation is the most common way to access properties in JavaScript** (as in many object-oriented languages), but you can also access properties on JavaScript objects by **using bracket notation with a string**.

var array = [];
 
array.push(12345);

With bracket notation ..

var array = [];

array["push"](12345);

This syntax is very useful when you want to dynamically decide which property to access.

var array = [];

var method = "push";

array[method](12345);


The point to remember is that, other than syntax, the only difference—performance or otherwise—between dot notation and bracket notation is that bracket notation allows you to use special characters in property names.

### Identifying Reference Types

**A function is the easiest reference type** to identify because when you use the typeof operator on a function, the operator should return "function".

`function reflect(value) {`

`            return value;`

`        }`

`        console.log(typeof reflect);    // "function"`

Other reference types are trickier to identify because, for **all reference types other than functions, typeof returns "object"**.

The instanceof operator takes an object and a constructor as parameters.

`var items = [];`

`var object = {};`

`function reflect(value) {`

`    return value;`

`}`

`console.log(items instanceof Array);// true`

`console.log(object instanceof Object);// true`

`console.log(reflect instanceof Function);// true`

The instanceof operator can identify inherited types.

That means every object is actually an instance of Object because every reference type inherits from Object.

`var items = [];`

`var object = {};`

`function reflect(value) {`

`    return value;`

`}`

`console.log(items instanceof Array);` // true

`console.log(items instanceof Object);` // true

`console.log(object instanceof Object);` // true

`console.log(object instanceof Array);`  // false

`console.log(reflect instanceof Function);` // true

`console.log(reflect instanceof Object);` // true

### Identifying Arrays

Although instanceof can identify arrays, there is one exception that affects web developers: **JavaScript values can be passed back and forth between frames in the same web page.**

This becomes a problem only when you try to identify the type of a reference value, because **each web page has its own global context—its own version of Object, Array, and all other built-in types.

To solve this problem, **ECMAScript 5 introduced Array.isArray(), which definitively identifies the value as an instance of Array regardless
of the value’s origin**.

 If your environment is ECMAScript 5 compliant, Array.isArray() is the best way to identify arrays.


`var items = [];`

`console.log(Array.isArray(items));      // true`

 This method isn’t supported in Internet Explorer 8 and earlier.

### Primitive Wrapper Types

There are three primitive wrapper types (String, Number, and Boolean).

**These special reference types exist to make working with primitive values as easy as working with objects**.

(It would be very confusing if you had to use a different syntax or switch to a procedural style just to get a substring of text.)



The primitive wrapper types are reference types that are **automatically created behind the scenes whenever strings, numbers, or Booleans are read**.

`var name = "Nicholas";  // a primitive string value is assigned to name` 

`var firstChar = name.charAt(0); // treats name like an object and calls charAt(0) using dot notation.`

`console.log(firstChar);                 // "N"`

This is what happens behind the scenes:

`// what the JavaScript engine does`

`var name = "Nicholas";`

`var temp = new String(name);`

`var firstChar = temp.charAt(0);`

`temp = null;`

`console.log(firstChar); // "N" `

Because the second line uses a string (a primitive) like an object, the JavaScript engine creates an instance of String so that charAt(0) will work.

**The String object exists only for one statement before it’s destroyed (a process called autoboxing).**

`var name = "Nicholas";`

`name.last = "Zakas";`

`console.log(name.last);                 // undefined`

 With primitive wrapper types, properties seem to disappear because the object on which the property was assigned is destroyed immediately afterward.

` //what the JavaScript engine does`

`var name = "Nicholas";`

`var temp = new String(name);`

`temp.last = "Zakas";`

`temp = null; 	// temporary object destroyed`

`var temp = new String(name);`

`console.log(temp.last); // undefined`

`temp = null;`

Instead of assigning a new property to a string, the code actually creates a new property on a temporary object that is then destroyed.

When you try to access that property later, a different object is temporarily created and the new property doesn’t exist there.

Manually instantiating primitive wrappers can also be confusing in other ways, so unless you find a special case where it makes sense to do so, you should avoid it.

**Most of the time, using primitive wrapper objects instead of primitives only leads to errors.**

###Summary

**While JavaScript doesn’t have classes, it does have types.**

**Each variable or piece of data is associated with a specific primitive or reference type.**

The five primitive types (strings, numbers, Booleans, null, and undefined) represent simple values stored directly in the variable object for a given context.

You can use **typeof to identify primitive types with the exception of null, which must be compared directly against the special value null**.


Reference types are the closest thing to classes in JavaScript, and objects are instances of reference types.

You can create new objects using the **new operator or a reference literal**.

You access properties and methods primarily using dot notation, but you can also use bracket notation.

Functions are objects in JavaScript, and you can identify them with the typeof operator.

You should use **instanceof** with a constructor to identify objects of any other reference type.

To make primitives seem more like references, JavaScript has three primitive wrapper types: String, Number, and Boolean. JavaScript creates these objects behind the scenes so that you can treat primitives like regular objects, but the temporary objects are destroyed as soon as the statement using them is complete.

Although you can create your own instances of primitive wrappers, it’s best not to do that because it can be confusing.
