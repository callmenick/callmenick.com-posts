<p class="text-align--center">
<a href="http://callmenick.com/_development/css-loaders-and-spinners-2/css-loaders-and-spinners-2-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/css-loaders-and-spinners-2" class="button button--inline-block button--medium">View Demo</a>
</p>

## Introduction

This tutorial and demo is a follow up and addition to my first set of CSS spinners and loaders, which you can [see here](http://callmenick.com/2014/04/25/css-spinners-loaders/). In that tutorial, I spoke about the progressive and modern application of CSS spinners and loaders, and touched on their ease of editing and scaling. Using some basic CSS to construct shapes, and some more advanced CSS3 animations and keyframes for some movement, we can created beautiful, scalable, and easily modifiable CSS spinners and loaders.

In this tutorial, I'm going to demonstrate four more spinners and loaders. This time, they will take on a more sleek, sophisticated aesthetic. Just as before, you can view the CSS working versions of all these by clicking the "view demo" button above. The images presented in each section should give you a representation of how that spinner works. Let’s dig right in.

## Common HTML & CSS

Just as before, all our spinners and loaders are single-element objects, meaning only one element is required in the markup. For each demo, we'll also be relying heavily on the pseudo classes `::before` and `::after`, so I recommend [brushing up on your knowledge](http://callmenick.com/2014/04/13/under-the-radar-css-selectors/) of these. Each spinner will reside inside a spinner container just for the sake of this tutorial. You may want to apply your own positioning to the spinner in your project. Here's a look at the common HTML:

```html
<div class="spinner-container">
  <div class="cmn-spinner__radar">Loading...</div>
</div>

<div class="spinner-container">
  <div class="cmn-spinner__multisquare">Loading...</div>
</div>

<div class="spinner-container">
  <div class="cmn-spinner__multiball">Loading...</div>
</div>

<div class="spinner-container">
  <div class="cmn-spinner__squarediamond">Loading...</div>
</div>
```

Now here's a look at the common CSS:

```css
/* ============================================================
  SPINNER CONTAINER
============================================================ */
.spinner-container {
  padding: 20px 0;
}

/* ============================================================
  SPINNER - COMMON
============================================================ */
[class*="cmn-spinner"] {
  position: relative;
  margin: 0 auto;
  width: 100px;
  height: 100px;
  color: rgba(0, 0, 0, 0);
  font-size: 0;
  text-indent: -99999px;
}
```

The width and height of the spinner can be changed here easily, and no subsequent CSS will have to be edited. This is because every style for each spinner is relative to the parent container. For browsers that don't support pseudo classes or animations, you may want to reset the `text-indent`, `font-size`, and `color` properties so that those users see the "Loading..." text. Now, let's look at each spinner.

## Spinner 1 - Radar

![Radar Spinner](/files/2015-03/spinner_multidisc.png)

This spinner takes on a radar-type look, except each of the quarter-circles are spinning at different speeds, giving a really cool effect of trails chasing each other. Here's the CSS:

```css
.cmn-spinner__radar {
  border-top: solid 2px #96ff96;
  border-right: solid 2px transparent;
  border-bottom: solid 2px transparent;
  border-left: solid 2px transparent;
  border-radius: 100%;
  animation: cmn-spinner--animation__radar 1.5s infinite linear;
}
.cmn-spinner__radar::before, .cmn-spinner__radar::after {
  display: block;
  position: absolute;
  border-right: solid 2px transparent;
  border-bottom: solid 2px transparent;
  border-left: solid 2px transparent;
  border-radius: 100%;
  content: "";
}
.cmn-spinner__radar::before {
  top: 12px;
  right: 12px;
  bottom: 12px;
  left: 12px;
  border-top: solid 2px #dcff64;
  animation: cmn-spinner--animation__radar 1.75s infinite linear;
}
.cmn-spinner__radar::after {
  top: 26px;
  right: 26px;
  bottom: 26px;
  left: 26px;
  border-top: solid 2px #fff;
  animation: cmn-spinner--animation__radar 2s infinite linear;
}

/* animation keyframes */
@keyframes cmn-spinner--animation__radar {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
```

## Spinner 2 - Multi Square

![Radar Spinner](/files/2015-03/spinner_multisquare.png)

I called this spinner the "multi square" spinner. It entails two squares moving clockwise around each other. They don't move on a circular path though. Instead, they move in either horizontal or vertical segments, depending on the position. Each square starts in a separate position and requires its own animation sequence, so there are two sets of keyframe syntax. Here's the CSS:

```css
/* ============================================================
  CMN-SPINNER__MULTISQUARE
============================================================ */
.cmn-spinner__multisquare::before, .cmn-spinner__multisquare::after {
  display: block;
  position: absolute;
  width: 48%;
  height: 48%;
  content: "";
}
.cmn-spinner__multisquare::before {
  top: 0;
  left: 0;
  border: solid 2px #96ff96;
  animation: cmn-spinner--animation__multisquare1 1.5s infinite;
}
.cmn-spinner__multisquare::after {
  bottom: 0;
  right: 0;
  border: solid 2px #dcff64;
  animation: cmn-spinner--animation__multisquare2 1.5s infinite;
}

/* animation keyframes - square 1 */
@keyframes cmn-spinner--animation__multisquare1 {
  0% {
    top: 0;
    left: 0;
  }

  25% {
    top: 0;
    left: 52%;
  }

  50% {
    top: 52%;
    left: 52%;
  }

  75% {
    top: 52%;
    left: 0;
  }

  100% {
    top: 0;
    left: 0;
  }
}

/* animation keyframes - square 2 */
@keyframes cmn-spinner--animation__multisquare2 {
  0% {
    bottom: 0;
    right: 0;
  }

  25% {
    bottom: 0;
    right: 52%;
  }

  50% {
    bottom: 52%;
    right: 52%;
  }

  75% {
    bottom: 52%;
    right: 0;
  }

  100% {
    bottom: 0;
    right: 0;
  }
}
```

## Spinner 3 - Multi Ball

![Radar Spinner](/files/2015-03/spinner_multidisc.png)

The multi ball spinner consists of two "balls". The balls are really just squares with a 100% border radius, making them look circular. The two balls exists because of the two pseudo classes available for us, and they sit on opposite ends of a bigger circular path (the actual `div`). The entire element spins in a start-stop motion, giving the perception that the two balls are spinning around each other. Here's the CSS:

```css
/* ============================================================
  CMN-SPINNER__MULTIBALL
============================================================ */
.cmn-spinner__multiball {
  animation: cmn-spinner--animation__multiball 1.5s infinite;
}
.cmn-spinner__multiball::before, .cmn-spinner__multiball::after {
  display: block;
  position: absolute;
  top: 0;
  bottom: 0;
  margin: auto 0;
  width: 48%;
  height: 48%;
  border-radius: 100%;
  content: "";
}
.cmn-spinner__multiball::before {
  left: 0;
  border: solid 2px #96ff96;
}
.cmn-spinner__multiball::after {
  right: 0;
  border: solid 2px #dcff64;
}

/* animation keyframs */
@keyframes cmn-spinner--animation__multiball {
  0% {
    transform: rotate(0);
  }

  40% {
    transform: rotate(180deg);
  }

  50% {
    transform: rotate(180deg);
  }

  90% {
    transform: rotate(360deg);
  }

  100% {
    transform: rotate(360deg);
  }
}
```

## Spinner 4 - Square-Diamond

![Radar Spinner](/files/2015-03/spinner_squarediamond.png)

The final spinner in this roundup is called the square-diamond spinner. This spinner consists of a smaller diamond inside a bigger square. The diamond spins and expands while the square spins and shrinks, until the square sits inside the diamond. It then pauses for a brief moment, and spins back into starting position. Each element starts and ends up in different positions, so there are two sets of keyframe syntax. Here's the CSS:

```css
/* ============================================================
  CMN-SPINNER__SQUAREDIAMOND
============================================================ */
.cmn-spinner__squarediamond::before, .cmn-spinner__squarediamond::after {
  display: block;
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  margin: auto;
  content: "";
}
.cmn-spinner__squarediamond::before {
  width: 100%;
  height: 100%;
  border: solid 2px #96ff96;
  animation: cmn-spinner--animation__squarediamond1 2s infinite;
}
.cmn-spinner__squarediamond::after {
  width: 40%;
  height: 40%;
  border: solid 2px #dcff64;
  transform: rotate(45deg);
  animation: cmn-spinner--animation__squarediamond2 2s infinite;
}

/* animation keyframes - squarediamond1 */
@keyframes cmn-spinner--animation__squarediamond1 {
  0% {
    transform: rotate(0);
    width: 100%;
    height: 100%;
  }

  50% {
    transform: rotate(360deg);
    width: 40%;
    height: 40%;
  }

  100% {
    transform: rotate(720deg);
    width: 100%;
    height: 100%;
  }
}

/* animation keyframes - squarediamond2 */
@keyframes cmn-spinner--animation__squarediamond2 {
  0% {
    transform: rotate(45deg);
    width: 40%;
    height: 40%;
  }

  50% {
    transform: rotate(405deg);
    width: 100%;
    height: 100%;
  }

  100% {
    transform: rotate(765deg);
    width: 40%;
    height: 40%;
  }
}
```

## Vendor Prefixing, Browser Support, etc.

For the sake of brevity, I didn't include any vendor prefixes in the CSS above. In the downloadable source code bundle though, all prefixes are there. As far as browser support goes, CSS3 animations, transforms, and transitions will need to be supported for these to work. Luckily, support is high (IE10 and up for animations), and fallbacks are easy as mentioned above. With a simple conditional style for IE10 and down, you can set `display: none` on all the pseudo classes, and reset the `text-indent`, `color`, and `font-size` properties to show the fallback "Loading..." text.

## Wrap Up

And that’s a wrap! Once again, we've seen some progressive techniques in action and created modern, scalable, beautiful, and reusable elements out of pure CSS. Don’t forget, you can view the demo and download the source by clicking the links below, and if you have and questions, comments, or feedback, you can also leave them below. Also, you can [check out this project on GitHub](https://github.com/callmenick/css-loaders-spinners-2). Thanks for reading!

<p class="text-align--center">
<a href="http://callmenick.com/_development/css-loaders-and-spinners-2/css-loaders-and-spinners-2-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/css-loaders-and-spinners-2" class="button button--inline-block button--medium">View Demo</a>
</p>