<p class="text-align--center">
<a href="https://github.com/callmenick/responsive-tabs" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/responsive-tabs/" class="button button--inline-block button--medium">View Demo</a>
</p>

## Tabbed Layouts

Tabbed layouts are a great way to house a lot of related content in a small, compact area. Often, you'll see them as sidebar widgets on blogs containing things like recent posts, favourite posts, and categories. But tabbed layouts may also exist as the main layout structure in web applications. Luckily, making your own layout is super simple, and requires simple and minimal amounts of CSS and JavaScript to get up and running. In this tutorial, we'll step through all the parts required to build your own tabbed layout component, including a simple fallback for browsers that have JavaScript disabled. Let's dive in with the structure and markup.

## Getting Started With Structure & Markup

Our structure will consist of the following:

* A navigation section for the tabs that will feature active-link highlighting
* The tabs themselves, that will either be displayed or hidden depending on which tab is active

The entire component will be housed in one master `div` element, and that's what we'll use to target children elements in the JavaScript. To get started, mark up an element like this:

```html
<div id="tabs" class="c-tabs no-js">
</div>
```

The ID name is arbitrary, but it will be used later on in the JavaScript. Also note the `no-js` class which will be removed if and when JavaScript gets run. If you're using a feature detection library like [Modernizr](https://modernizr.com/), then this should be familiar to you.

Now, let's build up our navigation:

```html
<div id="tabs" class="c-tabs no-js">
  <div class="c-tabs-nav">
    <a href="#" class="c-tabs-nav__link is-active"></a>
    <a href="#" class="c-tabs-nav__link"></a>
    ...
  </div>
</div>
```

Notice that I've initialized the first tab as the active tab. This will be of more significance a bit later when we also initialize the first tab to be active, and the first index to be 0 in the JavaScript. Also, keep in mind that while the number of items may be dynamic, you should have equal amounts of navigation links and tabs.

Let's move onto the actual tabs. We'll mark them up as separate `div` elements, and give the first one the `is-active` class. Here's what the final markup structure looks like:

```html
<div id="tabs" class="c-tabs no-js">
  <div class="c-tabs-nav">
    <a href="#" class="c-tabs-nav__link is-active"></a>
    <a href="#" class="c-tabs-nav__link"></a>
    ...
  </div>
  <div class="c-tab is-active">
    <div class="c-tab__content"></div>
  </div>
  <div class="c-tab">
    <div class="c-tab__content"></div>
  </div>
  ...
</div>
```

At this point in your browser, you should just see one big linear mess. That's alright though, because we're going to clean that up with some CSS!

## Styling It Up With CSS

**A quick note:** I'm using [flexbox](https://developer.mozilla.org/en-US/docs/Web/CSS/flex), and handling all my vendor prefixes with [autoprefixer](https://www.npmjs.com/package/gulp-autoprefixer) alongside [Gulp](http://gulpjs.com/). The CSS presented below does't have any prefixes, but all the compiled CSS in the [Git repo](https://github.com/callmenick/responsive-tabs) does, and you're free to just use that if you wish.

The enclosing `c-tabs` element doesn't require any styling to bring it all to life, so we're going to start with the navigation items. This is just a collection of anchor tags displayed alongside each other, with some state styles to match. Here's the CSS I came up with:

```css
.c-tabs-nav {
  display: flex;
  list-style: none;
  margin: 0;
  padding: 0;
}

.c-tabs-nav__link {
  flex: 1;
  margin-right: 4px;
  padding: 12px;
  color: #fff;
  background-color: #b3b3b3;
  text-align: center;
  transition: color 0.3s;
}

.c-tabs-nav__link:last-child {
  margin-right: 0;
}

.c-tabs-nav__link:hover {
  color: #6d6d6d;
}

.c-tabs-nav__link.is-active {
  color: #dc446e;
  background-color: #e7e7e7;
}

.c-tabs-nav__link i,
.c-tabs-nav__link span {
  margin: 0;
  padding: 0;
  line-height: 1;
}

.c-tabs-nav__link i {
  font-size: 18px;
}

.c-tabs-nav__link span {
  display: none;
  font-size: 18px;
}

@media all and (min-width: 720px) {
  .c-tabs-nav__link i {
    margin-bottom: 12px;
    font-size: 22px;
  }
  .c-tabs-nav__link span {
    display: block;
  }
}
```

Let's step through the navigation CSS. The containing element is a flex container governed by the class `c-tabs-nav`. Each of the anchor tags are flex items that are governed by the class `c-tabs-nav__link`, and they stretch equally to fit the available space including a slight margin in between each. These anchors will be responsible for listening for click events and changing tabs accordingly. They will also change state depending on if they are being hovered over, or if they are active (i.e. representing the tab currently in view). In the demo, each link also has an icon and a text span, and those are represented by the `i` and `span` declarations. On smaller screens, the span is hidden to preserve space. This is demonstrated by the simple media query at the end of this section of CSS.

Let's take a look now at the CSS for the actual tab containers:

```css
/**
 * Tab
 */
.c-tab {
  display: none;
  background-color: #e7e7e7;
}

.c-tab.is-active {
  display: block;
}

.c-tab__content {
  padding: 1.5rem;
}
```

This section of CSS is fairly simple and intuitive, but we'll step through it anyways. Tabs are governed by the class `c-tab`, and are hidden by default. That's achieved by defaulting them to `display: none`. If the `is-active` class is present though, then they get displayed by reverting to `display: block`. The class `c-tab__content` represents the content area inside the tab. Functionally, it does nothing. It's presence though allows for simple styling of the tab's content without affecting the container.

At this point, you should have a pretty neat looking visual on screen. However, clicking on the navigation buttons do nothing! Fear not though, we're about to bring it to life with JavaScript.

## Bringing It To Life With JavaScript

Let's take a stepwise look at what we want to achieve with our JavaScript:

1. We want to be able to set up our components with some sort of options hash
2. We want to be able to initialize our component somehow
3. We need to make sure we have event listeners for each of our navigation items
4. We also want to have a method that we can call internally or manually that will allow users to go to a specific tab
5. Finally, we want access to our component, so we'll add it to the global namespace

To kick things off, let's open up an [IIFE](https://en.wikipedia.org/wiki/Immediately-invoked_function_expression):

```javascript
(function() {

  'use strict';

})();
```

Now, let's set up our function, and add it to the global namespace:

```javascript
(function() {

  'use strict';

  var tabs = function(options) {
    // code here...
  };

  window.tabs = tabs;

})();
```

Notice that `tabs` takes one argument - `options` - which will contain the passed in options. Now, let's cache some variables and make them available in our functions that we will write next:

```javascript
(function() {

  'use strict';

  var tabs = function(options) {
    
    var el = document.querySelector(options.el);
    var tabNavigationLinks = el.querySelectorAll(options.tabNavigationLinks);
    var tabContentContainers = el.querySelectorAll(options.tabContentContainers);
    var activeIndex = 0;
    var initCalled = false;
    
  };

  window.tabs = tabs;

})();
```

The five variables that we've set up are:

1. `el` - the containing element based on a selector passed in with the options hash
2. `tabNavigationLinks ` - the collection of navigation links based on the specified class name passed in with the options hash
3. `tabContentContainers ` - the actual tab content containers based on the specified class name passed in with the options hash
4. `activeIndex` - the index of the currently active tab, defaulted to 0 to match our active classes in the html
5. `initCalled` - a simple boolean defaulted to false to check if `init` has been called yet, because we don't want to be able to initialize more than once

With that in mind, let's think about our next steps. We'll need some sort of initializer function that will iterate over all the tab links and add click event listeners to them, and also remove the `no-js` class as discussed earlier. We'll also need a function to handle that click event. And finally, we'll need a function that goes to a specific tab based on that tab's index. This will get called from the event handler function. Thinking ahead, we'll also want to return the initializer function so that you can call it whenever you are ready, and also the function that goes to a specific tab by index, to allow you to manually go to a tab whenever you want. Due to the power of JavaScript closures and the variable `initCalled ` that we defaulted to false, we're able to safely return the initializer and prevent it from being run more than once. Here's the outline now:

```javascript
(function() {

  'use strict';

  var tabs = function(options) {

    var el = document.querySelector(options.el);
    var tabNavigationLinks = el.querySelectorAll(options.tabNavigationLinks);
    var tabContentContainers = el.querySelectorAll(options.tabContentContainers);
    var activeIndex = 0;
    var initCalled = false;

    var init = function() {
    };

    var handleClick = function(link, index) {
    };

    var goToTab = function(index) {
    };

    return {
      init: init,
      goToTab: goToTab
    };

  };

  window.tabs = tabs;

})();
```

Let's examine the `init` function now. We know we only want to run this code if `initCalled` is still false. At that point, we'll toggle the boolean value of it, and also remove the `no-js` class from our element. After that, we'll loop over the navigation links, and attach event listeners to each of them by calling the function `handleClick` and passing in the link itself and its corresponding index. Here's what it looks like:

```javascript
var init = function() {
  if (!initCalled) {
    initCalled = true;
    el.classList.remove('no-js');
    
    for (var i = 0; i < tabNavigationLinks.length; i++) {
      var link = tabNavigationLinks[i];
      handleClick(link, i);
    }
  }
};
```

Now, let's move onto the `handleClick` function. This is pretty straightforward. We're going to attach a `click` event listener on the link, prevent default behaviour, and call the `goToTab` function that accepts the index value. Here's what it looks like:

```javascript
var handleClick = function(link, index) {
  link.addEventListener('click', function(e) {
    e.preventDefault();
    goToTab(index);
  });
};
```

Finally, let's take a look at the `goToTab` function. It doesn't make sense to run the code inside here unless the following three conditions are met:

1. The passed in index is !== to the index of the currently active tab
2. The index is less than 0
3. The index is greater than the highest index

If these three conditions are met, then we simply need to toggle the correct classes on the navigation links and tabs themselves, and then update the `activeIndex` value. Here's what the code looks like:

```javascript
var goToTab = function(index) {
  if (index !== activeIndex && index >= 0 && index <= tabNavigationLinks.length) {
    tabNavigationLinks[activeIndex].classList.remove('is-active');
    tabNavigationLinks[index].classList.add('is-active');
    tabContentContainers[activeIndex].classList.remove('is-active');
    tabContentContainers[index].classList.add('is-active');
    activeIndex = index;
  }
};
```

Awesome, now, this should work perfectly! And for your reference, here's the full JavaScript code:

```javascript
(function() {

  'use strict';

  /**
   * tabs
   *
   * @description The Tabs component.
   * @param {Object} options The options hash
   */
  var tabs = function(options) {

    var el = document.querySelector(options.el);
    var tabNavigationLinks = el.querySelectorAll(options.tabNavigationLinks);
    var tabContentContainers = el.querySelectorAll(options.tabContentContainers);
    var activeIndex = 0;
    var initCalled = false;

    /**
     * init
     *
     * @description Initializes the component by removing the no-js class from
     *   the component, and attaching event listeners to each of the nav items.
     *   Returns nothing.
     */
    var init = function() {
      if (!initCalled) {
        initCalled = true;
        el.classList.remove('no-js');
        
        for (var i = 0; i < tabNavigationLinks.length; i++) {
          var link = tabNavigationLinks[i];
          handleClick(link, i);
        }
      }
    };

    /**
     * handleClick
     *
     * @description Handles click event listeners on each of the links in the
     *   tab navigation. Returns nothing.
     * @param {HTMLElement} link The link to listen for events on
     * @param {Number} index The index of that link
     */
    var handleClick = function(link, index) {
      link.addEventListener('click', function(e) {
        e.preventDefault();
        goToTab(index);
      });
    };

    /**
     * goToTab
     *
     * @description Goes to a specific tab based on index. Returns nothing.
     * @param {Number} index The index of the tab to go to
     */
    var goToTab = function(index) {
      if (index !== activeIndex && index >= 0 && index <= tabNavigationLinks.length) {
        tabNavigationLinks[activeIndex].classList.remove('is-active');
        tabNavigationLinks[index].classList.add('is-active');
        tabContentContainers[activeIndex].classList.remove('is-active');
        tabContentContainers[index].classList.add('is-active');
        activeIndex = index;
      }
    };

    /**
     * Returns init and goToTab
     */
    return {
      init: init,
      goToTab: goToTab
    };

  };

  /**
   * Attach to global namespace
   */
  window.tabs = tabs;

})();
```

## A Simple No-JS Fallback

I mentioned earlier that we'll be implementing a very barebones fallback for browsers that have JavaScript disabled. Remember, if JavaScript is disabled, then the code will never run in the first place. Thus, we can leverage our `no-js` class to display all sections of content (tabs) linearly. Here's the basic CSS implementation:

```css
/**
 * Tabs no-js fallback
 */
.c-tabs.no-js .c-tabs-nav {
  display: none;
}

.c-tabs.no-js .c-tab {
  display: block;
  margin-bottom: 1.5rem;
}

.c-tabs.no-js .c-tab:last-child {
  margin-bottom: 0;
}
```

## Wrap Up

And that’s a wrap! In this tutorial, we've taken a very stepwise solution to build up our own responsive tabbed layout component. We leveraged a bit of CSS to style it, and JavaScript to pull it all together, and included a fallback for browsers that don't have JavaScript enabled. I hope you enjoyed this tutorial and find it useful in your future projects! Don’t forget, you can view the demo and download the source by clicking the links below, and if you have and questions, comments, or feedback, you can also leave them below.

<p class="text-align--center">
<a href="https://github.com/callmenick/responsive-tabs" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/responsive-tabs/" class="button button--inline-block button--medium">View Demo</a>
</p>