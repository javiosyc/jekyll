---
layout: post
title: "understanding_objects"
description: "Understanding Objects"
category: javascript
tags: [javascript,object]
---
{% include JB/setup %}

Even though there are a number of built- in reference types in JavaScript, you will most likely create your own objects fairly frequently.

As you do so, **keep in mind that objects in JavaScript are dynamic, meaning that
they can change at any point during code execution**.

Whereas class-based languages lock down objects based on a class definition, JavaScript objects have no such restrictions.

A large part of JavaScript programming is managing those objects, which is why understanding how objects work is key to understanding JavaScript as a whole.

## Defining Properties

When a property is first added to an object, JavaScript **uses an internal method called [[Put]] on the object**.

**The [[Put]] method creates a spot in the object to store the property**.

You can compare this to adding a key to a hash table for the first time.

This operation specifies not just the initial value, but also some attributes of the property.

So, in the previous example, when the name and age properties are first defined on each object, the [[Put]] method is invoked for each.

The result of calling [[Put]] is the creation of an own property on the object.

An own property simply indicates that the specific instance of the object owns that property.

The property is stored directly on the instance, and all operations on the property must be performed through that object.

**When a new value is assigned to an existing property, a separate operation called [[Set]] takes place**.


![](/assets/picture/javascript/adding_and_changing_properties_of_an_object.png)


## Detecting Properties

The **in operator** looks for a property with a given name in a specific object and returns true if it finds it.

In effect, the in operator checks to see if the given key exists in the hash table.


`console.log("name" in person1);  // true`

`console.log("age" in person1);  // true`

`console.log("title" in person1);  // false`

Keep in mind that methods are just properties that reference functions, so you can check for the existence of a method in the same way.

`var person1 = {`

`    name: "Nicholas",`

`    sayName: function() {`

`        console.log(this.name);`

`    }`

`};`

`console.log("sayName" in person1);  // true`


In some cases, however, you might want to check for the existence of a property only if it is an own property.

**The in operator checks for both own properties and prototype properties**, so you’ll need to take a different approach.

**Enter the hasOwnProperty() method, which is present on all objects and returns true only if the given property exists and is an own property**.


`var person1 = {`

`       name: "Nicholas",`

`       sayName: function() {`

`           console.log(this.name);`

`       }`

`   };`

`   console.log("name" in person1);  // true`

`   console.log(person1.hasOwnProperty("name")); // true`

`   console.log("toString" in person1);			// true`

`	console.log(person1.hasOwnProperty("toString"));// false`

### Removing Properties

Just as properties can be added to objects at any time, they can also be removed. 

**Simply setting a property to null doesn’t actually remove the property completely from the object, though**.

Such an operation calls [[Set]] with a value of null, which, as you saw earlier in the chapter, only replaces the value of the property.

You need to **use the delete operator to completely remove a property from an object**.

The **delete operator works on a single object property** and calls an internal operation named [[Delete]].

You can think of this operation as removing a key/value pair from a hash table.

When the delete operator is successful, it returns true.

`var person1 = {`

`       name: "Nicholas"`

`   };`

`   console.log("name" in person1);	//true`

`   delete person1.name;		// true - not output`

`	console.log("name" in person1); //false`

`	console.log(person1.name); //undefined`

![](/assets/picture/javascript/delete_name_property.png)


### Enumeration

By default, all properties that you add to an object are enumerable, which means that you can iterate over them using a for-in loop.

**Enumerable properties have their internal [[Enumerable]] attributes set to true**.

The for-in loop enumerates all enumerable properties on an object, assigning the property name to a variable.

`var property;`

`        for (property in object) {`

`            console.log("Name: " + property);`

`            console.log("Value: " + object[property]);`

`}`


If you just need a list of an object’s properties to use later in your program, ECMAScript 5 introduced the Object.keys() method to retrieve an array of enumerable property names, as shown here:


`var properties = Object.keys(object);`

`// if you want to mimic for-in behavior`

`var i, len;`

`        for (i=0, len=properties.length; i < len; i++){`

`            console.log("Name: " + properties[i]);`

`            console.log("Value: " + object[properties[i]]);`

`}`

There is a difference between the enumerable properties returned in a for-in loop and the ones returned by Object.keys().

The for-in loop also enumerates prototype properties, while Object.keys() returns only own (instance) properties.

The differences between prototype and own properties are discussed in Chapter 4.


**Keep in mind that not all properties are enumerable**.

In fact, most of the native methods on objects have their [[Enumerable]] attribute set to false.

You can check whether a property is enumerable by using **the propertyIsEnumerable() method**, which is present on every object:

`var person1 = {`

`            name: "Nicholas"`

`        };`

`        console.log("name" in person1); //true`

`		 console.log(person1.propertyIsEnumerable("name")); //true`

`        var properties = Object.keys(person1);`

`		 console.log("length" in properties);  //true`

`		 console.log(properties.propertyIsEnumerable("length")); // false`



### Types of Properties


There are two different types of properties: **data properties and accessor properties**.

Data properties contain a value, like the name property from earlier examples in this chapter.

The default behavior of the [[Put]] method is to create a data property, and every example up to this point in the chapter has used data properties.

Accessor properties don’t contain a value but instead define a function to call when the property is read (called a getter), and a function to call when the property is written to (called a setter).

Accessor properties only require either a getter or a setter, though they can have both.

There is a **special syntax to define an accessor property using an object literal**:


`var person1 = {`

`	_name: "Nicholas",`

`	get name() {`

`		console.log("Reading name");`

`       return this._name;`

`       },`

`	set name() {`

`		console.log("Setting name to %s" , value);`

`		this._name = value;`

`	}`

`};`

`console.log(person1.name) // "Reading name" then "Nicholas"`

`person1.name="Greg";`

`console.log(person1.name); //"Setting name to Greg" then "Greg"`


Accessor properties are most useful when you want the assignment of a value to trigger some sort of behavior, or when reading a value requires the calculation of the desired return value.


You don’t need to define both a getter and a setter; you can choose one or both.

If you define only a getter, then the property becomes read-only, and attempts to write to it will fail silently in nonstrict mode and throw an error in strict mode.

If you define only a setter, then the property becomes write-only, and attempts to read the value will fail silently in both strict and nonstrict modes.


### Property Attributes

Prior to ECMAScript 5, there was no way to specify whether a property should be enumerable.

In fact, there was no way to access the internal attributes of a property at all.

ECMAScript 5 changed this by introducing several ways of interacting with property attributes directly, as well as introducing new attributes to support additional functionality.


#### Common Attributes

There are two property attributes shared between data and accessor properties.

**One is [[Enumerable]], which determines whether you can iterate over the property**.

**The other is [[Configurable]], which determines whether the property can be changed**.

You can remove a configurable property using delete and can change its attributes at any time. (This also means configurable properties can be changed from data to accessor properties and vice versa.) 

**By default, all properties you declare on an object are both enumerable and configurable**.

If you want to change property attributes, you can **use the Object .defineProperty() method**. 

This method **accepts three arguments: the object that owns the property, the property name, and a property descriptor object containing the attributes to set**.

**The descriptor has properties with the same name as the internal attributes but without the square brackets**.


`var person1 = {`

`	name: "Nicholas"`

`};`

`Object.defineProperty(person1, "name", {`

`	enumerable: false`

`   });`

`   **console.log("name" in person1);  //true`

`	**console.log(person1.propertyIsEnumerable("name"));  // false **`

`	var properties = Object.keys(person1);`

`   console.log(properties.length);		//0`

`Object.defineProperty(person1, "name", {`

`	configurable: false`

`});`

`   // try to delete the Property`

`	**delete person1.name;**`

`	**console.log("name" in person1);		//true**`

`	console.log(person1.name);		// "Nicholas`

`	Object.defineProperty(person1, "name", {	//error`

`		configurable: true`

`});`

The last piece of the code tries to redefine name to be configurable once again.

However, this throws an error because **you can’t make a nonconfigurable property configurable again**.

Attempting to change a data property into an accessor property or vice versa should also throw an error in this case.


When JavaScript is running in strict mode, attempting to delete a nonconfigurable property results in an error.

In nonstrict mode, the operation silently fails.

#### Data Property Attributes


Data properties possess two additional attributes that accessors do not.

**The first is [[Value]], which holds the property value**.

This attribute is filled in automatically when you create a property on an object.

All property values are stored in [[Value]], even if the value is a function.

**The second attribute is [[Writable]], which is a Boolean value indicating whether the property can be written to**.

**By default, all properties are writable unless you specify otherwise**.

With these two additional attributes, **you can fully define a data property using Object.defineProperty() even if the property doesn’t already exist**. Consider this code:


`var person1 = {};`

`Object.defineProperty(person1, "name", {`

`    value: "Nicholas",`

`    enumerable: true,`

`    configurable: true,`

`    writable: true`

`});`

When Object.defineProperty() is called, it first checks to see if the property exists.

If the property doesn’t exist, a new one is added with the attributes specified in the descriptor.

**When you are defining a new property with Object.defineProperty(), it’s important to specify all of the attributes because Boolean attributes automatically default to false otherwise.**

For example, the following code creates a name property that is nonenumerable, nonconfigurable, and nonwritable because it doesn’t explicitly make any of those attributes true in the call to Object.defineProperty().


`var person1 = {};`

`Object.defineProperty(person1, "name", {`

`    value: "Nicholas"`

`});`

`console.log("name" in person1);`

`console.log(person1.propertyIsEnumerable("name"));`

`delete person1.name;`

`console.log("name" in person1);`

`person1.name = "Greg";`

`console.log(person1.name);`

#### Accessor Property Attributes

Accessor properties also have two additional attributes.

Because there is no value stored for accessor properties, there is no need for [[Value]] or [[Writable]].

Instead, **accessors have [[Get]] and [[Set]], which contain the getter and setter functions, respectively**.

As with the object literal form of getters and setters, you need only define one of these attributes to create the property.


**The advantage of using accessor property attributes instead of object literal notation to define accessor properties is that you can also define those properties on existing objects.**


`var person1 = {`

`    _name: "Nicholas",`

`    get name() {`

`        console.log("Reading name");`

`        return this._name;`

`},`

`    set name(value) {`

`        console.log("Setting name to %s", value);`

`        this._name = value;`

`} };`

This code can also be written as follows:

`var person1 = {`

`    _name: "Nicholas"`

`};`

`Object.defineProperty(person1, "name", {`

`    **get: function() {**`

`        console.log("Reading name");`

`        return this._name;`

`    },`

`    **set: function(value) {**`

`        console.log("Setting name to %s", value);`

`        this._name = value;`

`    },`

`    enumerable: true,`

`    configurable: true`

`});`


Notice that the get and set keys on the object passed in to Object .defineProperty() are data properties that contain a function.


Setting the other attributes ([[Enumerable]] and [[Configurable]]) allows you to change how the accessor property works.

For example, you can create a nonconfigurable, nonenumerable, nonwritable property like this:

`var person1 = {`

`    _name: "Nicholas"`

`};`

`Object.defineProperty(person1, "name", {`

`   get: function() {`

`       console.log("Reading name");`

`        return this._name;`

`} });`

`console.log("name" in person1);		// true`

`console.log(person1.propertyIsEnumerable("name"));	// false`

`delete person1.name;`

`console.log("name" in person1);	// true`

`person1.name = "Greg";`

`console.log(person1.name);		// "Nicholas"`

As with accessor properties defined via object literal notation, an accessor property without a setter throws an error in strict mode when you try to change the value.

In nonstrict mode, the operation silently fails.

Attempting to read an accessor property that has only a setter defined always returns undefined.


**** Defining Multiple Properties

It’s also possible to define multiple properties on an object simultaneously if you **use Object.defineProperties() instead of Object.defineProperty()**.

This method accepts two arguments: **the object to work on and an object containing all of the property information**.

The keys of that second argument are property names, and the values are descriptor objects defining the attributes for those properties.

For example, the following code defines two properties:

`var person1 = {};`

`Object.defineProperties(person1, {`

`// data property to store data`

`	_name: {`

`           value: "Nicholas",`

`           enumerable: true,`

`           configurable: true,`

`           writable: true`

`	},`

`// accessor property name: {`

`           get: function() {`

`               console.log("Reading name");`

`               return this._name;`

`           },`

`           set: function(value) {`

`               console.log("Setting name to %s", value);`

`               this._name = value;`

`           },`

`           enumerable: true,`

`           configurable: true`

`       }`

`});`

#### Retrieving Property Attributes

**If you need to fetch property attributes, you can do so in JavaScript by using Object.getOwnPropertyDescriptor().**

As the name suggests, this method works only on own properties.

**This method accepts two arguments: the object to work on and the property name to retrieve.**

**If the property exists, you should receive a descriptor object with four properties: configurable, enumerable, and the two others appropriate for the type of property.**

`var person1 = {`

`       name: "Nicholas"`

`   };`

`var descriptor = Object.getOwnPropertyDescriptor(person1, "name");`

`console.log(descriptor.enumerable);	// true`

`console.log(descriptor.configurable);	// true`

`console.log(descriptor.writable);		// true`

`console.log(descriptor.value);			// "Nicholas`


### Preventing Object Modification

** Object, just like properties, have internal attributes that govern their behavior**.

One of these attributes is **[[Extensible]], which is a Boolean value indicating if the object itself can be modified**.

All objects you create are extensible by default, meaning new properties can be added to the object at any time.

#### Preventing Extensions

One way to create a nonextensible object is with Object.preventExtensions().

This method accepts a single argument, which is the object you want to make nonextensible.

You can check the value of [[Extensible]] by using Object.isExtensible().

		var person1 = {

			name: "Nicholas"

		};

		console.log(Object.isExtensible(person1)); //true

		Object.preventExtensions(person1);

		console.log(Object.isExtensible(person1)); //false

		person1.sayName = function() {

			console.log(this.name);


		};

		console.log("sayName" in person1) ; //false


#### Sealing Objects

** The second way to create a nonextensible object is to seal the object **.

A sealed object is nonextensible, and all of its properties are nonconfigurable.

That means not only can you not add new properties to the object but you also can't remove properties or change their type (from data to accessor or vice versa).

If an object is sealed, you can only read from and write to its properties.

You can use ** the Object.seal()** method on an object to seal it.

When that happens, the [[Extensible]] attribute is set to fasle, and all properties have their [[Configurable]] attribute set to false.


		var person1 = {

			name: "Nicholas"

		};

		console.log(Object.isExtensible(person1));	//true

		console.log(Object.isSealed(person1));		//false

		Object.seal(person1);

		console.log(Object.isExtensible(person1));	//fasle

		console.log(Object.isSealed(person1));		//true

		person1.sayName = function() {

			console.log(this.name);

		};

		console.log("sayName" in person1);			//false

		person1.name = "Greg";

		console.log(person1.name);					//"Greg"

		delete person1.name;

		console.log("name" in person1);			//true

		console.log(person1.name);				//"Greg"

		var descriptor = Object.getOwnPropertyDescriptor(person1,"name");

		console.log(descriptor.configurable);	//false


#### Freezing Object

The last way to create a nonextensible object is to freeze it.

If ana object is frozen, you can't add or remove properties, you can't change properties' types, and you can't write to any data properties.

In essence, a frozen object is a sealed object where data properties are also read-only.

You can freeze an object by using Object.freeze() and determine if an object is frozen by using Object.isFrozen().

		var person1 = {

			name: "Nicholas"

		};

		console.log(Object.isExtensible(person1));	//true

		console.log(Object.isSealed(person1));		//false

		console.log(Object.isFrozen(person1));		//false

		Object.freeze(person1);

		console.log(Object.isExtensible(person1));	//false

		console.log(Object.isSealed(person1));		//true

		console.log(Object.isFrozen(person1));		//true



### Summary 

It helps to think of JavaScript objects as hash maps where properties are just key/value pairs.

You can add a property at any time by assigning a value to it, and you can remove a property at any time with the **delete operator**.

You can always check whether a property exists by using **the in operator on a property name and object**.

If the property in question is an own property, you could also use hasOwnProperty(), which exists on every object.

All object properties are enumerable by default, which means that they will appear in a for-in loop or be retrieved by Object.keys().

There are two types of properties: data properties and accessor properties.

Data properties are placeholders for values, and you can read from and write to them.

When a data property holds a function value, the property is considered a method of the object.

Unlike data properties, accessor properties don’t store values on their own; they use a combination of getters and setters to perform specific actions.

You can create both data properties and accessor properties directly using object literal notation.


All properties have several associated attributes.

These attributes define how the properties work.

Both data and accessor properties have [[Enumerable]] and [[Configurable]] attributes. 

**Data properties also have [[Writable]] and [[Value]] attributes, while accessor properties have [[Get]] and [[Set]] attributes.

**By default, [[Enumerable]] and [[Configurable]] are set to true for all properties, and [[Writable]] is set to true for data properties**.

You can change these attributes by using Object.defineProperty() or Object.defineProperties().

**It’s also possible to retrieve these attributes by using Object.getOwnPropertyDescriptor().**


When you want to lock down an object’s properties in some way, there are three different ways to do so.

**If you use Object.preventExtensions(), objects will no longer allow properties to be added**.

**You could also create a sealed object with the Object.seal() method, which makes that object non-extensible and makes its properties nonconfigurable**.

**The Object.freeze() method creates a frozen object, which is a sealed object with nonwritable data properties**.

Be careful with nonextensible objects, and always use strict mode so that attempts to access the objects incorrectly will throw an error.

