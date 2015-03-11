<p class="text-align--center">
<a href="https://github.com/callmenick/Draw-Fill-SVG" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/tutorial-demos/draw-fill-svg/" class="button button--inline-block button--medium">View Demo</a>
</p>

## Introducing SVG

SVG stands for "scalable vector graphics", and that means we can scale SVG's as much as we want without losing any quality. When you output an SVG in your document, it looks similar to HTML - a bunch of tags with attributes. That's because SVG is XHTML, and we can target these tags and dynamically alter their attributes. For a more in depth look at the fundamentals of inline SVG, check out my previous tutorial ["Getting Started With Inline SVG"](http://callmenick.com/2014/10/19/getting-started-inline-svg/).

## About This Plugin

I couldn't think of a better name for my plugin, so I called it "Draw Fill SVG". It's a combination of an SVG drawing animation and a stroke fadeout / fill fadein animation. In a nutshell, it performs three tasks: 

1. Applies a reset to the SVG attributes in question.
2. "Draws" the SVG by animating the strokes from 0 to 100%.
3. Fades out the stroke, and fades in the fill.

So how is all this achieved? SVG comes with a range of attributes and elements for us to leverage and manipulate. Of key importance will be the following:

* `fill` and `fill-opacity` - defines the fill colour of an SVG element, and that fill colour's opacity
* `stroke` and `stroke-opacity` - defines the stroke colour of an SVG element, and that stroke colour's opacity
* `stroke-dashoffset` - defines the distance into the dash pattern to start the dash
* `stroke-dasharray` - controls the pattern of dashes and gaps used to stroke paths

You can  read more about each of these properties on the [MDN attribute reference](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute).

## Resetting & Drawing The Stroke

Resetting and drawing the stroke is achieved by a very well documented technique which you can read about [here](http://jakearchibald.com/2013/animated-line-drawing-svg/). In a nutshell, it works like this:

1. Calculate the length of an SVG path.
2. Reset all transitions on the path to none.
3. This might seem like the confusing one, but give it a little thought and it will become clear. Set the `stroke-dasharray` to [length, length], and the `stroke-dashoffset` to [length], ultimately making the dash as long as the stroke itself, and offsetting it to the length of the stroke making it initially invisible. This is the technique mentioned in the link above.
4. Add a transition to the `stroke-dashoffset` property.
5. Set the `stroke-dashoffset` to 0, ultimately "drawing" the stroke.

This is all done via JavaScript, so all calculations and transitions are done dynamically, making it easy.

## Fade Out The Stroke, Fade In The Fill

This function leverages on transition event listeners, and listens for the ending of the stroke transition. Transition end events require prefixing due to different browser engines, and I'm using Modernizr to do take care of it in my function. You can read more about that in [this post](http://callmenick.com/2014/10/19/cross-browser-transition-animation-events-modernizr/). Once the stroke has finished drawing itself, the following happens:

1. Reset the transitions on the path to `none` again.
2. Set the transitions on the `fill-opacity` and `stroke-opacity` properties.
3. Change `fill-opacity` to 1.
4. Change `stroke-opacity` to 0.

## Replaying The Animation

In the plugin, I created a public `replay` function that restarts the animation from the beginning. This can be called easily on any new instance of the object. There's a demo below to see how this works, so sit tight!

## Writing The Plugin

Let's set up our plugin. First, we'll declare it, and any other variables and functions we're going to need. Our empty plugin with the required functions will look like this:

```javascript
/**
 * Draw Fill SVG
 *
 * A plugin that simulates a "draw" effect on the stroke of an SVG, fades out
 * the stroke, and fades in a fill colour.
 *
 * Licensed under the MIT license.
 * http://www.opensource.org/licenses/mit-license.php
 *
 * Copyright 2014, Call Me Nick
 * http://callmenick.com
 */

(function( window ){

  'use strict';

  /**
   * Cross browser transition end events
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

  /**
   * Extend obj function
   *
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
   * DrawFillSVG constructor
   *
   */

  function DrawFillSVG( options ) {
    this.options = extend( {}, this.options );
    extend( this.options, options );
    this._init();
  }

  /**
   * DrawFillSVG options
   *
   * Available options:
   * elementId - the ID of the element to draw
   */

  DrawFillSVG.prototype.options = {
    elementId : "svg"
  }

  /**
   * DrawFillSVG _init
   *
   * Initialise DrawFillSVG
   */

  DrawFillSVG.prototype._init = function() {
    
  }

  /**
   * Add to global namespace
   */

  window.DrawFillSVG = DrawFillSVG;

})( window );
```

We defined our transition-end event variable, our object extender, our DrawFillSVG constructor, and we're now inside our `_init` function. Let's now take a look at the `_init` function and what we need to achieve with it.

### The `_init` Function

In this function, we want to cache the necessary vars and make them accessible throughout out plugin. The variables in question would be the SVG `id` and the collection of paths that our SVG contains. After we have these variables all set, we need to animate the stroke. Here's now what our `_init` function looks like:

```javascript
/**
 * DrawFillSVG _init
 *
 * Initialise DrawFillSVG
 */

DrawFillSVG.prototype._init = function() {
  this.svg = document.getElementById(this.options.elementId);
  this.paths = this.svg.querySelectorAll("path");
  this._initAnimation();
}
```

### The `_initAnimation` Function

At the end of our `_init` function, we call on the `_initAnimation` function. This function performs a sequence of steps that essentially initialised the animation by drawing the strokes for each path, then going onto the next step. Based on the references above and the logical flow already discussed, this is that the `_initAnimation` looks like:

```javascript
/**
 * DrawFillSVG _initAnimation()
 *
 * Reset some style properties on our paths, add some transitions, set the
 * stroke-dasharray to the length of the path, and the stroke-dashoffset to
 * the length of the path pushing it out of view initially. Then, set the 
 * stroke-dashoffset to 0, animating the strokes in a drawing manner. Then,
 * run the path filler sequence.
 */

DrawFillSVG.prototype._initAnimation = function() {
  for ( var i = 0; i < this.paths.length; i++ ) {
    var path = this.paths[i];
    var length = path.getTotalLength();

    // reset opacities
    path.style.fillOpacity = 0;
    path.style.strokeOpacity = 1;

    // reset transitions
    path.style.transition = path.style.WebkitTransition = "none";

    // reset stroke dash array and stroke dash offset
    path.style.strokeDasharray = length + " " + length;
    path.style.strokeDashoffset = length;
    path.getBoundingClientRect();

    // apply new transitions
    path.style.transition = path.style.WebkitTransition = "stroke-dashoffset 2s ease-in-out";

    // go baby go
    path.style.strokeDashoffset = 0;

    // fill the path
    this._fillPath( path );
  }
}
```


### The `_fillPath` Function

Inside the `for` loop in the above function we call a `_fillPath` function and pass in a `path` parameter. This function takes each path, listens for the end of its transition, transitions out the path's stoke, then transitions in the fill. Here's what the `_fillPath` function looks like:

```javascript
/**
 * DrawFillSVG _fillPath()
 *
 * Resets the transition props, then fills the path and fades out the stroke
 * by updating the styles.
 */

DrawFillSVG.prototype._fillPath = function( path ) {
  path.addEventListener( transEndEventName, function() {
    // reset transitions
    path.style.transition = path.style.WebkitTransition = "none";
    path.style.transition = path.style.WebkitTransition = "fill-opacity 1s ease-in-out, stroke-opacity 1s ease-in-out";

    // edit props
    path.style.fillOpacity = 1;
    path.style.strokeOpacity = 0;
  } );
}
```

### The `replay` Function

You may be interested in being able to replay the animation at your own will. For this, I created the simple `replay` function that just takes us back to `_initAnimation`. Here's the `replay` function:

```javascript
/**
 * DrawFillSVG replay
 *
 * A public function that allows you to replay the animation if you want. For
 * example, click a button, and replay the animation.
 */

DrawFillSVG.prototype.replay = function() {
  this._initAnimation();
}
```

## Sample HTML

Here's some sample HTML that follows the pattern of what's required for the plugin to work. Note the following:

* To achieve the "draw" effect, SVG must have a stroke
* To achieve the fade-in fill effect, SVG must have a fill
* All shapes in the SVG must be defined as `path`s for the plugin to work

Sample HTML:

```html
<svg xmlns="http://www.w3.org/2000/svg" id="svg" class="svg" viewBox="0 0 960 480" preserveAspectRatio="xMinYMin meet">
  <path fill="..." stroke="..." stroke-width="..."/>
  <path fill="..." stroke="..." stroke-width="..."/>
  <path fill="..." stroke="..." stroke-width="..."/>
  <path fill="..." stroke="..." stroke-width="..."/>
</svg>
```

## Creating A New Instance (Plugin Usage)

Creating a new instance is simple, and only one option is required - the `id` of the SVG. You can create the new instance as follows:

```html
<script src="path/to/svgdrawfill.js"></script>
<script>
  (function() {
    var myAnimation = new DrawFillSVG({
      elementId: "svg"
    });
  })();
</script>
```

Creating the new instance automatically performs the animation.

## Using The Public `replay` Function

Using the `replay()` function is also very easy. Just call it on your new object instance. A real use case would be implementing a replay on a button click. Here's an example of that in action:

```html
<button id="animate">animate again</button>
<script>
  (function() {
    var myAnimation = new DrawFillSVG({
      elementId: "svg"
    });
    document.getElementById("animate").addEventListener( "click", function() {
      myAnimation.replay();
    });
  })();
</script>
```

## Dependencies

It goes without saying that this plugin will only work in browsers that support SVGs and transitions. I also used Modernizr's `prefixed()` extension for cross-browser transition end events. This is my preferred method.

Another important point to note is that this plugin targets ONLY paths in the SVG, and not any other basic shapes like `circle` or `rect`. In order for the plugin to work properly, please convert all shapes to paths.

## Wrap Up

And that's about it. I hope you enjoy this plugin, and use it as an introduction to the wonderful world of SVG manipulation and animation!

## License

Licensed under the MIT license - http://www.opensource.org/licenses/mit-license.php

<p class="text-align--center">
<a href="https://github.com/callmenick/Draw-Fill-SVG" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/tutorial-demos/draw-fill-svg/" class="button button--inline-block button--medium">View Demo</a>
</p>