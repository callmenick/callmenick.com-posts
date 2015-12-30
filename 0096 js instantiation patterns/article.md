In a lot of my demos, there are bits of JavaScript that pave the way for visual interactivity. The great thing about those scripts though is that they are reusable as "instances" that can exist around each other, without ever interfering with each other. In other words, if we can have many buttons that perform the same set of functionality, without ever having to repeat that functionality or worry about it conflicting. These are in fact JavaScript objects with added functionality, and each object instance is responsible for itself.

In JavaScript, there are 4 instantiation patterns available to us. We'll go through each of them here, and discuss the pros and cons of each. In the end, I'll give my opinion on my favourite one. Here are the 4 patterns:

1. Functional Instantiation
2. Functional-shared instantiation
3. Prototypal instantiation
4. Pseudoclassical instantiation

Let's look at each of them below.

## Functional Instantiation

Functional instantiation is at the root of object instantiation in JavaScript. We create a function, and inside it, we can create an empty object, some scoped variables, some instance variables, and some instance methods. At the end of it all, we can return that instance, so that every time the function is called, we have access to those methods. Here it is in action:

```javascript
// Set up
var Func = function() {
  var someInstance = {};
  var a = 0;
  var b = 1;
  
  someInstance.method1 = function() {
    // code for method1 here
  }
  
  someInstance.method2 = function() {
    // code for method2 here
  }
  
  someInstance.logA = function() {
    return a;
  }
  
  return someInstance;
}

// Usage
var myFunc = Func();
myFunc.logA(); // returns a
```

**Pros:** This pattern is the easiest to follow, as everything exists inside the function. It's instantly obvious that those methods and variables belong to that function.

**Cons:** This pattern creates a new set of functions in memory for each instance of the function `Func`. If you're creating a big app, it's ultimately just not suitable in terms of memory.

## Functional Shared Instantiation

Functional-shared instantiation is similar to functional instantiation, but the methods are an extension of the function instead. Like before, we create an empty object inside our function and return that object. Before we return it though, we extend it with some function methods. For that, we'll need an extender function. Here's the code:

```javascript
// Set up
var Func = function() {
  var someInstance = {};
  someInstance.a = 0;
  someInstance.b = 1;
  extend(someInstance, funcMethods);
  
  return someInstance;
}

var extend = function(to, from) {
  for (var key in from) {
    to[key] = from[key];
  }
}

var funcMethods = {};

funcMethods.method1 = function() {
  // code for method1 here...
}

funcMethods.method2 = function() {
  // code for method2 here...
}

funcMethods.logA = function() {
  return this.a;
}

// Usage
var myFunc = Func();
myFunc.logA(); // returns a
```

**Pros:** In this instantiation pattern, the object methods are referenced in memory, so object instances refer to those references when being called. This allows for great memory management.

**Cons:** If we choose to edit some of the `funcMethods` at some point and then create a new object instance, the old and new instances will refer to different references of the method in memory. This can get confusing for some, but is a minor caveat once you're aware of it.

## Prototypal Instantiation

Prototypal instantiation (which doesn't actually use the keyword `prototype`) is done by attaching methods directly to the object's prototype using the `Object.create()` method. It's fairly similar to the previous pattern, except this time, the returned object has prototypal methods that directly reference the `funcMethods` object. Check it out:

```javascript
// Set up
var Func = function() {
  var someInstance = Object.create(funcMethods);
  someInstance.a = 0;
  someInstance.b = 1;
  
  return someInstance;
}

var funcMethods = {};

funcMethods.method1 = function() {
  // code for method1 here...
}

funcMethods.method2 = function() {
  // code for method2 here...
}

funcMethods.logA = function() {
  return this.a;
}

// Usage
var myFunc = Func();
myFunc.logA(); // returns a
```

**Pros:** This pattern attaches methods directly to the object's prototype, rather than as attachments to the returned objects like before. 

**Cons:** In my opinion, there is room for improvement on this pattern. Even though the pros are great, it's still a bit of a long-winded implementation.

## Pseudoclassical Instantiation

The last (but note least) pattern is the pseudoclassical pattern, which takes a bit of the long-windedness out of the prototypal pattern. It does, as before, attach methods directly to the object's prototype. Another point of interest is the fact that the object's constructor is automatically included. This means that we can create new instances using the `new` keyword. The reason for this is that some behind the scenes work goes down in this pattern. Before and after all code in the object's body, `this = Object.create(Object.prototype);` and `return this;` get run in the background. Here's the pseudoclassical style in action:

```javascript
// Set up
var Func = function() {
  this.a = 0;
  this.b = 1;
}

Func.prototype.method1 = function() {
  // code for method1 here...
}

Func.prototype.method2 = function() {
  // code for method2 here...
}

Func.prototype.logA = function() {
  return this.a;
}

var myFunc = new Func();
myFunc.logA(); // returns a
```

**Pros:** For me, this feels the most natural when building up an object and attaching methods to it. Methods are attached directly to the prototype, instances are created with the `new` keyword, and the `this` keyword has a distinct scope. Rumour has it that it's the fastest instantiation pattern too, though I'm yet to test it myself.

**Cons:** Newcomers to JavaScript might find this pattern a bit difficult to understand, especially if their grasp on the `this` keyword is not up to scratch.

## Wrap Up

And that’s a wrap! In this tutorial, we looked at the four different instantiation patterns available to us in JavaScript. Of the four patterns mentioned above, my favourite is the pseudoclassical pattern, for reasons mentioned above. If you've seen the scripts in some of my demos, you'd have noticed that this was (and is) my pattern of choice! Thanks for reading, and remember, if you have and questions, comments, or feedback, you can also leave them below.