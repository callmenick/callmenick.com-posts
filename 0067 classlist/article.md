Adding and removing classes has to be one of the most common tasks when it comes to JavaScript and DOM manipulation. A user clicks the close button? Add the hidden class to that modal. In my early forays in the web development world, I'd sometimes include the entire jQuery library just to achieve this task! Not good at all. Nowadays, as I try to write vanilla JS as much as possible, it's important to have efficient, native ways to perform common tasks like adding and removing classes. Enter `classList`, a JavaScript property that performs this task.

## Element.classList

The `classList` property returns a token list of the class attribute of the element in question. Luckily for us, it also comes with a few handy methods:

* `add` - adds a class
* `remove` - removes a class
* `toggle` - toggles a class
* `contains` - checks if a class exists

Implementation of these methods is super easy. For example:

```javascript
// adds class "foo" to el
el.classList.add("foo");

// removes class "bar" from el
el.classList.remove("bar");

// toggles the class "foo"
el.classList.toggle("foo");

// outputs "true" to console if el contains "foo", "false" if not
console.log( el.classList.contains("foo") );

// add multiple classes to el
el.classList.add( "foo", "bar" );
```

## Support

Support is high, which is awesome. All major and modern browsers support it and support goes back many versions. IE support is only from 10 and up though, but luckily, there's a cross browser shim (that adds support for IE8 and up).

## Cross Browser Shim

The cross browser shim is written by [Eli Grey](http://eligrey.com/) and the MDN cites this shim as the go-to one. I won't paste out the code here, but you can get it on the [official GitHub repo](https://github.com/eligrey/classList.js). According to the repo docs, the shim works in every browser except IE 7 or earlier.

## Awesome

Awesome.