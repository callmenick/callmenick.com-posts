With the touch screen market booming, us web developers have to constantly find ways to make our websites function beautifully across devices. Users are getting accustomed to native snappiness and gestures, so to serve users this type of functionality, we have to smartly incorporate them in our projects. One such example is using touch events instead of click or hover with JavaScript, when touch events are supported. This method is attractive, because click events present a delay of about 300ms to check if a user meant to do a double click, whereas touch events happen instantly (like we would expect in a native app). In this snippet, I’ll use the [Modernizr](http://modernizr.com/) test Modernizr.touch to indicates if the browser supports touch events. Note that this method does not necessarily reflect a touch screen device, as is widely documented. According to Modernizr’s website:

> It’s recommended to set both touch *and* mouse events together, to cater for hybrid devices – see the [Touch And Mouse](http://www.html5rocks.com/en/mobile/touchandmouse/) HTML5 Rocks article.

Modernizr will inject a `.no-touch` class into our HTML tag at the beginning of the document, reflecting that the browser does not have touch event capabilities. So we will target our CSS like this:

```css
.no-touch figure:hover,
figure.hover {
    /* your css hover stuff here */
}
```

## About The CSS

Let’s assume we want to target all figure elements in our project. The CSS targets the figure element when we have no touch events enabled in the browser. On hover, our CSS properties get applied. We also have a hover class for the figure element that will be applied on touch, if Modernizr detects touch capabilities. This method covers a broad range, because non-touch enabled browsers that are on touch screen devices tend to register hover events on click.

## Touch Events With JavaScript

In the JavaScript, we’re going to look for touch capabilities with Modernizr. If touch is enabled, we look out for touchstart. But we need to be careful. If the user moves their finger, we want to cancel the whole process. In other words, we only want to add our .hover class if the user performs a touchstart and touchend without a touchmove in between. I’m using the classie.js library for adding and removing classes with ease. Let’s look at the JavaScript.

```javascript
(function(window){

	// check for touch
	if (Modernizr.touch) {

		// run the forEach on each figure element
		[].slice.call(document.querySelectorAll("figure")).forEach(function(el,i){

			// check if the user moves a finger
			var fingerMove = false;
			el.addEventListener("touchmove",function(e){
				e.stopPropagation();
				fingerMove = true;
			});

			// always reset fingerMove to false on touch start
			el.addEventListener("touchstart",function(e){
				e.stopPropagation();
				fingerMove = false;
			});

			// add hover class if figure touchend and fingerMove is false
			el.addEventListener("touchend",function(e){
				e.stopPropagation();
				if (fingerMove == false) {
					classie.toggle(el,"hover");
				}
			});

		});

	}

})(window);
```

## Breaking Down The JavaScript

Using the new and cool JavaScript forEach syntax, I’m targeting all figure elements. We set a variable called fingerMove to false, and use an event listener to detect `touchmove`. If `touchmove` is detected, we set fingerMove to true. On `touchstart` though, we always reset `fingerMove` to false. Now, on our figure, we can check for `touchend`, and if `fingerMove` is still false (i.e. no `touchmove` happened, setting `fingerMove` to true), we can toggle our `.hover` class. We make sure to include `stopPropagation()` on all our touch events to prevent DOM bubbling. And there you have it, a snappy, quick, native-feeling touch event on our website.