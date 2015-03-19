<p class="text-align--center">
<a href="https://github.com/callmenick/Animating-Hamburger-Icons" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/css-hamburger-menu-icons/" class="button button--inline-block button--medium">View Demo</a>
</p>

## Intro

Hamburger menu icons have become a staple in many websites and web apps, and whether you like them or not, they are becoming a familiar and recognizable UI action button. Users are associating this icon with the showing and hiding of menus, and its compact, space-saving nature makes it a desirable style, particularly on smaller screens. In the past, you may have created or downloaded an icon and dropped it into your document. This is all well and good, and if you’re using image sprites, then you’re on course for an easy implementation.

Today though, I’m going to show you how to create a CSS-only version of the famous hamburger menu icon. With mobile experiences and web animation coming of age, and the dawn of design specs like Google’s material design, users are also expecting a more interactive experience. In the demos, we’ll create some neat animations for our icons, indicating that they are “active”, or that perhaps a menu is open. I’ll use a teeny little bit of JavaScript to make that happen. Alright, let’s dig in!

## Common Markup

I’ll be creating 4 different demos. The markup for each button is fairly similar, and each button has one common class and one unique class. Each button consists of a parent `button` tag, with an inner `span` tag. The parent tag will play container to the icon, and allow us to add padding and background colors to the icon. The `span` tag will actually play home to the "hamburger bars". Here’s the markup:

```html
<button class="cmn-toggle-switch cmn-toggle-switch__rot">
  <span>toggle menu</span>
</button>

<button class="cmn-toggle-switch cmn-toggle-switch__htx">
  <span>toggle menu</span>
</button>

<button class="cmn-toggle-switch cmn-toggle-switch__htla">
  <span>toggle menu</span>
</button>

<button class="cmn-toggle-switch cmn-toggle-switch__htra">
  <span>toggle menu</span>
</button>
```

OK, let’s look at the common CSS which will be used across all buttons.

## Common CSS

For starters, I’ve reset all the button styles so that no shadows, borders, and default appearances are visible. I’ve also defined a width and height of the button (which will come in handy for the animation parts), and I’ve hidden the text. Here’s the CSS for the parent `cmn-toggle-switch` class:

```css
.cmn-toggle-switch {
  display: block;
  position: relative;
  overflow: hidden;
  margin: 0;
  padding: 0;
  width: 108px;
  height: 96px;
  font-size: 0;
  text-indent: -9999px;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  box-shadow: none;
  border-radius: none;
  border: none;
  cursor: pointer;
  -webkit-transition: background 0.3s;
          transition: background 0.3s;
}

.cmn-toggle-switch:focus {
  outline: none;
}
```

Now, the inner `span` tag is actually home to the hamburger bars. We need three bars, so I’ll use the tag itself, and its pseudo elements `::before` and `::after`. The positioning of each bar (and the `span` tag itself) is all mathematically calculated based on the dimensions of the parent `button`. If you’re a Sass user like myself, then this is very easy and you’ll find all the Sass and variables for editing in the source code. If not, you’ll have to think a little harder and do more math if you want to achieve different sizes than the demos below. Here’s the CSS for the inner spans:

```css
.cmn-toggle-switch span {
  display: block;
  position: absolute;
  top: 45px;
  left: 18px;
  right: 18px;
  height: 6px;
  background: white;
}

.cmn-toggle-switch span::before,
.cmn-toggle-switch span::after {
  position: absolute;
  display: block;
  left: 0;
  width: 100%;
  height: 6px;
  background-color: #fff;
  content: "";
}

.cmn-toggle-switch span::before {
  top: -27px;
}

.cmn-toggle-switch span::after {
  bottom: -27px;
}
```

Now, we have our hamburger icon! Let’s jump into each of our examples now.

## Style 1 – Rotating Icon

![Style 1](/files/2015-02/style1.png)

This is the easiest of the icons, as all we’re doing is rotating the icon when it is active. Here’s the CSS:

```css
/**
 * Style 1
 *
 * Rotating hamburger icon (rot), that simply rotates 90 degrees when activated.
 * Nothing too fancy, simple transition.
 */
.cmn-toggle-switch__rot {
  background-color: #28aadc;
}

.cmn-toggle-switch__rot span {
  -webkit-transition: -webkit-transform 0.3s;
          transition: transform 0.3s;
}

/* active state, i.e. menu open */
.cmn-toggle-switch__rot.active {
  background-color: #166888;
}

.cmn-toggle-switch__rot.active span {
  -webkit-transform: rotate(90deg);
      -ms-transform: rotate(90deg);
          transform: rotate(90deg);
}
```

## Style 2 – Animate To “X”

![Style 2](/files/2015-02/style2.png)

This version of the hamburger icon animates it to an “x” by moving the top and bottom bars to the vertical-center of the icon, then rotating them. I used transition delays to achieve the overall effect, as I wanted the transform to the “x” to occur after the top and bottom bars moved to the middle. Here’s the CSS:

```css
/**
 * Style 2
 * 
 * Hamburger to "x" (htx). Takes on a hamburger shape, bars slide
 * down to center and transform into an "x".
 */
.cmn-toggle-switch__htx {
  background-color: #ff3264;
}

.cmn-toggle-switch__htx span {
  -webkit-transition: background 0 0.3s;
          transition: background 0 0.3s;
}

.cmn-toggle-switch__htx span::before,
.cmn-toggle-switch__htx span::after {
  -webkit-transition-duration: 0.3s, 0.3s;
          transition-duration: 0.3s, 0.3s;
  -webkit-transition-delay: 0.3s, 0;
          transition-delay: 0.3s, 0;
}

.cmn-toggle-switch__htx span::before {
  -webkit-transition-property: top, -webkit-transform;
          transition-property: top, transform;
}

.cmn-toggle-switch__htx span::after {
  -webkit-transition-property: bottom, -webkit-transform;
          transition-property: bottom, transform;
}

/* active state, i.e. menu open */
.cmn-toggle-switch__htx.active {
  background-color: #cb0032;
}

.cmn-toggle-switch__htx.active span {
  background: none;
}

.cmn-toggle-switch__htx.active span::before {
  top: 0;
  -webkit-transform: rotate(45deg);
      -ms-transform: rotate(45deg);
          transform: rotate(45deg);
}

.cmn-toggle-switch__htx.active span::after {
  bottom: 0;
  -webkit-transform: rotate(-45deg);
      -ms-transform: rotate(-45deg);
          transform: rotate(-45deg);
}

.cmn-toggle-switch__htx.active span::before,
.cmn-toggle-switch__htx.active span::after {
  -webkit-transition-delay: 0, 0.3s;
          transition-delay: 0, 0.3s;
}
```

## Style 3 – Animate To Left-Pointing Arrow

![Style 3](/files/2015-02/style3.png)

In this version of the icon, the whole thing rotates 180 degrees, and the top and bottom bars animate to form a left-pointing arrow. Here’s the CSS:

```css
/**
 * Style 3
 *
 * Hamburger to left-arrow (htla). Hamburger menu transforms to a left-pointing
 * arrow. Usually indicates an off canvas menu sliding in from left that
 * will be close on re-click of the icon.
 */
.cmn-toggle-switch__htla {
  background-color: #32dc64;
}

.cmn-toggle-switch__htla span {
  -webkit-transition: -webkit-transform 0.3s;
          transition: transform 0.3s;
}

.cmn-toggle-switch__htla span::before {
  -webkit-transform-origin: top right;
      -ms-transform-origin: top right;
          transform-origin: top right;
  -webkit-transition: -webkit-transform 0.3s, width 0.3s, top 0.3s;
          transition: transform 0.3s, width 0.3s, top 0.3s;
}

.cmn-toggle-switch__htla span::after {
  -webkit-transform-origin: bottom right;
      -ms-transform-origin: bottom right;
          transform-origin: bottom right;
  -webkit-transition: -webkit-transform 0.3s, width 0.3s, bottom 0.3s;
          transition: transform 0.3s, width 0.3s, bottom 0.3s;
}

/* active state, i.e. menu open */
.cmn-toggle-switch__htla.active {
  background-color: #18903c;
}

.cmn-toggle-switch__htla.active span {
  -webkit-transform: rotate(180deg);
      -ms-transform: rotate(180deg);
          transform: rotate(180deg);
}

.cmn-toggle-switch__htla.active span::before,
.cmn-toggle-switch__htla.active span::after {
  width: 50%;
}

.cmn-toggle-switch__htla.active span::before {
  top: 0;
  -webkit-transform: translateX(42px) translateY(3px) rotate(45deg);
      -ms-transform: translateX(42px) translateY(3px) rotate(45deg);
          transform: translateX(42px) translateY(3px) rotate(45deg);
}

.cmn-toggle-switch__htla.active span::after {
  bottom: 0;
  -webkit-transform: translateX(42px) translateY(-3px) rotate(-45deg);
      -ms-transform: translateX(42px) translateY(-3px) rotate(-45deg);
          transform: translateX(42px) translateY(-3px) rotate(-45deg);
}
```

## Style 4 – Animate To Right-Pointing Arrow

![Style 4](/files/2015-02/style4.png)

This is very similar to the above version, except the arrow ends up pointing the other way (i.e. to the right). Here’s the CSS:

```css
/**
 * Style 4
 *
 * Hamburger to right-arrow (htra). Hamburger menu transforms to a
 * right-pointing arrow. Usually indicates an off canvas menu sliding in from 
 * right that will be close on re-click of the icon.
 */
.cmn-toggle-switch__htra {
  background-color: #ff9650;
}

.cmn-toggle-switch__htra span {
  -webkit-transition: -webkit-transform 0.3s;
          transition: transform 0.3s;
}

.cmn-toggle-switch__htra span::before {
  -webkit-transform-origin: top left;
      -ms-transform-origin: top left;
          transform-origin: top left;
  -webkit-transition: -webkit-transform 0.3s, width 0.3s, top 0.3s;
          transition: transform 0.3s, width 0.3s, top 0.3s;
}

.cmn-toggle-switch__htra span::after {
  -webkit-transform-origin: bottom left;
      -ms-transform-origin: bottom left;
          transform-origin: bottom left;
  -webkit-transition: -webkit-transform 0.3s, width 0.3s, bottom 0.3s;
          transition: transform 0.3s, width 0.3s, bottom 0.3s;
}

/* active state, i.e. menu open */
.cmn-toggle-switch__htra.active {
  background-color: #e95d00;
}

.cmn-toggle-switch__htra.active span {
  -webkit-transform: rotate(180deg);
      -ms-transform: rotate(180deg);
          transform: rotate(180deg);
}

.cmn-toggle-switch__htra.active span::before,
.cmn-toggle-switch__htra.active span::after {
  width: 50%;
}

.cmn-toggle-switch__htra.active span::before {
  top: 0;
  -webkit-transform: translateX(-6px) translateY(3px) rotate(-45deg);
      -ms-transform: translateX(-6px) translateY(3px) rotate(-45deg);
          transform: translateX(-6px) translateY(3px) rotate(-45deg);
}

.cmn-toggle-switch__htra.active span::after {
  bottom: 0;
  -webkit-transform: translateX(-6px) translateY(-3px) rotate(45deg);
      -ms-transform: translateX(-6px) translateY(-3px) rotate(45deg);
          transform: translateX(-6px) translateY(-3px) rotate(45deg);
}
```

## A Bit Of JavaScript

All of this is a bit useless if there’s no way to see the actual active versions of the icons, so here’s some JavaScript to achieve that:

```javascript
(function() {

  "use strict";

  var toggles = document.querySelectorAll(".cmn-toggle-switch");

  for (var i = toggles.length - 1; i >= 0; i--) {
    var toggle = toggles[i];
    toggleHandler(toggle);
  };

  function toggleHandler(toggle) {
    toggle.addEventListener( "click", function(e) {
      e.preventDefault();
      (this.classList.contains("active") === true) ? this.classList.remove("active") : this.classList.add("active");
    });
  }

})();
```

Of course, I’m iterating over all the icons. In your app however, you’ll likely have only one icon, so please edit the JS to suit.

## Sass To The Rescue

As I mentioned above, if you’re using Sass, it’ll make your life a whole lot easier. In the source code, all the Sass variables (only 4 of them) are set up making it very easy to edit the size of the button, thickness of the hamburger bars, and padding between the parent button and the inner span. It really saves a whole lot of time!

## Wrap Up

And that’s a wrap! Don’t forget, you can view the demo and download the source by clicking the links below, and if you have and questions, comments, or feedback, you can also leave them below.

<p class="text-align--center">
<a href="https://github.com/callmenick/Animating-Hamburger-Icons" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/css-hamburger-menu-icons/" class="button button--inline-block button--medium">View Demo</a>
</p>