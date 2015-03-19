<p class="text-align--center">
<a href="http://callmenick.com/_development/image-caption-reveal-on-hover/image-caption-reveal-on-hover-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/image-caption-reveal-on-hover/" class="button button--inline-block button--medium">View Demo</a>
</p>

This tutorial demonstrates some neat effects for image captions that reveal on hover. Instead of displaying captions regularly underneath or to the side of the image, we’re going to use CSS3 transitions to transition out the image and transition in the caption when we hover over the image.

For touch devices that don’t have hover capabilities, we’ll use Modernizr to detect touch, and invoke the transitions with touch instead of hover. Note that Modernizr isn’t a bullet proof method for detecting touch, but it covers a great range. On touch devices, we will display a close button when the caption is revealed. This will allow the user to close the caption.

## The Structure

We’re going to use the HTML5 `figure` element, which will allow us to have an image and a figure caption inside the tag. When we hover over the element, the image will transition out and the caption will transition in. We’ll target non-touch devices for hover using Modernizr (which will plug the `.no-touch` class into the html tag that the beginning of our doc), and for touch devices, we’ll use a little JavaScript to add the `.hover` class on touch. For touch devices, Modernizr will plug the `.touch` class into the html tag, so our JavaScript targeting will be easy.  Finally, I’ll be using classie.js for class helper functions. Without further ado, let’s dive in to the markup!

## The HTML Markup

For all of our examples, the HTML markup will be similar. We will have a `figure` element, and an `img` and `figcaption` element inside. Be mindful of HTML5 support for the figure element. The markup will look like this:

```html
<figure>
    <img src="img/img-src.jpg" alt="">
    <figcaption>
        <h3>Image title</h3>
        <span>Image Caption</span>
        <a href="#" class="close-caption hidden">x</a>
    </figcaption>
</figure>
```

Simple enough, right? Now, let’s look at some CSS, that will give our figure some style.

## The CSS

We want our figure to have a relative positioning, so that captions and images inside of it can stack on top of each other via absolute positioning. Our image will take on `display:block` with a relative positioning, and our caption take on an absolute display. The initial z-indexes are important too, so the image has a higher z-index than the caption (to start). I’ve given some basic styles to the `h3` and `span` that are inside the figure caption too. And finally, as seen in the HTML and CSS, the caption is initially hidden by adding the class `.hidden` to it. This is what the CSS for all figures looks like:

```css
figure {
    margin: 0;
    position: relative;
}
figure img {
    display: block;
    position: relative;
    z-index: 10;
    max-width: 100%;
    height: auto;
}
figure figcaption {
    display: block;
    position: absolute;
    z-index: 5;
}
figure h3 {
    color: #fff;
    font-size: 22px;
    line-height: 1.2;
    font-weight: 700;
    margin-bottom: 10px;
}
figure span {
    color: #b2cce1;
    display: block;
    line-height: 1.2;
}
figure a.close-caption {
    display: block;
    position: absolute;
    width: 44px;
    height: 44px;
    text-align: center;
    line-height: 44px;
    font-size: 24px;
    font-weight: 700;
    color: #315a7d;
}
figure a.close-caption.hidden {
    display: none
}
```

So far so good, right? Time to dive into each of our effects. Remember, each of our effects will either be triggered on hover, or on touch when the user is on a touch enabled device. Also remember to prefix all your CSS properties to work across all engines!

## Effect 1 – Image Slide Up

For this effect, when we hover over the figure element, the image will slide up, and the caption will reveal. We’ll use the CSS transform property to apply a translateY when we hover, or add the hover class. Our CSS looks like this:

```css
#effect-1 a.close-caption {
    bottom: 10px;
    right: 10px;
}
#effect-1 figure {
    background-color: steelblue
}
#effect-1 figcaption {
    bottom: 0;
    left: 0;
    width: 100%;
    padding: 20px;
}
#effect-1 figure img {
    transition: all 0.3s;
}
.no-touch #effect-1 figure:hover img,
#effect-1 figure.hover img {
    transform: translateY(-90px);
}
```

## Effect 2 – Image Flip Out, Caption Flip In

For this effect, when we hover, we want the image to spin about the y-axis out of view, and the caption to spin about the y-axis into view. We’ll be using the property  `rotateY`, and we need to hide the `backface-visibility` to prevent the backs of each element remaining in view when they spin about the y-axis. We also need to start our caption off at -180deg, so that it’s behind the image to begin. Here’s the CSS:

```css
#effect-2 a.close-caption {
    top: 10px;
    right: 10px;
}
#effect-2 figure figcaption {
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: steelblue;
    text-align: center;
    backface-visibility: hidden;
    transform: rotateY(-180deg);
    transition: all 0.5s;
}
#effect-2 figure figcaption h3 {
    margin-top: 150px
}
#effect-2 figure img {
    backface-visibility: hidden;
    transition: all 0.5s;
}
.no-touch #effect-2 figure:hover img,
#effect-2 figure.hover img {
    transform: rotateY(180deg);
}
.no-touch #effect-2 figure:hover figcaption,
#effect-2 figure.hover figcaption {
    transform: rotateY(0);
}
```

## Effect 3 – Scale to Corner

In our third effect, the image will scale down to a fraction of its initial size and pin to the top left corner, revealing the caption behind it. We’ll have to set the `transform-origin` property to the top left, and use the scale property to achieve the desired effect. Here’s our CSS:

```css
#effect-3 a.close-caption {
    bottom: 10px;
    right: 10px;
}
#effect-3 figure {
    background-color: steelblue
}
#effect-3 figure figcaption {
    width: 100%;
    bottom: 0;
    left: 0;
    padding: 20px;
}
#effect-3 figure img {
    transition: all 0.3s;
}
.no-touch #effect-3 figure:hover img,
#effect-3 figure.hover img {
    transform-origin: top left;
    transform: scale(0.5);
}
```

## Effect 4 – Caption Push Onto Image

In this effect, the caption will nudge the image to the right and partially out of view, while taking up some space itself. I achieved the nudge effect by transitioning in the image and caption at different speeds. The caption is initially out of view, achieved by using the `translateX` property and setting it to -100%. The figure’s overflow is also set to hidden to hide the caption when out of view, and the image when it is nudged to the right. Here’s the CSS:

```css
#effect-4 figure {
    overflow: hidden
}
#effect-4 figure a.close-caption {
    bottom: 10px;
    left: 10px;
}
#effect-4 figure figcaption {
    background-color: steelblue;
    top: 0;
    left: 0;
    width: 50%;
    height: 100%;
    padding: 20px;
    transform: translateX(-100%);
    transition: all 0.5s;
}
#effect-4 figure img {
    transition: all 0.3s;
}
.no-touch #effect-4 figure:hover img,
#effect-4 figure.hover img {
    transform: translateX(50%);
}
.no-touch #effect-4 figure:hover figcaption,
#effect-4 figure.hover figcaption {
    transform: translateX(0);
}
```

## Effect 5 – Image Spin Out Of View

In this effect, the image will spin out of view as if it were spiralling away into the screen. We’ll use a combination of two transform properties – rotate and scale – to achieve the desired effect. Here’s the CSS:

```css
#effect-5 figure a.close-caption {
    bottom: 10px;
    left: 10px;
}
#effect-5 figure figcaption {
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    background: steelblue;
    padding: 20px;
}
#effect-5 figure img {
    transition: all 0.5s;
}
.no-touch #effect-5 figure:hover img,
#effect-5 figure.hover img {
    transform: rotate(-360deg) scale(0);
}
```

If you download the source, I included some media queries to set it up for cross-device usage. This whole concept isn’t worth it though, unless we target our touch devices. So let’s get to that, and dive in to some JavaScript! Remember, I included classie.js to help me quickly add, remove, and check for classes.

## The JavaScript

The JavaScript first checks if a device is touch enabled. If it is, we query through all our figures. For each figure, we display the close-caption button. We detect touch events, but make sure we listen out for `touchmove` and `touchend` to cancel or complete the desired action (see this code snippet for a further explanation of what’s going on in this part). On touch, we add the hover class to our figure, and if the user presses the close-caption button, we remove the hover class. And that’s it!

```javascript
(function(window){

	// check for touch
	if (Modernizr.touch) {

		// run the forEach on each figure element
		[].slice.call(document.querySelectorAll("figure")).forEach(function(el,i){

			// get close-caption button in variable
			var closeCaption = el.querySelector(".close-caption");

			// show the close-caption button
			classie.remove(closeCaption,"hidden");

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
					classie.add(el,"hover");
				}
			});

			// if close-caption button clicked, remove hover class
			closeCaption.addEventListener("touchend",function(e){
				e.preventDefault();
				e.stopPropagation();
				if (fingerMove == false) {
					if (classie.has(el,"hover")) {
						classie.remove(el,"hover");
					}
				}
			});

		});

	}

})(window);
```

## Wrapping Up

This is just a scratch on the surface of the possibilities out there. I encourage you to experiment with more transitions, transforms, and all these new cool CSS3 properties, and come up with some of your own.

<p class="text-align--center">
<a href="http://callmenick.com/_development/image-caption-reveal-on-hover/image-caption-reveal-on-hover-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/image-caption-reveal-on-hover/" class="button button--inline-block button--medium">View Demo</a>
</p>