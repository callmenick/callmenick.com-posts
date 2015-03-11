CSS animations are playing a key role nowadays in some of the more cutting edge websites and web apps. But rather than just doing a bunch of animations sporadically, sometimes we want to do animations in sequence. Other times too, we want certain features/functionalities to become available to the user at the beginning or end of the animation. Without frame by frame control, this can be a task. But luckily, we can listen for CSS animation events with JavaScript. Consider the following HTML and CSS:

```language-markup
<!doctype html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Title</title>
	<link rel="stylesheet" href="style.css">
</head>
<body>

	<div id="monkey" class="animate"></div>
	<div id="banana"></div>

</body>
</html>
```

```language-css
#monkey.animate,
#banana.animate {
    -webkit-animation:myAnimation 1s;
    -moz-animation:myAnimation 1s;
    -ms-animation:myAnimation 1s;
    -o-animation:myAnimation 1s;
    animation:myAnimation 1s;
}
#monkey.hidden {
    display:none;
}
@-webkit-keyframes myAnimation { 
    0% { opacity: 0; }
    100% { opacity: 1; }
}
@-moz-keyframes myAnimation { 
    0% { opacity: 0; }
    100% { opacity: 1; }
}
@-ms-keyframes myAnimation { 
    0% { opacity: 0; }
    100% { opacity: 1; }
}
@-o-keyframes myAnimation { 
    0% { opacity: 0; }
    100% { opacity: 1; }
}
@keyframes myAnimation { 
    0% { opacity: 0; }
    100% { opacity: 1; }
}
```

What if we wanted to add the `.hidden` class to `#monkey` after this animation was complete? Or what if we had another element, say `#banana`, that we wanted to invoke an animation on only when #monkey was finished animating, by adding the `.animate` class to it? We’re in luck, thanks to JavaScript.

## CSS Animation Events with JavaScript

JavaScript give us three event listeners to play with when it comes to CSS animations. They are:

* __animationstart__ - listener function fires as soon as the animation begins
* __animationiteration__ - listener function fires at the beginning of every subsequent animation iteration
* __animationend__ - listener function fires at the end of the animation

This is amazing, because now we can chain animation events in some neat and concise JavaScript. Here’s an example of these listeners in action:

```language-javascript
// set the var here
var monkey = document.querySelector("#monkey");

// listen for animation start
monkey.addEventListener("animationstart",function(e){
	console.log("log at beginning of monkey animation");
},false);

// listen for animation iteration
monkey.addEventListener("animationiteration",function(e){
	console.log("log at beginning of each subsequent iteration");
},false);

// listen for animation end
monkey.addEventListener("animationend",function(e){
	console.log("log at end of monkey animation");
},false);
```

Inside the `animationend` listener, we might want to add an `animation` class to our `#banana` div, so it starts animating as soon as the `#monkey` finishes animating. We can plug into some events inside the listener also. An example of an event is the `elapsedTime`, which will be 0 at the start of the animation, will log the time at the beginning of each subsequent iteration inside `animationiteration`, and will be the final time inside `animationend`. Here’s an example of how we would achieve this:

```language-javascript
// listen for animation iteration
monkey.addEventListener("animationiteration",function(e){
	console.log(e.elapsedTime);
},false);
```

## Minor Caveats

Of course, all browsers have their prefixes to be aware of. So we’re looking at this breakdown for cross browser functionality (including IE 10 and up):

* __No prefix__ - animationstart, animationiteration, animationend
* __Webkit__ - webkitAnimationStart, webkitAnimationIteration, webkitAnimationEnd
* __Mozilla__ - mozAnimationStart, mozAnimationIteration, mozAnimationEnd
* __MS__ - MSAnimationStart, MSAnimationIteration, MSAnimationEnd
* __O__ – oAnimationStart, oAnimationIteration, oAnimationEnd

It would be such a chore to have to prefix each event listener 5 times every time you wanted to perform a function. Luckily, we can just use a helper function to take care of it for us. Here’s the helper function, and what our new event listener would look like:

```language-javascript
// prefixer helper function
var pfx = ["webkit", "moz", "MS", "o", ""];
function prefixedEventListener(element, type, callback) {
	for (var p = 0; p < pfx.length; p++) {
		if (!pfx[p]) type = type.toLowerCase();
		element.addEventListener(pfx[p]+type, callback, false);
	}
}

// new event listener function
var monkey = document.querySelector("#monkey");
prefixedEventListener(monkey,"AnimationStart",function(e){
	console.log("log at beginning of monkey animation");
});
```

And that’s it! With all this in mind, and a solid grasp on keyframe animations with CSS, there’s no shortage of awesome that you can get up to. Of course, be wary of browser support and necessary fallbacks if you’re looking for total cross browser compatibility.

## CSS Animation Events Testing

For your benefit, I’ve created a test area where you can start an animation sequence. A “console” div gets logged with all the info of what’s happening in the animation sequence. The links to the source code and demo are for this sequence, so feel free to download and mess around with them to get familiar with how the event listeners are working. Thanks for reading, and if you have any questions/comments/suggestions, feel free to leave them below!