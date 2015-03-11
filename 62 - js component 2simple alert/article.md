<p class="text-align--center">
<a href="http://callmenick.com/tutorial-demos/simple-alert/simple-alert-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/tutorial-demos/simple-alert" class="button button--inline-block button--medium">View Demo</a>
</p>

## Introduction

In [part one](http://callmenick.com/post/javascript-objects-building-javascript-component-part-1/) of this two-part tutorial, we went into some detail about JavaScript objects, and created a very basic component. Our component, however, only took on one parameter, and isn't easily extendable. In part two (this tutorial), we're going to look into some interesting functions, and give our component some options. These options can be specified each time we create a new instance of our component, giving us more control over the output.

## Simple Alert Outline

The outline for our component will be as follows:

1. We want the user to wire up a Simple Alert to whatever element he/she desires. For this tutorial, I'll be wiring up Simple Alerts to buttons.
2. We want the user to specify what type of alert will be shown. The user will have 4 options - error, success, warning, and default.
3. We want to give different styles to each of these alerts, so that the user experience is a bit more in line with what users are accustomed to (i.e. red is error, green is success, etc).
4. We want users to be able to dismiss the alerts. This functionality will be written inside our Simple Alert component.

So off the bat, we know we need some HTML and CSS structure. Because the JavaScript is actually creating the HTML for us on each instance of the alert, I like to style it in some test markup before so I know how it would look. With that in mind, let's take a look at what the markup should be like.

## Simple Alert Test Markup

In general, our alerts would look something like this after the JS outputs it:

```html
<div class="simple-alert TYPE">
  <span class="simple-alert__content">Message here</span>
  <a href="#" class="simple-alert__dismiss"></a>
</div>
```

"TYPE" is the type of alert, and this class, as mentioned above, will be represented by either of 4 types:

* `.error`
* `.success`
* `.warning`
* `.default`

Knowing all this, we now have some classes to work with. So let's dig into the CSS.

## The CSS For Our JS Object Output

As seen in the markup, we have some classes to work with that will give our alerts some style. I mentioned before that each of our four types of alerts will have different colour schemes to invoke some familiarity to users, so let's style with that in mind. Here's the CSS I came up with:

```css
.simple-alert {
  display: inline-block;
  position: relative;
  margin: 0 5px 5px 0;
  padding: 5px 45px 5px 5px;
}

.simple-alert.error {
  background-color: #f06464;
  border: solid 1px #d91515;
}

.simple-alert.success {
  background-color: lightgreen;
  border: solid 1px #38e038;
}

.simple-alert.warning {
  background-color: moccasin;
  border: solid 1px #ffbf4f;
}

.simple-alert.default {
  background-color: lightblue;
  border: solid 1px #5fb3ce;
}

.simple-alert__content {
  color: rgba(0, 0, 0, 0.5);
}

.simple-alert__dismiss {
  display: block;
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  margin: auto 0;
  width: 20px;
  height: 20px;
  background-image: url("../img/close.png");
  font-size: 0;
  text-indent: -9999px;
}
```

These styles should now give us a nice aesthetic that's pleasing to the user and representative of the various message types. Now, without further ado, let's get into some JavaScript!

## A Quick Re-Introduction

As before, we'll wrap our component in a self-executing function. Note the ";" before our function, which is a precaution in case of auto-concatenated JavaScript files. Also note that we pass in `window` in order to add our Simple Alert component to the global namespace. Here's our function wrapper:

```javascript
;( function( window ) {

  'use strict';
  
})( window );
```

If you remember part 1, you'll remember how we started out our component. If not, it was as easy as defining `SimpleAlert` as a new function, and adding it to the global namespace. Here's how our JS should look now:

```javascript
;( function( window ) {

  'use strict';
  
  /**
   * SimpleAlert
   */
  function SimpleAlert( options ) {
    // function body...
  }
  
  /**
   * Add to global namespace
   */
  window.SimpleAlert = SimpleAlert;
  
})( window );
```

## Component Building & Extending

Now, let's get our feet into some new stuff. As I mentioned before, we want to be able to pass in some options to our new instances of our object. In the demo, three options are available:

1. wrapper - the wrapper to append alerts to
2. type - the type of alert
3. message - the alert message

So we want to be able to accept an options object that looks like this:

```javascript
options = {
  wrapper : document.body,
  type : "default",
  message : "Default message."
}
```

Note that I've set some default options above. This is interesting though, because we actually want default options inside our object in case the user decides to keep some of the defaults and only set one or two options. This brings us to another issue, however. How do we see if the user has passed in no options, some options, or all options, and override them where necessary? For this, we have an "extend object" function. It looks like this:

```javascript
function extend( a, b ) {
  for( var key in b ) { 
    if( b.hasOwnProperty( key ) ) {
      a[key] = b[key];
    }
  }
  return a;
}
```

Don't get all confused! This function is simple, and performs three main things:

1. Firstly, it accepts two objects as parameters.
2. Secondly, it loops through the second object, `b`, and looks at each `key` in it. It checks to see if `b` has that property by calling on the method `hasOwnProperty`, and you can view documentation on that method [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty).
3. Finally, it updates and sets the corresponding `key` in the other object, `a`, and then when the loop is complete, it returns object `a`.

This is all well and good, but what exactly do we pass into this `extend` function for our options object to be ready for use in our component? We want to follow a couple steps to get there. So here's what we need to do:

1. We want `this.options` accessible in our entire component, so we first update it to be equal to our defaults. To do this, we pass in an empty object as `a` and the default options (initially represented by `this.options`) as `b` in our `extend` function.
2. We then extend `this.options` with our passed in object `options`, overriding the defaults where specified.

Our component, from start to finish, now looks something like this:

```javascript
;( function( window ) {

  'use strict';

  /**
   * Extend obj function
   *
   * This is an object extender function. It allows us to extend an object
   * by passing in additional variables and overwriting the defaults.
   */
  function extend( a, b ) {
    for( var key in b ) { 
      if( b.hasOwnProperty( key ) ) {
        a[key] = b[key];
      }
    }
    return a;
  }

  /**
   * SimpleAlert
   *
   * @param {object} options - The options object
   */
  function SimpleAlert( options ) {
    this.options = extend( {}, this.options );
    extend( this.options, options );
    // start the functionality...
  }

  /**
   * SimpleAlert options Object
   *
   * @type {HTMLElement} wrapper - The wrapper to append alerts to.
   * @param {string} type - The type of alert.
   * @param {string} message - The alert message.
   */
  SimpleAlert.prototype.options = {
    wrapper : document.body,
    type : "default",
    message : "Default message."
  }

  /**
   * Add to global namespace
   */
  window.SimpleAlert = SimpleAlert;

})( window );
```

## Adding The Actual Functionality

Now that we're all set up, we need to add some actual functionality. Firstly, after a new instance of the object is instantiated, we want to run some code. I'll separate this first chunk of code into an `_init()` function. The reason for underscoring these functions is just a matter of taste. It doesn't actually do anything special, but lets other developers, or implementers of the component, know that this function should be reserved for private use inside the component. Here's what we want our `_init()` function to do for us:

1. We want to create our actual alert. We do this by creating a `div` element, and adding the correct class names to it.
2. We want to then construct our inner HTML, i.e. the alert message and dismiss-alert button, and give that inner HTML to our newly created `div`.
3. We then want to listen for events on the dismiss button, and have some functions for showing and dismissing the alert

### Initial Function

Our `_init()` function now should look like this:

```javascript
/**
 * SimpleAlert _init
 *
 * This is the initializer function. It builds the HTML and gets the alert
 * ready for showing.
 *
 * @type {HTMLElement} this.sa - The Simple Alert div
 * @param {string} strinner - The inner HTML for our alert
 */
SimpleAlert.prototype._init = function() {
  // create element
  this.sa = document.createElement('div');
  this.sa.className = 'simple-alert ' + this.options.type;

  // create html
  var strinner = '';
  strinner += '<span class="simple-alert__content">';
    strinner += this.options.message;
  strinner += '</span>';
  strinner += '<a href="#" class="simple-alert__dismiss">close</a>';
  this.sa.innerHTML = strinner;

  // run the events
  this._events();
};
```

Note the usage of `this.options.type` and `this.options.message` which are of course, inherited values from our `this.options` object that we set up before. Also note that at the end of this function, we're calling on a new function, `_events()`, which will handle our events.

### Events Function

The `_events()` function takes care of dismissing our alert when a user clicks the dismiss button. It's a simple function, and here's what it looks like:

```javascript
/**
 * SimpleAlert _events
 *
 * This is our events function, and its sole purpose is to listen for
 * any events inside our Simple Alert.
 *
 * @type {HTMLElement} btn_dismiss - The dismiss-alert button
 */
SimpleAlert.prototype._events = function() {
  // cache vars
  var btn_dismiss = this.sa.querySelector('.simple-alert__dismiss'),
      self = this;

  // listen for dismiss
  btn_dismiss.addEventListener( "click", function(e) {
    e.preventDefault();
    self.dismiss();
  });
}
```

Notice that we set `var self = this` at the start of the function. This is because we want to access that instance of `this` inside the click event, but once inside the click event, `this` refers to the node that was clicked. Setting `this` to `self` before hand avoids any confusion. Note the call on the `dismiss()` function from before.

### The Show Function

The `show()` function does at its name says - it shows the alert. This is a very basic function, and just appends our newly created `div` in the `_init()` function to the wrapper passed in to the `options` object. Here's what the function looks like:

```javascript
/**
 * SimpleAlert show
 *
 * This function simply shows our Simple Alert by appending it
 * to the wrapper in question.
 */
SimpleAlert.prototype.show = function() {
  this.options.wrapper.appendChild(this.sa);
}
```

### The Dismiss Function

The `dismiss()` function is equally as basic as the `show()` function, and simply removes the alert from the wrapper. Here's the function:

```javascript
/**
 * SimpleAlert dismiss
 *
 * This function simply hides our Simple Alert by removing it
 * from the wrapper in question.
 */
SimpleAlert.prototype.dismiss = function() {
  this.options.wrapper.removeChild(this.sa);
};
```

### Why Separate Two Simple Functions?

Surely, the functionality of `show()` and `dismiss()` could have all happened inside the `_init()` function, but there's a reason we separate them.

* We want to instantiate our alert, and show the alert whenever we want. In a more complex component, adding a "public method" like `show()` will allow us to show the actual alert whenever we want, after it is created.
* In the case of dismissing the alert, we added an event listener to the dismiss button which is default behaviour. What if an implementer of the component wanted to add his/her own functionality, and allow dismissing the alerts by clicking on a custom button? 
* Finally, by doing this, we can (if we wanted) add two callbacks as part of our options object, and run the callbacks on alert creation, and alert dismissal. We would run these callbacks inside each of the functions `show()` and `dismiss()` respectively.

## The Final Component Code

Here at last, a look at the final component code:

```javascript
/**
 * Simple Alert
 *
 * This little function allows us to display alerts to the user. The alerts
 * append to a wrapper of choice, and takes on two other variables.
 *
 * Licensed under the MIT license.
 * http://www.opensource.org/licenses/mit-license.php
 *
 * Copyright 2014, Call Me Nick
 * http://callmenick.com
 */

;( function( window ) {

  'use strict';

  /**
   * Extend obj function
   *
   * This is an object extender function. It allows us to extend an object
   * by passing in additional variables and overwriting the defaults.
   */
  function extend( a, b ) {
    for( var key in b ) { 
      if( b.hasOwnProperty( key ) ) {
        a[key] = b[key];
      }
    }
    return a;
  }

  /**
   * SimpleAlert
   *
   * @param {object} options - The options object
   */
  function SimpleAlert( options ) {
    this.options = extend( {}, this.options );
    extend( this.options, options );
    this._init();
  }

  /**
   * SimpleAlert options Object
   *
   * @type {HTMLElement} wrapper - The wrapper to append alerts to.
   * @param {string} type - The type of alert.
   * @param {string} message - The alert message.
   */
  SimpleAlert.prototype.options = {
    wrapper : document.body,
    type : "default",
    message : "Default message."
  }

  /**
   * SimpleAlert _init
   *
   * This is the initializer function. It builds the HTML and gets the alert
   * ready for showing.
   *
   * @type {HTMLElement} this.sa - The Simple Alert div
   * @param {string} strinner - The inner HTML for our alert
   */
  SimpleAlert.prototype._init = function() {
    // create element
    this.sa = document.createElement('div');
    this.sa.className = 'simple-alert ' + this.options.type;

    // create html
    var strinner = '';
    strinner += '<span class="simple-alert__content">';
      strinner += this.options.message;
    strinner += '</span>';
    strinner += '<a href="#" class="simple-alert__dismiss">close</a>';
    this.sa.innerHTML = strinner;

    // run the events
    this._events();
  };

  /**
   * SimpleAlert _events
   *
   * This is our events function, and its sole purpose is to listen for
   * any events inside our Simple Alert.
   *
   * @type {HTMLElement} btn_dismiss - The dismiss-alert button
   */
  SimpleAlert.prototype._events = function() {
    // cache vars
    var btn_dismiss = this.sa.querySelector('.simple-alert__dismiss'),
        self = this;

    // listen for dismiss
    btn_dismiss.addEventListener( "click", function(e) {
      e.preventDefault();
      self.dismiss();
    });
  }

  /**
   * SimpleAlert show
   *
   * This function simply shows our Simple Alert by appending it
   * to the wrapper in question.
   */
  SimpleAlert.prototype.show = function() {
    this.options.wrapper.appendChild(this.sa);
  }

  /**
   * SimpleAlert dismiss
   *
   * This function simply hides our Simple Alert by removing it
   * from the wrapper in question.
   */
  SimpleAlert.prototype.dismiss = function() {
    this.options.wrapper.removeChild(this.sa);
  };

  /**
   * Add to global namespace
   */
  window.SimpleAlert = SimpleAlert;

})( window );
```

## An Example Of Execution

Now that our component is complete, let's take a look at an example execution.

```html
<button id="simple-alert__button--error">Show Me An Error Alert</button>
<section class="demo-section" id="alerts"></section>

<script>
  ( function() {
    // cache vars
    var btn_error = document.getElementById("simple-alert__button--error");

    // show error
    btn_error.addEventListener( "click", function(e) {
      e.preventDefault();
      var sa = new SimpleAlert({
        wrapper : document.getElementById("alerts"),
        type : "error",
        message : "Show me an error!"
      });
      sa.show();
    });

  })();
</script>
```

Simple and easy.

## Wrap Up

And that’s a wrap! In this tutorial, we dove deep into the creation of a JavaScript component, object extending, instantiating, and executing. The results may be simple, but it is a great insight into the wonderful world of JavaScript OOP, and a great starting point to building your own components. Don’t forget, you can view the demo and download the source by clicking the links below, and if you have and questions, comments, or feedback, you can also leave them below. Also, you can [check out this project on GitHub](https://github.com/callmenick/Simple-Alert) and fiddle as you see fit.

<p class="text-align--center">
<a href="http://callmenick.com/tutorial-demos/simple-alert/simple-alert-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/tutorial-demos/simple-alert" class="button button--inline-block button--medium">View Demo</a>
</p>