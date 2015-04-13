## Introduction

In this two-part tutorial, we're going to take a look at JavaScript objects, prototype programming, and building our very first JavaScript component that we can reuse and extend in various parts of our application. Let's get started!

## About Objects & JavaScript

Think of an object as a collection of *things*. For example, imagine you have a bicycle. That bike is you object, and has a collection of *things* called **properties**. An example of a property would be the bike's model, the year is was made, and its components. Its components might then have its own set of properties, like the brakes, seat, derailleur, wheels, etc. These are all properties of the "bike" object. Pretty straightforward, yes?

JavaScript by nature is an object-based language, where everything has its own set of properties that we can access. Here's a more formal definition from the MDN:

> JavaScript is designed on a simple object-based paradigm. An object is a collection of properties, and a property is an association between a name and a value. A property's value can be a function, in which case the property is known as a method. In addition to objects that are predefined in the browser, you can define your own objects.

That's neat...not only are we provided a set of predefined objects in the browser, but we can create our own. In JavaScript, we access an object's properties via the dot notation. But first, let's take a quick look at the creation and instantiation of an object.

## Creation Of An Object

You've probably created or used objects in JavaScript without even knowing it. That's because almost everything in JavaScript is an object. Here's a note from the MDN:

> All primitive types except `null` and `undefined` are treated as objects. They can be assigned properties (assigned properties of some types are not persistent), and they have all characteristics of objects.

While there are a vast number of predefined objects at our disposal, I won't be going through any of that. From this point on, and throughout the rest of this tutorial, we're going to build a very simple JavaScript component called SimpleAlert. With that in mind, let's create our own object.

```javascript
var simpleAlert = new Object();
```

And that's it! Pretty easy, huh? This is clearly useless though, unless we add some properties to it. We can do this by using the dot notation. Let's add some.

```javascript
// create new object
var simpleAlert = new Object();

// add some properties
simpleAlert.sa_default = "Hello World!";
simpleAlert.sa_error = "Error...";
simpleAlert.sa_success = "Success!";

// output object in the console	
console.log(simpleAlert);
```

From the console, we can see that our object now has three properties. Since we set these properties above, we can now access them anywhere in our script. For example, if we wanted to send an error alert to the user, we could do this:

```javascript
// alert error message
alert(simpleAlert.sa_error);
```

This is just one way to create an object and access its properties.

## Creating Objects With Object Initializers

Another way is using what's called "object initialisers". According to the MDN:

> ...you can create objects using an object initializer. Using object initializers is sometimes referred to as creating objects with literal notation. "Object initializer" is consistent with the terminology used by C++.

Restructuring the creation of our SimpleAlert object would be as easy as this:

```javascript
// create new object
var simpleAlert = {
  sa_default    : "Hello World!",
  sa_error      : "Error...",
  sa_success    : "Success!"
}

// output object in the console
console.log(simpleAlert);
```

Property values can also be functions. For example:

```javascript
// create new object
var simpleAlert = {
  sa_default    : "Hello World!",
  sa_error      : "Error...",
  sa_success    : "Success!",
  sa_fallback   : function(){
    console.log("Fallback");
  }
}

// run the fallback
simpleAlert.sa_fallback();
```

The above should output "Fallback" to the console. When a property's value is a function, it is known as a **method**.

## Using A Constructor Function

Another great way to create objects in JavaScript is to use a "constructor function". From the MDN:

> Define the object type by writing a constructor function. There is a strong convention, with good reason, to use a capital initial letter. Create an instance of the object with `new`.

You can read more about it [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_Objects#Using_a_constructor_function). Using the constructor function, we can rewrite our object creation like this:

```javascript
// constructor function
function SimpleAlert( sa_default, sa_error, sa_success ) {
  this.sa_default = sa_default;
  this.sa_error = sa_error;
  this.sa_success = sa_success;
}

// creation of new object
var my_alert = new SimpleAlert( "Hello World!", "Error...", "Success!" );
```

Now if we output to the console, we can see our entire object. We can also now get the properties of our object via dot notation.

```javascript
console.log(my_alert); // outputs object
console.log(my_alert.sa_error); // outputs "Error..."
```

The beauty of this technique is that you can now create multiple instances of the object, and pass in different values (which can be later accessed as properties). Properties can also be objects and even functions.

## A Whole Lot More About Objects

In JavaScript, there's a whole lot more to objects than I mentioned above, although the methods mentioned above are a great start. The "initialiser" and "constructor function" methods in particular are useful, and we'll use these two methods below to create our very first SimpleAlert JavaScript component. I highly recommend reading through the MDN document [Working With Objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_Objects) to boost your knowledge even more. Also, take a read through the [Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object) document to see what properties/methods are readily available for us.

## Building Blocks For Our Component

We're going to make a component called SimpleAlert, that posts an alert (or message) to the user's screen when they press a button. The message that gets displayed will Depend on which button the user presses. Here's a look at the HTML:

```html
<div id="component">
  <button id="default">Show Default Message</button>
  <button id="success">Show Success Message</button>
  <button id="error">Show Error Message</button>
</div>
```

For our component, we'll wrap all our code in a self executing function and then make it available in the global namespace. We'll start off like this:

```javascript
;(function( window ) {

  'use strict';
  
})( window );
```

There are a couple points to note here:

1. I'm using strict mode. You can read more about strict mode [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope/Strict_mode).
2. We are passing `window` into our self-executing function so that we can later add our SimpleAlert component to the global namespace.

Let's build a bit on our component. We want to first create our function `SimpleAlert`.  We also want to add it to the global namespace. Here's how we'll do this:

```javascript
;(function( window ) {

  'use strict';

  /**
   * SimpleAlert function
   */
  function SimpleAlert( message ) {
    this.message = message;
  }
  
  // rest of code here...
  
  /**
   * Add SimpleAlert to global namespace
   */
  window.SimpleAlert = SimpleAlert;
  
})( window );
```

So far, if we create a new instance of our object, nothing happens. It just exists, and that's pretty sad. For example, if we do this:

```javascript
(function() {
  /**
   * Show default
   */
  var default_btn = document.getElementById( "default" );
  default_btn.addEventListener( "click", function() {
    var default_alert = new SimpleAlert("Hello World!");
    default_alert;
  } );

})();
```

Before I continue, I need to introduce to you briefly `Object.prototype`. This represents the Object prototype object. From the MDN:

> All objects in JavaScript are descended from `Object`; all objects inherit methods and properties from `Object.prototype`, although they may be overridden (except an `Object` with a `null` prototype, i.e. `Object.create(null)`).

For more reading, go [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/prototype). We'll be utilising this in our component creation. Let's build onto our component a bit. Let's add an `init` function, and call that function as soon as our object is created. Inside this `init` function, let's send our message to the console. Now, our JavaScript looks like this:

```javascript
;(function( window ) {

  'use strict';

  /**
   * SimpleAlert function
   */
  function SimpleAlert( message ) {
    this.message = message;
    this._init();
  }

  /**
   * Initialise the message
   */
  SimpleAlert.prototype._init = function() {
    console.log(this.message);
  }

  /**
   * Add SimpleAlert to global namespace
   */
  window.SimpleAlert = SimpleAlert;

})( window );
```

And now, our new instance of our object created above should output "Hello World!" to the console. We're getting somewhere! Let's actually show the notice to the user on screen, and not in the console. Our `_init` function may now look like this:

```javascript
;(function( window ) {

  'use strict';

  /**
   * SimpleAlert function
   */
  function SimpleAlert( message ) {
    this.message = message;
    this._init();
  }

  /**
   * Initialise the message
   */
  SimpleAlert.prototype._init = function() {
    this.component = document.getElementById("component");
    this.box = document.createElement("div");
    this.box.className = "simple-alert";
    this.box.innerHTML = this.message;
    this.component.appendChild( this.box );
  }

  /**
   * Add SimpleAlert to global namespace
   */
  window.SimpleAlert = SimpleAlert;

})( window );
```

Let's go through the `_init` function step by step.

1. Inside our constructor function, we are making the variable `message` accessible throughout by writing `this.message = message`. 
2. We then call our `_init` function by writing `this._init()`. Every time we create a new instance of our SimpleAlert object, these two steps automatically run, so we automatically go to `_init()`. 
3. Inside `_init()`, we are assigning our variables by using the `this` keyword and a variable name of our choice. We fetch the `#component` dive, and create our new `div` element where we would have our new SimpleAlert show up. Then we append the message to this new `div` that we created.

Pretty neat, huh?

## The Code In Full

Here is the code in full, including some CSS to tie it all together neatly. Firstly, the HTML:

```html
<div id="component">
  <button id="default">Show Default Message</button>
  <button id="success">Show Success Message</button>
  <button id="error">Show Error Message</button>
</div>
```

Same as we've seen above. Here's some CSS to make our alerts look a bit nice on screen:

```css
.simple-alert {
  padding: 20px;
  border: solid 1px #ebebeb;
}
```

Simple. Edit at your will, of course! And finally, here's the JavaScript that covers our component, and the actual event listeners on all three buttons triggering new instances of our object and hence displaying alerts to the screen:

```javascript
// COMPONENT
//
// The building blocks for our SimpleAlert component.
////////////////////////////////////////////////////////////
;(function( window ) {

  'use strict';

  /**
   * SimpleAlert function
   */
  function SimpleAlert( message ) {
    this.message = message;
    this._init();
  }

  /**
   * Initialise the message
   */
  SimpleAlert.prototype._init = function() {
    this.component = document.getElementById("component");
    this.box = document.createElement("div");
    this.box.className = "simple-alert";
    this.box.innerHTML = this.message;
    this.component.appendChild( this.box );
    this._initUIActions;
  }

  SimpleAlert.prototype._initUIActions = function() {

  }

  /**
   * Add SimpleAlert to global namespace
   */
  window.SimpleAlert = SimpleAlert;

})( window );


// EVENTS
//
// The code for the creation of new object instances,
// depending on which button is pressed.
////////////////////////////////////////////////////////////
;(function() {
  
  /**
   * Show default
   */
  var default_btn = document.getElementById( "default" );
  default_btn.addEventListener( "click", function() {
    var default_alert = new SimpleAlert("Hello World!");
    default_alert;
  } );

  /**
   * Show success
   */
  var default_btn = document.getElementById( "success" );
  default_btn.addEventListener( "click", function() {
    var default_alert = new SimpleAlert("Success!!");
    default_alert;
  } );

  /**
   * Show error
   */
  var default_btn = document.getElementById( "error" );
  default_btn.addEventListener( "click", function() {
    var default_alert = new SimpleAlert("Error...");
    default_alert;
  } );

})();
```

## Wrap Up

Here, we looked into JavaScript objects and we've seen how to create a very basic JavaScript component using objects. This demonstration is simple, but provides us with some fundamental knowledge for creating reusable and extendable components. Think of how you might add a "dismiss" button to the alert, triggering a new function `hide()` when pressed.

Next time, we expand a bit more on our SimpleAlert component, and we learn how to set default options and pass in our own set of options. This is where we will see the beauty of this type of programming coming into play. I hope you enjoyed this tutorial, and if you have any questions, comments or feedback, leave it below. Stay tuned for part 2, and thanks for reading!