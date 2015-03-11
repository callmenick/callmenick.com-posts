## JavaScript DOM Methods

JavaScript DOM functionality and methods allow us to append an element onto another by calling the `appendChild` method. A common use for this is creating an element with JavaScript, and wanting to append it to the body of our HTML (for example, a lightbox overlay). What if we wanted to prepend an element to another element though? What if we generated an element with some content using JavaScript, and wanted to prepend  a title to this element, before the content? JavaScript doesn’t give us a  `prependchild` method, but we can create our own prependchild JavaScript function easily, using some already existing methods. Let’s dig in.

## Creating Our Own Prependchild JavaScript Code

We’re going to use two JavaScript methods to achieve what we want:

* `insertBefore`
* `firstChild`

The `insertBefore` method takes two arguments. The first is the element we want to insert, and the second is the element that we want our target to be before. If that sounds confusing, let’s break it down like this:

1. Let’s fetch an element called `section` that exists in our DOM already.
2. Let’s create a new element called `span`, that we want to insert inside `section`.
3. We want the position of span to be before anything else inside `section`.

The method `firstChild` will allow us to target the first child element inside an element, so we’ll use this in our second parameter for `insertBefore`. Here’s what our final code will look like:

```language-javascript
// fetch our section element
var section = document.querySelector("section");

// create our span element
var span = document.createElement("span");

// prepend our span eleemnt to our section element
section.insertBefore( span, section.firstChild );
```

## Wrap Up

This snippet is a great example of not overcomplicating things when we need to achieve a certain goal. A little thought and usage of what’s already available to us lets us achieve anything we need to.