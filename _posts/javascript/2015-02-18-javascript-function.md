---
layout: post
title: "javascript function"
description: "javascript function"
category: [javascript,function]
tags: [javascript]
---
{% include JB/setup %}

## Function

The defining characteristic of a function — what distinguishes it from any other object — is the presence of **an internal property named [[Call]]**.

Internal properties are not accessible via code but rather define the behavior of code as it executes.

ECMAScript defines multiple internal properties for objects in JavaScript, and these internal properties are indicated by **double-square-bracket notation**.

**The [[Call]] property is unique to functions and indicates that the object can be executed**.

### Declarations vs. Expressions

two literal forms of functions.

1.**Function declaration**, which begins with the function keyword and includes the name of the function immediately following it.

` function add(num1, num2) {`

`            return num1 + num2;`

`}`

2.**Function expression**, which doesn’t require a name after function.

 These functions are considered **anonymous** because the function object itself has no name.

 Instead, function expressions are typically **referenced via a variable or property**, as in this expression:

`var add = function(num1, num2) {`

`		return num1 + num2;`

`};`

This code actually assigns a function value to the variable add. 

The function expression is almost identical to the function declaration except for **the missing name and the semicolon at the end**.

Assignment expressions typically end with a semicolon, just as if you were assigning any other value.


Function declarations **are hoisted to the top of the context (either the function in which the declaration occurs or the global scope) when the code is executed.**

`var result = add(5, 5);`

`        function add(num1, num2) {`

`            return num1 + num2;`

`}`

That’s because the JavaScript **engine hoists the function declaration to the top and actually executes the code** as if it were written like this:

` // how the JavaScript engine interprets the code`

`        function add(num1, num2) {`

`            return num1 + num2;`

`        }`

`        var result = add(5, 5);`

**Function hoisting happens only for function declarations because the function name is known ahead of time.**

Function expressions, on the other hand, cannot be hoisted because the functions can be referenced only through a variable.

So this code causes an error:

`// error!`
`       var result = add(5, 5);`

`        var add = function(num1, num2) {`

`            return num1 + num2;`

`};`


###Functions as Values

Because JavaScript has first-class functions, you can use them just **as you do any other objects**.

You can assign them to variables, add them to objects, pass them to other functions as arguments, and return them from functions.


The sort() method on JavaScript arrays accepts a comparison function as an optional parameter.

The comparison function is called whenever two values in the array must be compared.

If the first value is smaller than the second, the comparison function must return a negative number.

If the first value is larger than the second, the function must return a positive number.

If the two values are equal, the function should return zero.

By default, sort() converts every item in an array to a string and then performs a comparison.

That means you can’t accurately sort an array of numbers without specifying a comparison function.


`var numbers = [ 1, 5, 8, 4, 7, 10, 2, 6 ];`

`numbers.sort(function(first, second) {`

`       return first - second;`

`   });`

`console.log(numbers); // "[1, 2, 4, 5, 6, 7, 8, 10]"`

`numbers.sort();`

`console.log(numbers);       // "[1, 10, 2, 4, 5, 6, 7, 8]"`

###Parameters

Another unique aspect of JavaScript functions is that you can **pass any number of parameters to any function without causing an error**.

That’s because function parameters are actually **stored as an array-like structure called arguments**.

Just like a regular JavaScript array, arguments can grow to contain any number of values.

The values are referenced via numeric indices, and there is a length property to determine how many values are present.


**The arguments object is automatically available inside any function**.

This means named parameters in a function exist mostly for convenience and don’t actually limit the number of arguments that a function can accept.


The first implementation of reflect() is much easier to understand because it uses a named argument (as you would in other languages).

The version that uses the arguments object can be confusing because there are no named arguments, and you must read the body of the function to determine if arguments are used.

That is why many developers prefer to avoid using arguments unless necessary.


### Overloading

As mentioned previously, JavaScript functions can accept any number of parameters, and the types of parameters a function takes aren’t specified at all.

That means JavaScript functions don’t actually have signatures.

**A lack of function signatures also means a lack of function overloading.**


You can retrieve the number of parameters that were passed in by using the arguments object, and you can use that information to determine what to do.

`function sayMessage(message) {`

`    if (arguments.length === 0) {`

`        message = "Default message";`

`}`

`    console.log(message);`

`}`

`sayMessage("Hello!");       // outputs "Hello!"`

 This is a little more involved than function overloading in other languages, but the end result is the same. If you really want to check for different data types, you can use typeof and instanceof.

 In practice, checking the named parameter against undefined is more common than relying on arguments.length.

 ### Object Methods

 When a property value is actually a function, the property is considered a method.

`var person = {`

`            name: "Nicholas",`

`            sayName: function() {`

`                console.log(person.name);`

`            }`

`        };`

`person.sayName();       // outputs "Nicholas"`


#### The this Object

You may have noticed something strange in the previous example. The sayName() method references person.name directly, which creates tight coupling between the method and the object.

This is problematic for a number of reasons. 

First, if you change the variable name, you also need to remember to change the reference to that name in the method.

Second, this sort of tight coupling makes it difficult to use the same function for different objects. Fortunately, JavaScript has a way around this issue.


**Every scope in JavaScript has a this object that represents the calling object for the function.**

In the global scope, this represents the **global object (window in web browsers)**.

**When a function is called while attached to an object, the value of this is equal to that object by default.**

So, instead of directly referencing an object inside a method, you can reference this instead.


`var person = {`

`    name: "Nicholas",`

`    sayName: function() {`

`        console.log(this.name);`

`    }`

`};`

`person.sayName();       // outputs "Nicholas"`


`function sayNameForAll() {`

`    console.log(this.name);`

`}`

`var person1 = {`

`    name: "Nicholas",`

`    sayName: sayNameForAll`

`};`

`var person2 = {`

`    name: "Greg",`

`    sayName: sayNameForAll`

`};`

`var name = "Michael";`

`person1.sayName(); // outputs "Nicholas"`

`person2.sayName(); // outputs "Greg"`

`sayNameForAll(); // outputs "Michael"` 


### ***Changing this***

The ability to use and manipulate the this value of functions is key to good object-oriented programming in JavaScript.

Functions can be used in many different contexts, and they need to be able to work in each situation.

**Even though this is typically assigned automatically, you can change its value to achieve different goals.**

**There are three function methods that allow you to change the value of this. (Remember that functions are objects, and objects can have methods, so functions can, too.)**

#### The call() Method

The first function method for manipulating this is call(), which executes the function with a particular this value and with specific parameters.

**The first parameter of call() is the value to which this should be equal when the function is executed.**

All subsequent parameters are the parameters that should be passed into the function.

`function sayNameForAll(label) {`

`    console.log(label + ":" + this.name);`

`}`

`var person1 = {`

`    name: "Nicholas"`

`};`

`var person2 = {`

`    name: "Greg"`

`};`

`var name = "Michael";`

`sayNameForAll.call(this, "global"); // outputs "global:Michael"`

`sayNameForAll.call(person1, "person1"); // outputs "person1:Nicholas"`

`sayNameForAll.call(person2, "person2"); // outputs "person2:Greg"`


#### **The apply() Method**

The second function method you can use to manipulate this is apply().

The apply() method works exactly the same as call() except that it accepts only two parameters: **the value for this** and **an array or array-like object of parameters to pass to the function (that means you can use an arguments object as the second parameter)**.

So, instead of individually naming each parameter using call(), you can easily pass arrays to apply() as the second argument.

Otherwise, call() and apply() behave identically.


`function sayNameForAll(label) {`

`    console.log(label + ":" + this.name);`

`}`

`var person1 = {`

`    name: "Nicholas"`

`};`

`var person2 = {`

`    name: "Greg"`

`};`

`var name = "Michael";`

`sayNameForAll.apply(this, ["global"]); // outputs "person1:Michael"`

`sayNameForAll.apply(person1, ["person1"]);  // outputs "person1:Nicholas"`

`sayNameForAll.apply(person2, ["person2"]);  // outputs "person2:Greg"`


The method you use typically depends on **the type of data you have**.

**If you already have an array of data, use apply(); if you just have individual variables, use call()**.


#### **The bind() Method**

The third function method for changing this is bind().

This method was added in ECMAScript 5, and it behaves quite differently than the other two.

The first argument to bind() is the this value for the new function.

All other arguments represent named parameters that should be permanently set in the new function.

You can still pass in any parameters that aren’t permanently set later.

The following code shows two examples that use bind().

You create the sayNameForPerson1() function by binding the this value to person1, while sayNameForPerson2() binds this to person2 and binds the first parameter as "person2".



`function sayNameForAll(label) {`

`       console.log(label + ":" + this.name);`

`}`

`var person1 = {`

`       name: "Nicholas"`

`};`

`var person2 = {`

`       name: "Greg"`

`};`

// create a function just for person1

`var sayNameForPerson1 = sayNameForAll.bind(person1);`

`sayNameForPerson1("person1"); // outputs "person1:Nicholas"`

// create a function just for person2

`var sayNameForPerson2 = sayNameForAll.bind(person2, "person2");`

`sayNameForPerson2(); // outputs "person2:Greg"`

// attaching a method to an object doesn't change 'this'

`person2.sayName = sayNameForPerson1;`

`person2.sayName("person2"); // outputs "person2:Nicholas"`



## Summary
JavaScript functions are unique in that they are also objects, meaning they can be accessed, copied, overwritten, and generally treated just like any other object value.

The biggest difference between a JavaScript function and other objects is a special internal property, [[Call]], which contains the execution instructions for the function.

**The typeof operator looks for this internal property on an object, and if it finds it, returns "function"**.

**There are two function literal forms: declarations and expressions**.

Function declarations contain the function name to the right of the function keyword and are hoisted to the top of the context in which they are defined.

Function expressions are used where other values can also be used, such as assignment expressions, function parameters, or the return value of another function.

Because functions are objects, there is a Function constructor.

You can create new functions with the Function constructor, but this isn’t generally recommended because it can make your code harder to understand and debugging much more difficult.

That said, you will likely run into its usage from time to time in situations where the true form of the function isn’t known until runtime.

You need a good grasp of functions to understand how object- oriented programming works in JavaScript.

Because JavaScript has no concept of a class, functions and other objects are all you have to work with to achieve aggregation and inheritance.
