<p class="text-align--center">
<a href="http://callmenick.com/_development/css-loaders-and-spinners/css-loaders-and-spinners-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/css-loaders-and-spinners/" class="button button--inline-block button--medium">View Demo</a>
</p>

## Why CSS Spinners & Loaders?

We’ve long lived in a world of animated gif spinners and loaders, and they’ve served their purpose (and still do). They have always had their limitations, though. What if you wanted to use a bigger spinner, or change the colours? What about transparency, or that jagged edge that appears when your spinner doesn’t match up against a similar colour background? Enter the world of CSS spinners and loaders. Using some basic CSS to construct shapes, and some more advanced CSS3 [animations](https://developer.mozilla.org/en-US/docs/Web/CSS/animation) and [keyframes](https://developer.mozilla.org/en-US/docs/Web/CSS/@keyframes) for some movement, we can created beautiful, scalable, and easily modifiable CSS spinners and loaders.

I’m going to demonstrate 4 different spinners, and how to construct them.  You can view the CSS working versions of all these by clicking the “view demo” button above. The images presented in each section should give you a representation of how that spinner works. Some math is involved in creating these spinners, but we’re developers after all, math is easy! Let’s dig right in.

## Common HTML & CSS

All our loaders are single-element loaders, meaning we only have one element in our markup for each spinner. Spinners naturally tend to be absolutely positioned, so we’ll set that also. For each spinner, we’ll be using a combination of base level styling and [pseudo-class :before and :after](http://callmenick.com/2014/04/13/under-the-radar-css-selectors/) styling. Here’s what our common HTML looks like:

```html
<section class="spinner-1">
  <div class="spinner"></div>
</section>

<section class="spinner-2">
  <div class="spinner"></div>
</section>

<section class="spinner-3">
  <div class="spinner"></div>
</section>

<section class="spinner-4">
  <div class="spinner"></div>
</section>
```

and now our common CSS:

```css
section {
  position: relative;
}

.spinner {
  position: absolute;
}
```

Now, let’s get into each of our loaders and their individual styles. Remember, add all the necessary vendor prefixes to your CSS to get as much cross-browser compatibility as possible. The CSS below isn’t prefixed for brevity purposes, but if you download the source, all the prefixes are there.

## CSS Spinner/Loader \#1

![Spinner 1](/files/2015-03/spinner_1.png)

In this spinner, we have a large circular disc. Inside that disc, there’s a smaller, differently coloured circular disc. This small disc spins around the centre axis of the large disc, and is offset a little bit from the edge. We will apply a rotational animation to the small circular disc, but have to use the CSS property transform-origin to offset the axis which it rotates from. We’ll have to use some math to calculate the transform origin based on the offset of the small disc to the outside of the large disc, the width of the large disc, and the width of the small disc. Here’s what the CSS looks like:

```css
.spinner-1 {
  height: 100px;
}
.spinner-1 .spinner {
  width: 100px;
  height: 100px;
  background-color: #28aadc;
  border-radius: 100%;
}
.spinner-1 .spinner:before {
  display: block;
  position: absolute;
  z-index: 2;
  left: 0;
  right: 0;
  top: 10px;
  margin: 0 auto;
  width: 20px;
  height: 20px;
  background-color: #fff;
  content: "";
  border-radius: 20px;
  transform-origin: 10px 40px;
  animation: spinner-1 1s infinite linear;
}

@keyframes spinner-1 {
  from {
    transform: rotate(0deg);
  }

  to {
    transform: rotate(360deg);
  }
}
```

## CSS Spinner/Loader \#2

![Spinner 2](/files/2015-03/spinner_2.png)

This spinner looks like a satellite/spaceship/cyclops of some sort. Cool. We have a stationary small disc, and around it a bigger disc that is somehow cut in half. This is a lot easier than you think, and is achieved with a combination of the border-radius and border properties. A transparent border on the left and right of this disc gives it the effect.. This exterior half disc piece does the spinning, this time with an easing function. Here’s the CSS:

```css
.spinner-2 {
  height: 100px;
}
.spinner-2 .spinner {
  width: 100px;
  height: 100px;
}
.spinner-2 .spinner:before {
  display: block;
  position: absolute;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  border-top: solid 20px #ff3c50;
  border-bottom: solid 20px #ff3c50;
  border-left: solid 20px transparent;
  border-right: solid 20px transparent;
  content: "";
  border-radius: 50px;
  animation: spinner-2 1s infinite ease;
}
.spinner-2 .spinner:after {
  display: block;
  position: absolute;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  margin: auto;
  width: 40px;
  height: 40px;
  background-color: #28aadc;
  content: "";
  border-radius: 20px;
}

@keyframes spinner-2 {
  from {
    transform: rotate(0deg);
  }

  to {
    transform: rotate(360deg);
  }
}
```

## CSS Spinner/Loader \#3

![Spinner 3](/files/2015-03/spinner_3.png)

This spinner consists of two discs of the same size, on opposite ends of an imaginary square shape. The discs converge at the centre of the square, then move vertically in opposite directions. They then move back to the centre of the imaginary square, and back to their start positions. This is all achieved with  the CSS3 transform: translate() property, and again some math is needed to calculate the positions in which they should move. Basic math, though. Here’s the CSS:

```css
.spinner-3 {
  height: 100px;
}
.spinner-3 .spinner {
  width: 100px;
  height: 100px;
}
.spinner-3 .spinner:before, .spinner-3 .spinner:after {
  display: block;
  position: absolute;
  top: 0;
  bottom: 0;
  margin: auto 0;
  width: 40px;
  height: 40px;
  content: "";
  border-radius: 20px;
}
.spinner-3 .spinner:before {
  left: 0;
  background-color: #28aadc;
  animation: spinner-3-left 1s infinite linear;
}
.spinner-3 .spinner:after {
  right: 0;
  background-color: #ff3c50;
  animation: spinner-3-right 1s infinite linear;
}

/* left ball */
@keyframes spinner-3-left {
  0% {
    transform: translate(0, 0);
  }

  25% {
    transform: translate(30px, 0);
  }

  50% {
    transform: translate(30px, -30px);
  }

  75% {
    transform: translate(30px, 0);
  }

  100% {
    transform: translate(0, 0);
  }
}

/* right ball */
@keyframes spinner-3-right {
  0% {
    transform: translate(0, 0);
  }

  25% {
    transform: translate(-30px, 0);
  }

  50% {
    transform: translate(-30px, 30px);
  }

  75% {
    transform: translate(-30px, 0);
  }

  100% {
    transform: translate(0, 0);
  }
}
```

## CSS Spinner/Loader \#4

![Spinner 4](/files/2015-03/spinner_4.png)

This spinner/loader starts of in a similar way to the previous one, consisting of two discs of the same size, on opposite ends of an imaginary square shape. This time though, they pulse. When one is small, the other is big, and vice versa. This time, we’ll use the transform: scale(); CSS3 property. Here’s what the CSS looks like:

```css
.spinner-4 {
  height: 100px;
}
.spinner-4 .spinner {
  width: 100px;
  height: 100px;
}
.spinner-4 .spinner:before, .spinner-4 .spinner:after {
  display: block;
  position: absolute;
  top: 0;
  bottom: 0;
  margin: auto 0;
  width: 50px;
  height: 50px;
  content: "";
  border-radius: 25px;
}
.spinner-4 .spinner:before {
  left: 0;
  background-color: #28aadc;
  animation: spinner-4-left 0.5s infinite ease;
}
.spinner-4 .spinner:after {
  right: 0;
  background-color: #ff3c50;
  animation: spinner-4-right 0.5s infinite ease;
}

/* left ball */
@keyframes spinner-4-left {
  0% {
    transform: scale(0.5);
  }

  50% {
    transform: scale(1);
  }

  100% {
    transform: scale(0.5);
  }
}

/* right ball */
@keyframes spinner-4-right {
  0% {
    transform: scale(1);
  }

  50% {
    transform: scale(0.5);
  }

  100% {
    transform: scale(1);
  }
}
```

## Sass To The Rescue

[Sass/SCSS](http://sass-lang.com/) can save us a great deal here if we want to change sizes, edit colours, and the works in our spinners. I personally LOVE Sass/SCSS, and originally did all these spinners in SCSS. I presented this tutorial in regular CSS for those of you not into Sass/SCSS just yet. But if you are, then I’ve included the SCSS files in the source folder. I was able to create a base variable for each loader, and base the other sizes/variables off of that. By modifying one or two variables (as opposed to a lot, plus math recalculation, plus changing for each vendor prefix), I can size up/down my entire spinner. Neat! Either way, once you get your CSS/Sass/SCSS set up, it’s not difficult to change. It’s not like you’re gonna have 10 different spinners in one project.

## What About Compatibility?

This tutorial and these spinners are heavily based on CSS3 transitions, transforms, and animations. Compatibility for these properties is increasing, and IE10 and up supports all 3. Of course, it’s a matter of throwing in a good old GIF in an IE9 and below specific stylesheet for a graceful fallback.

## Wrap Up

I highly recommend getting into this modern way of thinking, and breaking the shackles of age-old animated GIF’s. It might seem like a lot of CSS for one tiny little loader, but the truth is, every detail counts these days. When you compress the amount of CSS required for a loader, it’s going to be smaller than the size of a GIF. Include it in your already existent stylesheet and you’re on the ball. It’s scalable, expandable, colourable, modifiable, and just plain ol’ cool looking. I hope this tutorial gave you some insight into the world of CSS animations, transforms, and creating infinite keyframe loops. Try creating your own, feel free to download the source code by clicking the links below, and leave some feedback below. Thanks for reading, and I hope you learned something!

<p class="text-align--center">
<a href="http://callmenick.com/_development/css-loaders-and-spinners/css-loaders-and-spinners-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/css-loaders-and-spinners/" class="button button--inline-block button--medium">View Demo</a>
</p>