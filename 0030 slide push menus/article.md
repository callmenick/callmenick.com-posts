<p class="text-align--center">
<a href="https://github.com/callmenick/Slide-Push-Menus" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/slide-push-menus/" class="button button--inline-block button--medium">View Demo</a>
</p>

## Getting Started

In this tutorial, we’re going to create some slide and push menus using CSS3 transitions. The menus will be hidden off the screen at first, and will transition into view when the “toggle menu” button is pressed. Let’s first define the difference in the two types of menus:

* A slide menu will slide in above the content when toggled, and the content will stay in place.
* A push menu will slide in and “push” the main content aside when toggled.

Before we proceed, there are some important things to note:

* This tutorial relies on CSS transitions to work smoothly. On older browsers though, the content will just jump to the appropriate position.
* I’m using classie.js for easily adding and removing classes.
* I’m using the JavaScript functions `querySelector` and `querySelectorAll` which are supported from IE8 and up.

Finally, let’s take a look at the 8 varieties of menu we will create:

* __Slide left menu,__ which slides in from the left above the content
* __Slide right menu,__ which slides in from the right above the content
* __Slide top menu,__ which slides in from the top above the content
* __Slide bottom menu,__ which slides in from the bottom above the content
* __Push left menu,__ which slides in from the left and pushes the content to the right
* __Push right menu,__ which slides in from the right and pushes the content to the left
* __Push top menu,__ which slides in from the top and pushes the content down
* __Push bottom menu,__ which slides in from the bottom and pushes the content up

When a menu is open, we’ll show a “mask” over the main wrapper. This is basically a semi-transparent overlay that hides the main content. When the user clicks the overlay, the menu will toggle back out of view. In each menu, there will also be a “close menu” button, which will help out when the menu takes up the full width of the screen on smaller screen sizes. Let’s first take a look at the general markup for the document, the markup for each of the menus, and CSS for all the menus.

## Document Markup

It's important to remember that because we sometimes want all the content to get nudged in a direction when a menu is "pushing" onto the screen, the content needs to be wrapped. Likewise, the menus need to reside outside this wrapper. Here's the general markup:

```html
<div id="o-wrapper" class="o-wrapper">
  
  <div class="c-buttons">
    <button id="c-button--slide-left" class="c-button">Slide Left</button>
    <button id="c-button--slide-right" class="c-button">Slide Right</button>
    <button id="c-button--push-left" class="c-button">Push Left</button>
    <button id="c-button--push-right" class="c-button">Push Right</button>
    <button id="c-button--slide-top" class="c-button">Slide Top</button>
    <button id="c-button--slide-bottom" class="c-button">Slide Botton</button>
    <button id="c-button--push-top" class="c-button">Push Top</button>
    <button id="c-button--push-bottom" class="c-button">Push Bottom</button>
  </div>

  <!-- other content in here -->
  
</div><!-- /o-wrapper -->

<!-- menus here -->

<div id="c-mask" class="c-mask"></div><!-- /c-mask -->
```

I've also included some buttons which will be useful later when we want to active menus via JavaScript. Let's look at the structure of the menus.

## The Markup For The Menus

Each menu will be governed by a master class of `c-menu`, with a respective modifier for styling customisations. All the menus will also have a corresponding ID for quick querying in the JavaScript. Here's the markup for each menu:

```html
<nav id="c-menu--slide-left" class="c-menu c-menu--slide-left">
  <button class="c-menu__close">&larr; Close Menu</button>
  <ul class="c-menu__items">
    <li class="c-menu__item"><a href="#" class="c-menu__link">Home</a></li>
    <li class="c-menu__item"><a href="#" class="c-menu__link">About</a></li>
    <li class="c-menu__item"><a href="#" class="c-menu__link">Services</a></li>
    <li class="c-menu__item"><a href="#" class="c-menu__link">Work</a></li>
    <li class="c-menu__item"><a href="#" class="c-menu__link">Contact</a></li>
  </ul>
</nav><!-- /c-menu slide-left -->

<nav id="c-menu--slide-right" class="c-menu c-menu--slide-right">
  <!-- ... -->
</nav><!-- /c-menu slide-right -->

<nav id="c-menu--push-left" class="c-menu c-menu--push-left">
  <!-- ... -->
</nav><!-- /c-menu push-left -->

<nav id="c-menu--push-right" class="c-menu c-menu--push-right">
  <!-- ... -->
</nav><!-- /c-menu push-right -->

<nav id="c-menu--slide-top" class="c-menu c-menu--slide-top">
  <!-- ... -->
</nav><!-- /c-menu slide-top -->

<nav id="c-menu--push-top" class="c-menu c-menu--push-top">
  <!-- ... -->
</nav><!-- /c-menu push-top -->

<nav id="c-menu--slide-bottom" class="c-menu c-menu--slide-bottom">
  <!-- ... -->
</nav><!-- /c-menu slide-bottom -->

<nav id="c-menu--push-bottom" class="c-menu c-menu--push-bottom">
  <!-- ... -->
</nav><!-- /c-menu push-bottom -->
```

I didn't write out the markup for every single menu, but the pattern is obvious here. Let's move onto the CSS!

## The Common CSS

First, let's take a look at the common CSS for all the menus and the document body:

```css
/**
 * Menu overview.
 */
.c-menu {
  position: fixed;
  z-index: 200;
  background-color: #67b5d1;
  transition: transform 0.3s;
}

.c-menu__items {
  list-style: none;
  margin: 0;
  padding: 0;
}

/**
 * Close button resets.
 */
.c-menu__close {
  color: #fff;
  background-color: #3184a1;
  font-size: 14px;
  border: none;
  box-shadow: none;
  border-radius: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  cursor: pointer;
}

/**
 * Close button resets.
 */
.c-menu__close:focus {
  outline: none;
}

/**
 * Body states.
 *
 * When a menu is active, we want to hide the overflows on the body to prevent
 * awkward document scrolling.
 */
body.has-active-menu {
  overflow: hidden;
}

/**
 * Mask component
 */
 
.c-mask {
  position: fixed;
  z-index: 100;
  top: 0;
  left: 0;
  overflow: hidden;
  width: 0;
  height: 0;
  background-color: #000;
  opacity: 0;
  transition: opacity 0.3s, width 0s 0.3s, height 0s 0.3s;
}

.c-mask.is-active {
  width: 100%;
  height: 100%;
  opacity: 0.7;
  transition: opacity 0.3s;
}
```

Each menu needs to be fixed in place and above all other content. Inside each menu, there will also be a close button, allowing users to close the menu. Also, we need to account for an active state, and hide content overflow on the body when a menu is active. This will prevent some awkward document scrolling when a menu is activated. Finally, we need to include some styles for the mask, allowing it to fade in and out when menus are activated and deactivated. Let's now move onto the various versions of the menus.

## 1 - Left & Right Menus

Menus coming in from the left and right of the screen will inherit a lot of the same styles, so let's group them together. First, we'll take a look at some of the common CSS:

```css
.c-menu--slide-left,
.c-menu--slide-right,
.c-menu--push-left,
.c-menu--push-right {
  width: 100%;
  height: 100%;
  overflow-y: scroll;
}

@media all and (min-width: 320px) {
  .c-menu--slide-left,
  .c-menu--slide-right,
  .c-menu--push-left,
  .c-menu--push-right {
    width: 300px;
  }
}

.c-menu--slide-left .c-menu__item,
.c-menu--slide-right .c-menu__item,
.c-menu--push-left .c-menu__item,
.c-menu--push-right .c-menu__item {
  display: block;
  text-align: center;
  border-top: solid 1px #b5dbe9;
  border-bottom: solid 1px #3184a1;
}

.c-menu--slide-left .c-menu__item:first-child,
.c-menu--slide-right .c-menu__item:first-child,
.c-menu--push-left .c-menu__item:first-child,
.c-menu--push-right .c-menu__item:first-child {
  border-top: none;
}

.c-menu--slide-left .c-menu__item:last-child,
.c-menu--slide-right .c-menu__item:last-child,
.c-menu--push-left .c-menu__item:last-child,
.c-menu--push-right .c-menu__item:last-child {
  border-bottom: none;
}

.c-menu--slide-left .c-menu__link,
.c-menu--slide-right .c-menu__link,
.c-menu--push-left .c-menu__link,
.c-menu--push-right .c-menu__link {
  display: block;
  padding: 12px 24px;
  color: #fff;
}

.c-menu--slide-left .c-menu__close,
.c-menu--slide-right .c-menu__close,
.c-menu--push-left .c-menu__close,
.c-menu--push-right .c-menu__close {
  display: block;
  padding: 12px 24px;
  width: 100%;
}
```

Take note of the width and height of the menus and how that changes depending on screen width. Also of importance is the overflow property on the menu itself, allowing users to scroll up and down the menu if there is a lot of content. Let's move on now to the sub-versions of the left and right menus.

### 1a - Slide & Push Left Menus

Slide and push left menus inherit the exact same styles, because the movement of them is the same. Here's the CSS:

```css
.c-menu--slide-left,
.c-menu--push-left {
  top: 0;
  left: 0;
  transform: translateX(-100%);
}

@media all and (min-width: 320px) {
  .c-menu--slide-left,
  .c-menu--push-left {
    transform: translateX(-300px);
  }
}

.c-menu--slide-left.is-active,
.c-menu--push-left.is-active {
  transform: translateX(0);
}
```

The menus start off screen initially, and when they are activated, they translate back to the start position. The animation occurs due to the transition property set earlier in the common CSS.

### 1b - Slide & Push Right Menus

These menus are the exact same as before, but in the opposite direction. Here's the CSS:

```css
.c-menu--slide-right,
.c-menu--push-right {
  top: 0;
  right: 0;
  transform: translateX(100%);
}

@media all and (min-width: 320px) {
  .c-menu--slide-right,
  .c-menu--push-right {
    transform: translateX(300px);
  }
}

.c-menu--slide-right.is-active,
.c-menu--push-right.is-active {
  transform: translateX(0);
}
```

This time, the menus start off the right of the screen, and transition into view like before. 

### Left & Right Push Menus - Wrapper States

We need to also account for the wrapper being pushed either to the left or right if a push menu is activated. Here's the CSS for that:

```css
.o-wrapper.has-push-left {
  transform: translateX(100%);
}

@media all and (min-width: 320px) {
  .o-wrapper.has-push-left {
    transform: translateX(300px);
  }
}

.o-wrapper.has-push-right {
  transform: translateX(-100%);
}

@media all and (min-width: 320px) {
  .o-wrapper.has-push-right {
    transform: translateX(-300px);
  }
}
```

Let's move on to the top and bottom menus!

## 1 - Top & Bottom Menus

Once again, menus coming in from the left and right of the screen will inherit a lot of the same styles, so let's group them together. Here's some common CSS for them:

```css
.c-menu--slide-top,
.c-menu--slide-bottom,
.c-menu--push-top,
.c-menu--push-bottom {
  vertical-align: middle;
  width: 100%;
  height: 60px;
  text-align: center;
  overflow-x: scroll;
}

.c-menu--slide-top .c-menu__items,
.c-menu--slide-bottom .c-menu__items,
.c-menu--push-top .c-menu__items,
.c-menu--push-bottom .c-menu__items {
  display: inline-block;
  text-align: center;
}

.c-menu--slide-top .c-menu__item,
.c-menu--slide-bottom .c-menu__item,
.c-menu--push-top .c-menu__item,
.c-menu--push-bottom .c-menu__item {
  display: inline-block;
  line-height: 60px;
}

.c-menu--slide-top .c-menu__link,
.c-menu--slide-bottom .c-menu__link,
.c-menu--push-top .c-menu__link,
.c-menu--push-bottom .c-menu__link {
  display: block;
  padding: 0 4px;
  color: #fff;
}

.c-menu--slide-top .c-menu__close,
.c-menu--slide-bottom .c-menu__close,
.c-menu--push-top .c-menu__close,
.c-menu--push-bottom .c-menu__close {
  display: inline-block;
  margin-right: 12px;
  padding: 0 24px;
  height: 60px;
  line-height: 60px;
}
```

The height of this menu is important this time, as it will account for how much the wrapper gets pushed up or down. Let's look at the individual sub-versions.

### 1a - Slide & Push Top Menus

Slide and push top menus inherit the exact same styles. Here's the CSS:

```css
.c-menu--slide-top,
.c-menu--push-top {
  top: 0;
  left: 0;
  transform: translateY(-60px);
}

.c-menu--slide-top.is-active,
.c-menu--push-top.is-active {
  transform: translateY(0);
}
```

These menus start off the top of the screen initially, and transition back into their default position when active. The translation is based on the height of the menu this time.

### 1b - Slide & Push Bottom Menus

Now let's look at the menus coming in from the bottom. Here's the CSS for those:

```css
.c-menu--slide-bottom,
.c-menu--push-bottom {
  bottom: 0;
  left: 0;
  transform: translateY(60px);
}

.c-menu--slide-bottom.is-active,
.c-menu--push-bottom.is-active {
  transform: translateY(0);
}
```

These are very similar to the ones above, but this time, they start off the bottom of the screen. 

### Top & Bottom Push Menus - Wrapper States

Once again, we need to account for wrapper movement when the top and bottom menus get pushed onto the screen. Here's the CSS:

```css
.o-wrapper.has-push-top {
  transform: translateY(60px);
}

.o-wrapper.has-push-bottom {
  transform: translateY(-60px);
}
```

Nice! Now, you might be wondering how it all works. With a bit of JavaScript of course! Let's create our JavaScript component, and then write a little script to activate the menus.

## The JavaScript

In our JavaScript component, we want to achieve a few things:

1. We want to, at any point, be able to create a new instance of a slide or push menu. For that, we will need a constructor function. 
2. We want our menu to take options as well. The options will allow you to decide which type of menu they want to use upon instantiation, amongst other things.
3. When a new instance of a menu is created, we want to be able to open and close that menu, and we also need to listen for events on the close button and the mask. These two events will close the menu.
4. When a menu is open, we want to disable all buttons that can toggle new menus. For this, we rely on the `menuOpenerClass` option, which holds the class name set on all buttons that can be used to call the open function of a menu instance.
5. Finally, we need to add our component to the global namespace so that we can access it anywhere.

Here's the JavaScript, including two helper functions:

```javascript
(function(window) {

  'use strict';

  /**
   * Extend Object helper function.
   */
  function extend(a, b) {
    for(var key in b) { 
      if(b.hasOwnProperty(key)) {
        a[key] = b[key];
      }
    }
    return a;
  }

  /**
   * Each helper function.
   */
  function each(collection, callback) {
    for (var i = 0; i < collection.length; i++) {
      var item = collection[i];
      callback(item);
    }
  }

  /**
   * Menu Constructor.
   */
  function Menu(options) {
    this.options = extend({}, this.options);
    extend(this.options, options);
    this._init();
  }

  /**
   * Menu Options.
   */
  Menu.prototype.options = {
    wrapper: '#o-wrapper',          // The content wrapper
    type: 'slide-left',             // The menu type
    menuOpenerClass: '.c-button',   // The menu opener class names (i.e. the buttons)
    maskId: '#c-mask'               // The ID of the mask
  };

  /**
   * Initialise Menu.
   */
  Menu.prototype._init = function() {
    this.body = document.body;
    this.wrapper = document.querySelector(this.options.wrapper);
    this.mask = document.querySelector(this.options.maskId);
    this.menu = document.querySelector('#c-menu--' + this.options.type);
    this.closeBtn = this.menu.querySelector('.c-menu__close');
    this.menuOpeners = document.querySelectorAll(this.options.menuOpenerClass);
    this._initEvents();
  };

  /**
   * Initialise Menu Events.
   */
  Menu.prototype._initEvents = function() {
    // Event for clicks on the close button inside the menu.
    this.closeBtn.addEventListener('click', function(e) {
      e.preventDefault();
      this.close();
    }.bind(this));

    // Event for clicks on the mask.
    this.mask.addEventListener('click', function(e) {
      e.preventDefault();
      this.close();
    }.bind(this));
  };

  /**
   * Open Menu.
   */
  Menu.prototype.open = function() {
    this.body.classList.add('has-active-menu');
    this.wrapper.classList.add('has-' + this.options.type);
    this.menu.classList.add('is-active');
    this.mask.classList.add('is-active');
    this.disableMenuOpeners();
  };

  /**
   * Close Menu.
   */
  Menu.prototype.close = function() {
    this.body.classList.remove('has-active-menu');
    this.wrapper.classList.remove('has-' + this.options.type);
    this.menu.classList.remove('is-active');
    this.mask.classList.remove('is-active');
    this.enableMenuOpeners();
  };

  /**
   * Disable Menu Openers.
   */
  Menu.prototype.disableMenuOpeners = function() {
    each(this.menuOpeners, function(item) {
      item.disabled = true;
    });
  };

  /**
   * Enable Menu Openers.
   */
  Menu.prototype.enableMenuOpeners = function() {
    each(this.menuOpeners, function(item) {
      item.disabled = false;
    });
  };

  /**
   * Add to global namespace.
   */
  window.Menu = Menu;

})(window);
```

I saved this out to a file called `menu.js`, and included it in the index file. Underneath this inclusion, we can now start creating new menu instances, and listening for clicks on buttons to open up the menus. I'll just do one version here, but in the source, you can see how I created all 8 instances. Here's the slide left menu code:

```javascript
var slideLeft = new Menu({
  wrapper: '#o-wrapper',
  type: 'slide-left',
  menuOpenerClass: '.c-button',
  maskId: '#c-mask'
});

var slideLeftBtn = document.querySelector('#c-button--slide-left');

slideLeftBtn.addEventListener('click', function(e) {
  e.preventDefault;
  slideLeft.open();
});
```

And there you go. You should now be able to toggle menus and see them in action!

## Wrap Up

There it is! Some nice slide and push menus, ready to drop into your project. I included sample media queries in the source code, so feel free to download it or view the demos by clicking the links below. Also, don’t forget to leave your comments below, and thanks for stopping by!

<p class="text-align--center">
<a href="https://github.com/callmenick/Slide-Push-Menus" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/slide-push-menus/" class="button button--inline-block button--medium">View Demo</a>
</p>