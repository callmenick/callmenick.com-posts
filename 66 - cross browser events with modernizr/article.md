Transition and animation events are a powerful JavaScript feature that allows us to pretty much create visual stories all with native browser tech. By listening for transition endings or animation iterations, we could trigger subsequent functions involving new animations and iterations, ultimately composing some beautiful interactivity. Modernizr helps us out a lot with achieving some cross-browser events by way of their `Modernizr.prefixed()` function. Make sure to have that checked off in the extensibility section when you generate your Modernizr script. Here's a look at some example usages:

```javascript
/**
 * transition end event listener
 *
 */

var transEndEventNames = {
  "WebkitTransition" : "webkitTransitionEnd",
  "MozTransition"    : "transitionend",
  "OTransition"      : "oTransitionEnd",
  "msTransition"     : "MSTransitionEnd",
  "transition"       : "transitionend"
},
transEndEventName = transEndEventNames[ Modernizr.prefixed('transition') ];
document.querySelector("#el").addEventListener( transEndEventName, function() {
  // code here runs after #el transition has ended
});

/**
 * animation iteration event listener
 *
 */
 
var animIterationEventNames = {
  "WebkitAnimation" : "webkitAnimationIteration",
  "MozAnimation"    : "animationiteration",
  "OAnimation"      : "oAnimationIteration",
  "msAnimation"     : "MSAnimationIteration",
  "animation"       : "animationiteration"
},
animIterationEventName = animIterationEventNames[ Modernizr.prefixed('animation') ];
document.querySelector("#el").addEventListener( animIterationEventName, function() {
  // code here runs after each animation iteration of #el
});
```