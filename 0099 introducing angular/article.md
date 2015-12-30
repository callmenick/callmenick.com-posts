<p class="text-align--center">
<a href="https://github.com/callmenick/angular-js-introduction" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/angular/intro-to-angular/" class="button button--inline-block button--medium">View Demo</a>
</p>

## Introducing AngularJS

[AngularJS](https://angularjs.org/) is a beast of a JavaScript framework, and it comes with a huge toolkit that helps us build powerful, maintainable, and structured applications. It takes a lot of pain away when it comes to developing dynamic applications due to its beautifully declarative syntax, ease of view manipulation, and two way data binding. In this tutorial, I'm going to plunge into some of the fundamentals of Angular and demonstrate just how easy and powerful it is.

Before we dive in though, let's introduce some important terminology. Above, I mentioned "two way data binding", but what does that even mean? Data binding, in general, means that a connection between some model (think business logic) and the view (think UI) exists, and changes in one will affect the other. In Angular, data is bound both ways, meaning that if the model updates, the view will automatically update to reflect those changes and vice versa. This makes DOM manipulation really easy for us, and is one of the key attractions when using Angular.

Angular also leverages controllers, which act as links to the views. Because of Angular's two way data binding, controllers are very easy to keep organised and maintained, ultimately leading to clean and readable code, receiving input from the DOM, and sending back data as necessary. Data binding also eliminates a lot of the usual JavaScript code that comes with DOM manipulation, further adding to the cleanliness and modularity of controllers.

The final terminology tidbit that I'll introduce is called "directives", which is a great feature available in Angular. Directives let you structure reusable components using your own naming conventions. We won't be going over how to create our own directives in this tutorial. However, I'm bringing them up here because Angular comes with a set of built-in directives already, and we're going to see a few. They are usually prefixed with `ng`, and are highly useful.

Without further ado, let's get to the good stuff.

## Bootstrapping The App

To bootstrap any Angular app, we need to call upon the `ng-app` directive.  [This directive](https://docs.angularjs.org/api/ng/directive/ngApp) can be attached to any HTML element, and is used to auto-bootstrap an Angular application. The `ng-app` directive represents the root of the Angular app, and only one Angular app can be bootstrapped per HTML page. As such, it's common to see it attached to the root `html` element like this:

```html
<html ng-app="app">
</html>
```

Notice how we gave it a name of `app` as well. We'll see how that comes into play when we write our first bit of JavaScript. Before continuing though, we'll need to actually include Angular in our app, as well as our own custom `app.js` file where all our JavaScript will live. I've used [Bower](http://bower.io/) to get Angular in project directory, and I have it configured to save Bower components to the `/lib` directory. I've also saved my `app.js` file in the `/app` directory, so now our `index.html` file looks like this:

```html
<!DOCTYPE html>
<html lang="en" ng-app="app">
<head>
  <script src="lib/angular/angular.min.js"></script>
</head>
<body>

<script src="app/app.js"></script>

</body>
</html>
```

From here on, we'll be working inside the `app.js` file. In order to let Angular know that we're ready to call upon its superpowers, we need to set up the initial state of the application. Here's what ourJavaScript should look like so far:

```javascript
(function() {

  'use strict';

  /**
   * angular
   * Description: Angular!
   */
  angular

  /**
   * app
   * Description: Sets up the initial state of the application
   */
  .module('app', [])

})();
```

We can break this into three parts:

1. The enclosing IIFE, which protects us from possibly polluting the global scope
2. The `angular` declaration
3. The `.module('app', [])` declaration, which sets the initial state of the application and names it `app`

If you're curious, the square brackets inside the setter are responsible for loading up any dependencies we may want via Angular's [dependency injection.](https://docs.angularjs.org/guide/di) We won't be covering dependency injection in this tutorial, but as you get further into Angular, it's a largely important topic, so check it out and get familiar!

Our app does nothing at the moment, so let's build and attach our first controller, and bind it to the DOM somehow.

## Setting Up Our First Controller

To declare a new controller in Angular, we simply attach `.controller` to our module (which is a controller's constructor function). In our case, since we're keeping all our JavaScript in one file, we can just chain it like this:

```javascript
(function() {

  'use strict';

  angular
  .module('app', [])

  /**
   * AppController
   * Description: Sets up a controller
   */
  .controller('AppController', ['$scope', function($scope) {

  }]);

})();
```

There are a few important things to note here. The first is that we've named our controller `AppController` in a similar way to how we named our base level module. Secondly, we've used an inline injection annotation to explicitly specify `$scope` as a dependency of our controller. Thirdly, we've passed the `$scope` variable into an anonymous function, thus making it available inside that function, which is actually where all our controller code will reside. But what exactly is the `$scope` variable?

In Angular, [scope](https://docs.angularjs.org/guide/scope) is an object that refers to the application model, and act as the glue between a controller and a view. In the case of the controller defined above, we can set up the initial state of the scope by attaching properties to the `$scope` object. We can also attach functions to the `$scope` object. All these `$scope` properties will be available to the view at the point in the DOM where the controller is registered. But how do we register a controller?

Over in our HTML, we want some section of the DOM to be bound to the controller so that we can really start leveraging the concept of two way data binding and putting Angular to work. Remember, we named our controller `AppController`, so our HTML is as simple as this:

```html
<main ng-controller="AppController">
</main>
```

We've now made use of our second directive, the `ng-controller` directive, and our `AppController` is now bound to the DOM. We're ready to see some action! From here on, we'll walk through some basics to see Angular in action. All the code that follows can be assumed to be contained inside our controller function.

*It's generally advisable to use the `controller as` syntax in angular to make it obvious what controller you're accessing in the template. In this tutorial though, we'll be keeping it simple.*

## Ridiculously Simple Data Binding

In our HTML, we can render scope properties using the double curly brace notation, like this:

```html
<p>Hello, <em>{{ username }}</em>!</p>
```

As it stands, nothing in between the  would get rendered between the curly braces, because we haven't attached `username` to our `$scope` object. Let's do that quickly:

```javascript
/**
 * username
 * Description: The default username to attach to the scope
 */
$scope.username = 'Nick';
```

Hit refresh, and voila! You've just witnessed two way data binding in action for the first time in this app. But let's take it further, and see if we can change the username on click of a button.

## Changing Scope Properties with ng-click

Angular offers another directive to us called [ngClick](https://docs.angularjs.org/api/ng/directive/ngClick) which allows us to specify custom behaviour when an element is clicked. Again, we see the power of Angular coming into play here, because we're able to sidestep all of the usual event listener and handling logic, and leverage ngClick directly. Let's build on our previous example, but this time, create a function that changes the scope's username:

JavaScript:

```javascript
/**
 * changeUsername
 * Description: Updates the scope username
 */
$scope.changeUsername = function(username) {
  $scope.username = username;
};
```

Now, in our template, we can use ngClick and pass in a parameter directly. Here's the resulting HTML:

```html
<p>Hello, <em>{{ username }}</em>!</p>
<button ng-click="changeUsername('Bob')">Hello Bob?</button>
<button ng-click="changeUsername('Adam')">Hello Adam?</button>
<button ng-click="changeUsername('Elena')">Hello Elena?</button>
```

When you click the buttons now, you should see the name update! This, again, demonstrates the magic of Angular's two way data binding. All we did was update the scope property in the controller, and the corresponding result got output in the DOM.

*Sidenote: Angular's directives are all named in camel case (eg: ngClick), but when leveraged in templates, get hyphenated (eg: ng-click).*

## Looping Over an Array with ng-repeat

A lot of times in JavaScript, we're faced with some collection of items that we want to iterate over, output, and add some functionality to. Typically, for arrays, you'd use a `for` loop, iterating over the array and manually building up a string or series of elements, and append it to the DOM. Angular offers us the [ngRepeat](https://docs.angularjs.org/api/ng/directive/ngRepeat) directive to iterate over arrays quickly and easily, accessing each item of the array along the way. First, let's build up an array in our controller:

```javascript
/**
 * usersArray
 * Description: A generic list of users, possibly pulled from a database, in
 * array notation.
 */
$scope.usersArray = [
  ['BB', 'King'],
  ['Ray', 'Charles'],
  ['Muddy', 'Waters'],
  ['Lightnin', 'Hopkins'],
  ['Howlin', 'Wolf']
];
```

Inside our template, we can simply attach the ngRepeat directive to the element that we want repeated, and inject the contents of the current index using the double curly brace syntax. Here's the HTML:

```html
<table>
  <tr>
    <th>First Name</th>
    <th>Last Name</th>
  </tr>
  <tr ng-repeat="user in usersArray">
    <td>{{ user[0] }}</td>
    <td>{{ user[1] }}</td>
  </tr>
</table>
```

As you can see, we're attaching the ngRepeat to the `tr` element using the `user in usersArray` syntax. This means that the `tr` element will be the repeating one, and `user` will indicate the value at the current index in the array. Since our array is just a collection of sub-arrays, `user[0]` will output "BB" and `user[1]` will output "King" on the first step through the array (i.e. at index 0). 

## Looping Over an Array of Objects with ng-repeat

A lot of times in JavaScript, we might be using a collection of objects. Typically, these get sent back from a database or an API call as some response data. Let's model a typical collection of objects:

```javascript
$scope.usersObject = [
  {
    firstname: 'BB',
    lastname: 'King'
  },
  {
    firstname: 'Ray',
    lastname: 'Charles'
  },
  {
    firstname: 'Muddy',
    lastname: 'Waters'
  },
  {
    firstname: 'Lightnin',
    lastname: 'Hopkins'
  },
  {
    firstname: 'Howlin',
    lastname: 'Wolf'
  }
];
```

Now, using the same logic, we can iterate over the collection of objects:

```html
<table>
  <tr>
    <th>First Name</th>
    <th>Last Name</th>
  </tr>
  <tr ng-repeat="user in usersObject">
    <td>{{ user.firstname }}</td>
    <td>{{ user.lastname }}</td>
  </tr>
</table>
```

Notice how this time, we're able to access object properties directly using the dot notation! Simple, easy, and effective. Moving on.

## Simple Input Binding

A lot of times in applications, we deal with user input. Angular provides us (yet again) with a super easy way to bind form inputs to scope properties via the [ngModel](https://docs.angularjs.org/api/ng/directive/ngModel) directive. Let's initialize our model with a random string, and create a function that alerts this model contents when called:

JavaScript:

```javascript
/**
 * randomUserModel
 * Description: Initializes the random user model username
 */
$scope.randomUserModel = 'random user';

/**
 * randomUserModelAlert
 * Description: Alerts the random username available from the ng model.
 */
$scope.randomUserModelAlert = function() {
  alert($scope.randomUserModel);
};
```

In our template, we can bind an input to the model, thus opening up a data binding connection, like this:

```html
<input type="text" ng-model="randomUserModel">
<button ng-click="randomUserModelAlert()">alert my random name!</button>
```

Whenever we change the value in the input field, the scope property will automatically be updated. And when we make a call to our alert function (using ngClick), it will alert the updated model contents.

## Showing and Hiding with ng-show & ng-hide

[ngShow](https://docs.angularjs.org/api/ng/directive/ngShow) and [ngHide](https://docs.angularjs.org/api/ng/directive/ngHide) are two pretty neat directives, in the sense that they take a lot of direct DOM manipulation out of the equation for us. They either show or hide DOM elements based on the truthy/falsy evaluation of the expression in the ngShow/ngHide attribute. In other words, if we set `ng-show="true"` in our template, the element will indeed show, as `true` evaluates to true (obviously). Likewise, if we set `ng-hide="true"`, it will be hidden. For this example, we don't even need to use any JavaScript at all. We'll make use of [checkbox inputs](https://docs.angularjs.org/api/ng/input/input%5Bcheckbox%5D), and the way that Angular handles the true/false values when we bind a model to them. Here's some sample HTML:

```html
<input type="checkbox" id="box1" name="box1" ng-model="checkboxModel.box1">
<label for="box1">Show box1!</label>
<br>
<input type="checkbox" id="box2" name="box2" ng-model="checkboxModel.box2">
<label for="box2">Hide box2!</label>
<div class="c-component__box" ng-show="checkboxModel.box1">
  <h3>I am box 1</h3>
  <p>woo woo!</p>
</div>
<div class="c-component__box" ng-hide="checkboxModel.box2">
  <h3>I am box 2</h3>
  <p>woo woo!</p>
</div>
```

When "Show box 1" is checked, `checkboxModel.box1` evaluates to true, and Angular tells box 1 to show by evaluation `ng-show="checkboxModel.box1"`. Likewise, when the second checkbox is checked, `checkboxModel.box2` evaluates to true, and thus, box 2 gets hidden.

## Altering Class Names with ng-class

We'll wrap up our introduction to Angular with one of my personal favourite directives, [ngClass](https://docs.angularjs.org/api/ng/directive/ngClass), which allows you to dynamically set classes on HTML elements by data-binding an expression. You can read about the various ways to leverage ngClass, but in our example, we're going to keep it simple. Let's first set up an empty class name, and a function that could potentially change that scope property inside the controller:

```javascript
/**
 * textClass
 * Description: A generic class name to attach to a string of text
 */
$scope.textClass = '';

/**
 * changeTextClass
 * Description: Changes the text class
 */
$scope.changeTextClass = function(name) {
  $scope.textClass = name;
}
```

Now, in our template, we can assign `textClass` to the ngClass attribute, and set up a button that triggers the `changeTextClass` function:

```html
<p class="c-component__text" ng-class="textClass">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Facere molestias explicabo aspernatur inventore nam fugit, nihil numquam distinctio totam hic, iure porro, magnam consequuntur nemo deleniti dicta amet quo assumenda.</p>
<button ng-click="changeTextClass('pink')">Add a pink class</button>
```

When we click the button, the function gets called, the scope property gets updated, and so too does the class name. And just like that, our text is pink. Magic.

## Wrap Up

And that’s a wrap! In this tutorial, we touched on a lot of basics that should get you off the ground with Angular, but we've barely scratched the surface. Angular is a beast, but just approach it stepwise and you'll be good to go. I hope to be writing a lot more on Angular soon, so stay tuned!

Also, don’t forget, you can view the demo and download the source by clicking the links below, and if you have and questions, comments, or feedback, you can also leave them below.

<p class="text-align--center">
<a href="https://github.com/callmenick/angular-js-introduction" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/angular/intro-to-angular/" class="button button--inline-block button--medium">View Demo</a>
</p>