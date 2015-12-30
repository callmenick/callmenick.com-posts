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
<button class="c-hamburger c-hamburger--rot">
  <span>toggle menu</span>
</button>

<button class="c-hamburger c-hamburger--htx">
  <span>toggle menu</span>
</button>

<button class="c-hamburger c-hamburger--htla">
  <span>toggle menu</span>
</button>

<button class="c-hamburger c-hamburger--htra">
  <span>toggle menu</span>
</button>
```

OK, let’s look at the common CSS which will be used across all buttons.

## Common CSS

For starters, I’ve reset all the button styles so that no shadows, borders, and default appearances are visible. I’ve also defined a width and height of the button (which will come in handy for the animation parts), and I’ve hidden the text. Here’s the CSS for the parent `cmn-toggle-switch` class:

```css
.c-hamburger {
  display: block;
  position: relative;
  overflow: hidden;
  margin: 0;
  padding: 0;
  width: 96px;
  height: 96px;
  font-size: 0;
  text-indent: -9999px;
  appearance: none;
  box-shadow: none;
  border-radius: none;
  border: none;
  cursor: pointer;
  transition: background 0.3s;
}

.c-hamburger:focus {
  outline: none;
}
```

Now, the inner `span` tag is actually home to the hamburger bars. We need three bars, so I’ll use the tag itself, and its pseudo elements `::before` and `::after`. The positioning of each bar (and the `span` tag itself) is all mathematically calculated based on the dimensions of the parent `button`. If you’re a Sass user like myself, then this is very easy and you’ll find all the Sass and variables for editing in the source code. If not, you’ll have to think a little harder and do more math if you want to achieve different sizes than the demos below. Here’s the CSS for the inner spans:

```css
.c-hamburger span {
  display: block;
  position: absolute;
  top: 44px;
  left: 18px;
  right: 18px;
  height: 8px;
  background: white;
}

.c-hamburger span::before,
.c-hamburger span::after {
  position: absolute;
  display: block;
  left: 0;
  width: 100%;
  height: 8px;
  background-color: #fff;
  content: "";
}

.c-hamburger span::before {
  top: -20px;
}

.c-hamburger span::after {
  bottom: -20px;
}
```

Now, we have our hamburger icon! Let’s jump into each of our examples now.

## Style 1 – Rotating Icon

![Style 1](/files/2015-02/style1.png)

This is the easiest of the icons, as all we’re doing is rotating the icon when it is active. Here’s the CSS:

```css
.c-hamburger--rot {
  background-color: #28aadc;
}

.c-hamburger--rot span {
  transition: transform 0.3s;
}

/* active state, i.e. menu open */
.c-hamburger--rot.is-active {
  background-color: #166888;
}

.c-hamburger--rot.is-active span {
  transform: rotate(90deg);
}
```

## Style 2 – Animate To “X”

![Style 2](/files/2015-02/style2.png)

This version of the hamburger icon animates it to an “x” by moving the top and bottom bars to the vertical-center of the icon, then rotating them. I used transition delays to achieve the overall effect, as I wanted the transform to the “x” to occur after the top and bottom bars moved to the middle. Here’s the CSS:

```css
.c-hamburger--htx {
  background-color: #ff3264;
}

.c-hamburger--htx span {
  transition: background 0s 0.3s;
}

.c-hamburger--htx span::before,
.c-hamburger--htx span::after {
  transition-duration: 0.3s, 0.3s;
  transition-delay: 0.3s, 0s;
}

.c-hamburger--htx span::before {
  transition-property: top, transform;
}

.c-hamburger--htx span::after {
  transition-property: bottom, transform;
}

/* active state, i.e. menu open */
.c-hamburger--htx.is-active {
  background-color: #cb0032;
}

.c-hamburger--htx.is-active span {
  background: none;
}

.c-hamburger--htx.is-active span::before {
  top: 0;
  transform: rotate(45deg);
}

.c-hamburger--htx.is-active span::after {
  bottom: 0;
  transform: rotate(-45deg);
}

.c-hamburger--htx.is-active span::before,
.c-hamburger--htx.is-active span::after {
  transition-delay: 0s, 0.3s;
}
```

## Style 3 – Animate To Left-Pointing Arrow

![Style 3](/files/2015-02/style3.png)

In this version of the icon, the whole thing rotates 180 degrees, and the top and bottom bars animate to form a left-pointing arrow. Here’s the CSS:

```css
.c-hamburger--htla {
  background-color: #32dc64;
}

.c-hamburger--htla span {
  transition: transform 0.3s;
}

.c-hamburger--htla span::before {
  transform-origin: top right;
  transition: transform 0.3s, width 0.3s, top 0.3s;
}

.c-hamburger--htla span::after {
  transform-origin: bottom right;
  transition: transform 0.3s, width 0.3s, bottom 0.3s;
}

/* active state, i.e. menu open */
.c-hamburger--htla.is-active {
  background-color: #18903c;
}

.c-hamburger--htla.is-active span {
  transform: rotate(180deg);
}

.c-hamburger--htla.is-active span::before,
.c-hamburger--htla.is-active span::after {
  width: 50%;
}

.c-hamburger--htla.is-active span::before {
  top: 0;
  transform: translateX(38px) translateY(4px) rotate(45deg);
}

.c-hamburger--htla.is-active span::after {
  bottom: 0;
  transform: translateX(38px) translateY(-4px) rotate(-45deg);
}
```

## Style 4 – Animate To Right-Pointing Arrow

![Style 4](/files/2015-02/style4.png)

This is very similar to the above version, except the arrow ends up pointing the other way (i.e. to the right). Here’s the CSS:

```css
.c-hamburger--htra {
  background-color: #ff9650;
}

.c-hamburger--htra span {
  transition: transform 0.3s;
}

.c-hamburger--htra span::before {
  transform-origin: top left;
  transition: transform 0.3s, width 0.3s, top 0.3s;
}

.c-hamburger--htra span::after {
  transform-origin: bottom left;
  transition: transform 0.3s, width 0.3s, bottom 0.3s;
}

/* active state, i.e. menu open */
.c-hamburger--htra.is-active {
  background-color: #e95d00;
}

.c-hamburger--htra.is-active span {
  transform: rotate(180deg);
}

.c-hamburger--htra.is-active span::before,
.c-hamburger--htra.is-active span::after {
  width: 50%;
}

.c-hamburger--htra.is-active span::before {
  top: 0;
  transform: translateX(-8px) translateY(4px) rotate(-45deg);
}

.c-hamburger--htra.is-active span::after {
  bottom: 0;
  transform: translateX(-8px) translateY(-4px) rotate(45deg);
}
```

## A Bit Of JavaScript

All of this is a bit useless if there’s no way to see the actual active versions of the icons, so here’s some JavaScript to achieve that:

```javascript
(function() {

  "use strict";

  var toggles = document.querySelectorAll(".c-hamburger");

  for (var i = toggles.length - 1; i >= 0; i--) {
    var toggle = toggles[i];
    toggleHandler(toggle);
  };

  function toggleHandler(toggle) {
    toggle.addEventListener( "click", function(e) {
      e.preventDefault();
      (this.classList.contains("is-active") === true) ? this.classList.remove("is-active") : this.classList.add("is-active");
    });
  }

})();
```

Of course, I’m iterating over all the icons. In your app however, you’ll likely have only one icon, so please edit the JS to suit.

## Sass To The Rescue

As I mentioned above, if you’re using Sass, it’ll make your life a whole lot easier. In the source code, all the Sass variables are set up making it very easy to edit the size of the button, thickness of the hamburger bars, and padding between the parent button and the inner span. It really saves a whole lot of time! Here's the Sass block that you can configure:

```scss
$button-width: 96px;                    // The width of the button area
$button-height: 96px;                   // The height of the button area
$bar-thickness: 8px;                    // The thickness of the button bars
$button-pad: 18px;                      // The left/right padding between button area and bars.
$button-bar-space: 12px;                // The spacing between button bars
$button-transistion-duration: 0.3s;     // The transition duration
```

## Wrap Up

And that’s a wrap! Don’t forget, you can view the demo and download the source by clicking the links below, and if you have and questions, comments, or feedback, you can also leave them below.

<p class="text-align--center">
<a href="https://github.com/callmenick/Animating-Hamburger-Icons" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/css-hamburger-menu-icons/" class="button button--inline-block button--medium">View Demo</a>
</p>