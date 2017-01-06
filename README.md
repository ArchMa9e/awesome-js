# awesome-js
🦄 A curated list of javascript fundamentals for preparing interviews.

##Table of Contents
- [Fundamentals](#fundamentals)
    - [1.1 - typeof](#1.1)
    - [1.2 - scope](#1.2)
    - [1.3 - error handling](#1.3)
- Algorithms
- Answers
- Credits

##Fundamentals
####1.1 typeof <a name='1.1'/>
<a name='1.1.1'/>
1. What is a potential pitfall with using 
```javascript
typeof bar === "object" 
```
to determine if bar is an object? How can this pitfall be avoided?

[See Answer](#a1.1.1)


####1.2 scope <a name='1.2'/>
<a name='1.2.1'/>
1. What will the code below output to the console and why?
```javascript
var myObject = {
    foo: "bar",
    func: function() {
        var self = this;
        console.log("outer func:  this.foo = " + this.foo);
        console.log("outer func:  self.foo = " + self.foo);
        (function() {
            console.log("inner func:  this.foo = " + this.foo);
            console.log("inner func:  self.foo = " + self.foo);
        }());
    }
};
myObject.func();
```

[See Answer](#a1.2.1)



<a name='1.2.2'/>

2. What is the significance of, and reason for, wrapping the entire content of a JavaScript source file in a function block?

[See Answer](#a1.2.2)



#### Error Handling <a name='1.3'/>

<a name='1.3.1'/>

1. What is the significance, and what are the benefits, of including `'use strict'` at the beginning of a JavaScript source file?

[See Answer](#a1.3.1)




##Answers

####1.1 typeof
<a name='a1.1.1'/>
1. In Javascript, ```null``` is also considered an object. Therefore, the code below surprises new developers.

```javascript
var bar = null;
console.log(typeof bar === "object"); //logs true
```

As long as one is aware of this, the problem can easily be avoided by also checking if bar is null:

````javascript
console.log((bar !== null) && (typeof bar === "object"));  // logs false
````

To be entirely thorough in our answer, there are two other things worth noting:

First, the above solution will return false if bar is a function. In most cases, this is the desired behavior, but in situations where you want to also return true for functions, you could amend the above solution to be:

```javascript
console.log((bar !== null) && ((typeof bar === "object") || (typeof bar === "function")));
```

Second, the above solution will return true if bar is an array (e.g., if var bar = [];). In most cases, this is the desired behavior, since arrays are indeed objects, but in situations where you want to also false for arrays, you could amend the above solution to be:

```javascript
console.log((bar !== null) && (typeof bar === "object") && (toString.call(bar) !== "[object Array]"));
```
[Back to Question](#1.1.1)



#### 1.2 scope

<a name='a1.2.1'/>

1. The above code will output the following to the console:

```javascript
outer func:  this.foo = bar
outer func:  self.foo = bar
inner func:  this.foo = undefined
inner func:  self.foo = bar
```

In the outer function, both `this` and `self` refer to `myObject` and therefore both can properly reference and access `foo`.

In the inner function, though, `this` no longer refers to `myObject`. As a result, `this.foo` is undefined in the inner function, whereas the reference to the local variable `self` remains in scope and is accessible there.

[Back to Question](#1.2.1)



<a name='a1.2.2'/>

2. This is an increasingly common practice, employed by many popular JavaScript libraries (jQuery, Node.js, etc.). This technique creates a closure around the entire contents of the file which, perhaps most importantly, creates a private namespace and thereby helps avoid potential name clashes between different JavaScript modules and libraries.

   Another feature of this technique is to allow for an easily referenceable (presumably shorter) alias for a global variable. This is often used, for example, in jQuery plugins. jQuery allows you to disable the `$` reference to the jQuery namespace, using `jQuery.noConflict()`. If this has been done, your code can still use `$` employing this closure technique, as follows:

   ```javascript
   (function($) { /* jQuery plugin code referencing $ */ } )(jQuery);
   ```

[Back to Question](#1.2.2)



#### 1.3 Error Handling

<a name='a1.3.1'/>

1. the short and most important answer here is that `use strict` is a way to voluntarily enforce stricter parsing and error handling on your JavaScript code at runtime. Code errors that would otherwise have been ignored or would have failed silently will now generate errors or throw exceptions. In general, it is a good practice.

   Some of the key benefits of strict mode include:

   - **Makes debugging easier.** Code errors that would otherwise have been ignored or would have failed silently will now generate errors or throw exceptions, alerting you sooner to problems in your code and directing you more quickly to their source.
   - **Prevents accidental globals.** Without strict mode, assigning a value to an undeclared variable automatically creates a global variable with that name. This is one of the most common errors in JavaScript. In strict mode, attempting to do so throws an error.
   - **Eliminates this coercion**. Without strict mode, a reference to a `this` value of null or undefined is automatically coerced to the global. This can cause many headfakes and pull-out-your-hair kind of bugs. In strict mode, referencing a a `this` value of null or undefined throws an error.
   - **Disallows duplicate property names or parameter values.** Strict mode throws an error when it detects a duplicate named property in an object (e.g., `var object = {foo: "bar", foo: "baz"};`) or a duplicate named argument for a function (e.g., `function foo(val1, val2, val1){}`), thereby catching what is almost certainly a bug in your code that you might otherwise have wasted lots of time tracking down.
   - **Makes eval() safer.** There are some differences in the way `eval()` behaves in strict mode and in non-strict mode. Most significantly, in strict mode, variables and functions declared inside of an `eval()` statement are *not* created in the containing scope (they *are* created in the containing scope in non-strict mode, which can also be a common source of problems).
   - **Throws error on invalid usage of delete.** The `delete` operator (used to remove properties from objects) cannot be used on non-configurable properties of the object. Non-strict code will fail silently when an attempt is made to delete a non-configurable property, whereas strict mode will throw an error in such a case.

[Back to Question](#1.3.1)



