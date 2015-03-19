<p class="text-align--center">
<a href="http://callmenick.com/_development/advanced-parallax-effect/advanced-parallax-effect-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/advanced-parallax-effect" class="button button--inline-block button--medium">View Demo</a>
</p>

## Introduction

Previously, [we looked at a simple implementation of parallax scrolling](http://callmenick.com/2014/07/15/simple-parallax-scrolling-effect/). In that tutorial, the background remained fixed while the foreground scrolled as normal. We used only CSS to achieve this, covering the available space with big and bold background images. In this tutorial, we'll look into achieving a more true-to-life representation of parallax scrolling. As defined previously, parallax is an effect where the position of an object seems to be different when viewed from different positions. In this tutorial, our background will move at a slower speed than our foreground. Without further ado, let's get started!

##  The Markup & Structure

Our markup is the exact same as before. I'll rewrite it here to keep it all together. Each `section` contains either content or a background-image/heading combo. Here it is:

```html
<section class="module parallax parallax-1">
  <div class="container">
    <h1>Motion</h1>
  </div>
</section>

<section class="module content">
  <div class="container">
    <h2>Lorem Ipsum Dolor</h2>
    <p>Lorem ipsum dolor...</p>
  </div>
</section>

<section class="module parallax parallax-2">
  <div class="container">
    <h1>Shape</h1>
  </div>
</section>

<section class="module content">
  <div class="container">
    <h2>Lorem Ipsum Dolor</h2>
    <p>Lorem ipsum dolor...</p>
  </div>
</section>

<section class="module parallax parallax-3">
  <div class="container">
    <h1>Colour</h1>
  </div>
</section>

<section class="module content">
  <div class="container">
    <h2>Lorem Ipsum Dolor</h2>
    <p>Lorem ipsum dolor...</p>
  </div>
</section>
```

Each `section` with a class of `parallax` will contain our background images and heading text, while each `section` with a class of `content` will be simple content-containing sections. The `container` class is a fluid div with a maximum width, making the responsiveness of it all very simple. Now, let’s dig into the CSS, but first, a side-note on our background image selection for this version.

## Side Note - Background Image Selection

In the simple parallax tutorial, we used big background images and had them cover the available space. This worked fine, because the images remained fixed and never repeated. However, in this case, the images are actually scrolling (even though at different speeds), so the images have to repeat, otherwise they will just scroll off screen. If we use the same big images, there is no guarantee that the image will always start when that container hits the viewport. It will work fine, but we will see the line where the image repeats itself. To avoid this, it's advisable to use repeating patterns or textures. I used repeating patterns in this tutorial, so the background always joins up seamlessly and scrolls infinitely. Now, onto the CSS!

## The CSS

Our CSS in this case will be slightly different. We won't have fixed heights for the parallax containers, and we won't have our background images covering the entire sections. Instead, we'll have repeating background images, and we'll pad the parallax sections. Here's the CSS, with some sample media queries included:

```css
/* ============================================================
  PRIMARY STRUCTURE
============================================================ */
.container {
  max-width: 960px;
  margin: 0 auto;
}

/* ============================================================
  SECTIONS
============================================================ */
section.module:last-child {
  margin-bottom: 0;
}
section.module h2 {
  margin-bottom: 40px;
  font-family: "Roboto Slab", serif;
  font-size: 30px;
}
section.module p {
  margin-bottom: 40px;
  font-size: 16px;
  font-weight: 300;
}
section.module p:last-child {
  margin-bottom: 0;
}
section.module.content {
  padding: 40px 0;
}
section.module.parallax {
  padding: 240px 0;
  background-position: 0 0;
}
section.module.parallax h1 {
  color: #1e1e1e;
  font-size: 48px;
  line-height: 1;
  font-weight: 700;
  text-align: center;
  text-transform: uppercase;
  text-shadow: 0 0 10px white;
}
section.module.parallax-1 {
  background-image: url("../img/demo/pattern1.png");
}
section.module.parallax-2 {
  background-image: url("../img/demo/pattern2.png");
}
section.module.parallax-3 {
  background-image: url("../img/demo/pattern3.png");
}

@media all and (min-width: 600px) {
  section.module h2 {
    font-size: 42px;
  }
  section.module p {
    font-size: 20px;
  }
  section.module.parallax {
    padding: 300px 0;
  }
  section.module.parallax h1 {
    font-size: 96px;
  }
}
@media all and (min-width: 960px) {
  section.module.parallax h1 {
    font-size: 160px;
  }
}
```

As it stands, everything will scroll as normal. We don't want this though. We want our backgrounds inside our parallax sections to scroll at a slower speed. How do we achieve this? JavaScript! Let's take a look.

## Variable Scroll Speeds With JavaScript

For each parallax section, we'll intercept the background position and alter it to be 50% lower than it would normally be on each scroll. For example, if we scrolled 10px, the new background position relative to where it was before would be 5px. This fraction can be changed easily in our JavaScript, allowing us to change the apparent scroll speed of the background to whatever we want. Here's a look at the JavaScript:

```javascript
(function(){

  var parallax = document.querySelectorAll(".parallax"),
      speed = 0.5;

  window.onscroll = function(){
    [].slice.call(parallax).forEach(function(el,i){

      var windowYOffset = window.pageYOffset,
          elBackgrounPos = "50% " + (windowYOffset * speed) + "px";
      
      el.style.backgroundPosition = elBackgrounPos;

    });
  };

})();
```

Let's take a step by step look at exactly what's going on here.

1. We first find all our sections with a class of `.parallax`.
2. We then set our speed as a fraction between 0 and 1. In this case, I used 0.5.
3. On scroll, we loop through each of our parallax sections, finding the y offset of the window.
4. We then adjust the `background-position` property of our parallax section accordingly, based on our speed fraction.
5. The position of our background image is now updated each time a scroll occurs, giving the impression that it is scrolling at a slower speed than the foreground elements.

Voila! Simple, effective, and easy to play around with.

## Wrap Up

In this tutorial, we looked at a from-scratch implementation of an advanced parallax scrolling technique. With some simple CSS manipulation via JavaScript, we've given the user a feeling of variable speed scrolling. And that’s a wrap! Don’t forget, you can view the demo and download the source by clicking the links below, and if you have and questions, comments, or feedback, you can also leave them below.

<p class="text-align--center">
<a href="http://callmenick.com/_development/advanced-parallax-effect/advanced-parallax-effect-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/advanced-parallax-effect" class="button button--inline-block button--medium">View Demo</a>
</p>