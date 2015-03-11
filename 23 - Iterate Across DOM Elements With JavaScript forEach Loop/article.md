The JavaScript forEach loop is neat and fast, and replaces the traditional for loop. Using JavaScript’s new `querySelectorAll` function, we can get all elements in query, and iterate across them. When we use `querySelectorAll`, we fetch a node list. It isn’t quite an array, so we can’t iterate over it with a `forEach` loop. The standard for loops are still a go, but the `forEach` method is fast and neat. The iteration looks like this:

```language-javascript
// run the forEach on each article element
[].slice.call(document.querySelectorAll("article")).forEach(function(el,i){

	// el represents the article in question
	el.querySelector(".title");

	// i represents the iteration count on the foreach loop
	console.log(i);

});
```

## The Script Explained

When we call slice on an array, the array is set as the value of  this in slice. In our case though, we don’t have a predefined array, so we’re starting off with an empty one. The trick now is to use `call()`. The `call()` method [calls a function with a given this value and arguments provided individually](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call). Now, we can manually set the value of this in our function. Our node list is an object with a length value, and looks enough like an array for slice to work on it. [This Stack Overflow post](http://stackoverflow.com/questions/2125714/explanation-of-slice-call-in-javascript) has a nice explanation too, saying that "`[].slice` returns a function object. A function object has a function `call()` which calls the function assigning the first parameter of the `call()` to this; in other words, making the function think that it’s being called from the parameter (the NodeList returned by `document.querySelectorAll('article')`) rather than from an array."

[Here’s a JSFiddle](http://jsfiddle.net/AVuKK/) for you to play with, and see how you can interact with elements inside the `forEach` loop.