<p class="text-align--center">
<a href="https://github.com/callmenick/Flexbox-Examples" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/flexbox-examples/" class="button button--inline-block button--medium">View Demo</a>
</p>

## Introduction

A while back, I wrote an [article for SitePoint introducing flexbox.](http://www.sitepoint.com/are-we-ready-to-use-flexbox/) In that article, I investigated and tried to answer the question:

> Are we ready to use flexbox?

Back then, I said you'd have to choose wisely when to use it, but insisted that you should start getting familiar with the syntax and incorporating it into your modern projects. Today, I'm happy to assertively answer the question one more time:

> Yes, we are ready to use flexbox!

If you're unfamiliar with flexbox, then here's a quick primer. The [CSS Flexible Layout Module](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Flexible_boxes) is a layout module that allows us to easily layout boxes on the screen, given the available space. It is a big improvement over the traditional box model, given that it doesn't use floats at all. Boxes can be arranged in rows or columns, specific orders can be set for flexbox items, and there are a host of alignment, spacing, and sizing options.

I probably won't cover every single aspect in this tutorial, so I suggest you read up on the MDN link referenced above if you want to sharpen up your overall knowledge on the topic. Rather, this tutorial will address some common layout problems, and show how quickly and easily we can build them. Every layout pattern I present will also be responsive, further showing the ease in which we can build with flexbox. Here's what we'll tackle:

1. A simple grid system
2. The holy grail layout
3. A fluid navigation with transitioning width search bar
4. Two different vertical alignment scenarios

Alright, let's dive in!

## 1 - A Simple Grid System

Grid systems govern a big part of layouts today, and the default CSS box-model behaviour has resulted in us resorting to floated layouts or inline-block hacks, both of which have their share of quirks. Flexbox allows us to really easily develop a powerful and scalable grid system with just a few lines of CSS. Let's dig in.

Imagine we had some simple markup like this:

```html
<div class="grid">
  <div class="grid__row">
    <div class="grid__item">1</div>
    <div class="grid__item">2</div>
    ...
  </div>
</div>
```

In a traditional grid set up, we'd have to somehow state how many items the grid row might contain, and then set the widths for each item in the row accordingly. With flexbox, we can add as many items to the row as we want, and the widths will adjust accordingly. In other words, we can have markup like this, without having to worry about stating how many items exist in each row in the CSS:

```html
<div class="grid">
  <div class="grid__row">
    <div class="grid__item">1</div>
    <div class="grid__item">2</div>
  </div>
  <div class="grid__row">
    <div class="grid__item">1</div>
    <div class="grid__item">2</div>
    <div class="grid__item">3</div>
  </div>
  <div class="grid__row">
    <div class="grid__item">1</div>
    <div class="grid__item">2</div>
    <div class="grid__item">3</div>
    <div class="grid__item">4</div>
  </div>
</div>
```

Let's now look at the CSS for this. I'll use some properties purely for aesthetics (like the border and padding), but other than that, it's so simple. Here it is:

```css
.grid {
  border: solid 1px #e7e7e7;
}

.grid__row {
  display: flex;
}

.grid__item {
  flex: 1;
  padding: 12px;
  border: solid 1px #e7e7e7;
}
```

Just like that, we have a grid in action! Adding the `display: flex` to the `.grid__row` containers creates a flex container, and each sub-item is a flex item as a result. Applying `flex: 1` to the flex items makes them occupy an equal amount of space relative to the full width of the container. Now you can have as many grid rows as you want, each with their own arbitrary amount of grid items, and it'll conform to a simple responsive grid with no further CSS required.

What about breakpoints and column layout? If we want the grid items to be in a column layout instead of a row, we can simply declare `flex-direction: column` on the `.grid__row` containers. In that case, we can create a super simple responsive layout by including some modifiers. So our markup might look like this:

```html
<div class="grid">
  <div class="grid__row grid__row--sm">
    <div class="grid__item">1</div>
    ...
  </div>
  <div class="grid__row grid__row--md">
    <div class="grid__item">1</div>
    ...
  </div>
  <div class="grid__row grid__row--lg">
    <div class="grid__item">1</div>
    ...
  </div>
</div>
```

And our revised CSS like this:

```css
.grid {
  border: solid 1px #e7e7e7;
}

.grid__row {
  display: flex;
  flex-direction: column;
}

.grid__item {
  flex: 1;
  padding: 12px;
  border: solid 1px #e7e7e7;
}

@media all and ( min-width: 480px ) {

  .grid__row--sm {
    flex-direction: row;
  }

}

@media all and ( min-width: 720px ) {

  .grid__row--md {
    flex-direction: row;
  }

}

@media all and ( min-width: 960px ) {

  .grid__row--lg {
    flex-direction: row;
  }

}
```

And voila. A super simple responsive grid system with just a few lines of CSS. This system is so bullet-proof as it stands, that you can even nest grids without having to worry about a thing:

```html
<div class="grid">
  <div class="grid__row grid__row--sm">
    <div class="grid__item">
      <div class="grid">
        <div class="grid__row grid__row--lg">
          <div class="grid__item">Nested 1</div>
          ...
        </div>
      </div>
    </div>
    <div class="grid__item">2</div>
  </div>
  <div class="grid__row grid__row--md">
    <div class="grid__item">1</div>
    ...
  </div>
</div>
```

Check out the [demo in action here.](http://callmenick.com/_development/flexbox-examples/index.html)

## 2 - The Holy Grail Layout

The holy grail layout is pretty famous in web design, and even in the age of web applications and all that jazz, this layout still plays a very important part on the web today - content-rich sites employ it heavily. Back in 2006, the holy grail layout was beautifully achieved and [documented on A List Apart](http://alistapart.com/article/holygrail). It utilised floats, negative margins, and minimum widths to ensure that combined sizes don't conflict and break the layout. All that, combined with the modern day necessity to employ a responsive layout, meant a lot of number crunching, float clearing, and other sorceries to get it right. And if you decided to change the width of your sidebar, well you'd have to cascade the math into all the other components.

Flexbox eases up the headache quite a bit, as we can specify column or row layouts, and also explicitly set the order of elements in the CSS, even if they appear in a different order in the markup. Here's a typical holy grail setup:

```html
<body class="holy-grail">
  <header class="holy-grail__header"></header>
  <main class="holy-grail__body">
    <div class="holy-grail__content"></div>
    <div class="holy-grail__sidebar holy-grail__sidebar--first"></div>
    <div class="holy-grail__sidebar holy-grail__sidebar--second"></div>
  </div>
  <footer class="holy-grail__footer"></footer>
</body>
```

In my demo, the holy grail layout is nested inside a document, so there's no `body` or `main` tags like shown above. Regardless, we're interested in the class names and the appearance of sections in the markup as opposed to the actual elements themselves. In particular, take note of the modifier classes used on the two sidebars, and the trivial order in which they appear in the markup. Let's break this down to paint a clear picture of what's happening:

1. We have a parent container, `.holy-grail`, and in it we have three flex items. These items are `.holy-grail__header`, `holy-grail__body`, and `holy-grail__footer`.
2. The three of these items are stacked on top of each other, and occupy 100% width of the screen. Therefore, the flex container must have a columnar flex direction.
3. The body of the holy grail, governed by `.holy-grail__body`, is a sub flex container. It's child flex items should have a columnar layout on narrow screens, and a row layout on wider screens.

Taking into consideration all of the above, let's build our holy grail layout:

```css
.holy-grail {
  display: flex;
  flex-direction: column;
}

.holy-grail__header,
.holy-grail__footer {
  flex: 0 0 100%;
}

.holy-grail__body {
  display: flex;
}

.holy-grail__sidebar {
  /* nothing at narrow screens */
}

.holy-grail__sidebar--first {
  order: 1;
}

.holy-grail__sidebar--second {
  order: 3;
}

.holy-grail__content {
  order: 2;
}

@media all and ( min-width: 720px ) {
  
  .holy-grail__body {
    flex-direction: row;
  }

  .holy-grail__sidebar {
    flex: 0 0 180px;
  }

  .holy-grail__content {
    flex: 1;
  }

}

@media all and ( min-width: 960px ) {

  .holy-grail__sidebar {
    flex: 0 0 240px;
  }

}
```

It really doesn't get simpler than that! As I mentioned before in the breakdown, we're setting up two flex containers in the initial view (narrow screens). At our first breakpoint, we change the flex direction of the body to take on a row pattern, and give the sidebars a width of 180px using the shorthand `flex` property. This shorthand property allows us to restrict the `flex-grow` and `flex-shrink` properties up front, and explicitly declare the width. The content uses the `flex: 1` property to make it fill the available space. Ordering the flex items was also a breeze using the simple `order` property. There's not much more to it besides some aesthetic styles that you might want to include...it really is that simple and effective. Did I mention that by default, flexbox makes the columns equal height? [Check out the demo here.](http://callmenick.com/_development/flexbox-examples/index2.html)

## 3 - Fluid Navigation With Animating Search Bar

In our next example, we're going to create something a little fun that includes a neat transition. We'll build a full width fluid navigation, and in it, we'll include a search bar that transitions smoothly to a bigger width on focus. By leveraging the power of flexbox, we want to be able to add as many items to the navigation as we want without having to alter the CSS. I'll use some modifier classes to achieve what I want. As a little bonus, I'm going to make the navigation fully responsive with a toggle button included! Here's what the HTML looks like:

```html
<nav class="flexy-nav">
  <button id="flexy-nav__toggle" class="flexy-nav__toggle">Toggle Nav</button>
  <ul id="flexy-nav__items" class="flexy-nav__items">
    <li class="flexy-nav__item"><a href="#" class="flexy-nav__link">Item 1</a></li>
    <li class="flexy-nav__item"><a href="#" class="flexy-nav__link">Item 2</a></li>
    <li class="flexy-nav__item"><a href="#" class="flexy-nav__link">Item 3</a></li>
    <li class="flexy-nav__item"><a href="#" class="flexy-nav__link">Item 4</a></li>
  </ul>
  <form action="" class="flexy-nav__form">
    <input class="flexy-nav__search" type="text" placeholder="Type search terms and hit enter...">
  </form>
</nav>
```

Let's break it down before diving into the CSS. We have our main flex container governed by the `.flexy-nav` class. The button acts as the toggle switch, the unordered list houses the main navigation items, and the form holds the search bar. In the narrow screen setup, we want all three items to be in columnar layout, and we also want the individual list items to be in a column also. On a wider screen, we want to hide the toggle switch, make the unordered list a flex container, distribute the list items in a row, and make the form a fixed width. The list items will be evenly distributed in the remaining space as a result. When we focus on the input text field, we want to see it transition smoothly to a wider width, while all the list items shrink smoothly. Here's the CSS:

```css
/* resets */

input,
button {
  font: inherit;
  border-radius: none;
  box-shadow: none;
  appearance: none;
}

button {
  cursor: pointer;
}

/* nav container */

.flexy-nav {
  display: flex;
  flex-direction: column;
}

/* nav links */

.flexy-nav__items {
  display: none;
  flex: 1;
  flex-direction: column;
  list-style: none;
  margin: 0 0 4px 0;
  padding: 4px;
  text-align: center;
}

.flexy-nav__items--visible {
  display: flex;
}

.flexy-nav__item {
  background-color: #f1f1f1;
  border-bottom: solid 1px #e7e7e7;
}

.flexy-nav__item:last-child {
  border-bottom: 0;
}

.flexy-nav__link {
  padding: 8px;
  display: block;
}

/* nav toggle */

.flexy-nav__toggle {
  margin: 0 0 4px 0;
  padding: 4px;
  color: #fff;
  background-color: #f07850;
  border: none;
}

.flexy-nav__toggle:hover,
.flexy-nav__toggle:focus {
  outline: none;
  background-color: #c93f11;
}

/* nav form */

.flexy-nav__form {
  height: 48px;
}

.flexy-nav__search {
  display: block;
  margin: 0;
  padding: 0 4px;
  width: 100%;
  height: 48px;
  color: #6d6d6d;
  background-color: #fff;
  border: solid 2px #e7e7e7;
}

.flexy-nav__search:focus {
  outline: none;
  border: solid 2px #6d6d6d;
}

/* media queries */

@media all and (min-width: 768px) {
  .flexy-nav {
    flex-direction: row;
  }

  .flexy-nav__items {
    display: flex;
    flex-direction: row;
    margin: 0;
    padding: 0;
    height: 48px;
  }

  .flexy-nav__item {
    flex: 1;
    margin-right: 4px;
    border-bottom: none;
  }

  .flexy-nav__link {
    padding: 0;
    line-height: 48px;
  }

  .flexy-nav__toggle {
    display: none;
  }

  .flexy-nav__form {
    flex: none;
  }

  .flexy-nav__search {
    width: 240px;
    transition: width 0.3s;
  }

  .flexy-nav__search:focus {
    width: 360px;
  }
}
```

As promised, here's a ridiculously simple JavaScript snippet to show and hide the navigation on narrow screens:

```javascript
(function() {
  var toggle = document.querySelector("#flexy-nav__toggle");
  var nav = document.querySelector("#flexy-nav__items");
  toggle.addEventListener("click", function(e) {
    e.preventDefault();
    nav.classList.contains("flexy-nav__items--visible") ? nav.classList.remove("flexy-nav__items--visible") : nav.classList.add("flexy-nav__items--visible");
  });
})();
```

Simple as that. We've just created a neat and scalable fluid navigation with flexbox, and incorporated a neat search-bar transition too. We can add and remove links as we please or change up the dimensions on the search bar on the fly without affecting the functionality one bit. Ah, the joys of flexbox. Don't forget to check out the [demo for this one here.](http://callmenick.com/_development/flexbox-examples/index3.html)

## 4 - Vertical Alignment

Let's face it, vertical alignment with traditional CSS sucks. Inline-block elements can sometimes help out, there are some absolute position hacks, and there is the old-school table layouts (which don't semantically apply to many situations today). All of them have there big quirks though, and always require some kinds of extra sorcery to make them work.

Flexbox takes care of this so easily. We're going to look at two examples that involve vertical alignment:

1. First, we'll look at a media object type setup, where we have a user's avatar to the left, and their name and some info to the right. We will use flexbox to make sure the image and the body are perfectly vertically aligned.
2. Next, we'll look at simply vertically (and horizontally) aligning an element of fixed width and variable height in a container. As the element grows in height, it remains perfectly centred.

Let's start with the first one. As mentioned, we want a user avatar to the left, and a description to the right. No matter how long or short the description gets though, we want it to always remain perfectly vertically aligned with the avatar. Here's some typical markup:

```html
<div class="user">
  <div class="user__avatar"></div>
  <div class="user__description">
    <h2 class="user__username">John Doe</h2>
    <p class="user__excerpt">I'm John Doe...</p>
  </div>
</div>

<div class="user">
  <div class="user__avatar"></div>
  <div class="user__description">
    <h2 class="user__username">Harry Potter</h2>
    <p class="user__excerpt">I'm Harry Potter...with a really long description...</p>
  </div>
</div>
```

Before digging into the CSS, take note that we'll use a property that we haven't seen before. It's the `align-items` property, and it allows us to align items along the current flex line in the perpendicular direction. In other words, if our flex line is running horizontally, we can align them based on the perpendicular line. In our case, we want to center-align them, so we can just use `align-items: center`. Here's the CSS:

```css
.user {
  display: flex;
  align-items: center;
}

.user:last-child {
  margin-bottom: 0;
}

.user__avatar {
  flex: 0 0 96px;
  width: 96px;
  height: 96px;
  background-color: #e7e7e7;
}

.user__description {
  flex: 1;
  margin-left: 24px;
  padding: 12px;
  border: solid 1px #e7e7e7;
}
```

Easy as that. You can style up the text as you please, make those descriptions as long as you want, or change up the avatar size. It doesn't matter, the functionality will remain the same. [Check out the demo to see it in action.](http://callmenick.com/_development/flexbox-examples/index4.html)

Let's move on to the second part of our vertical alignment series. This time, let's imagine we have a banner image at the top of our markup, and inside the banner, we want some headline text. On small screens, the height of the banner will be 180px, and it'll break two more times up to a height of 480px. At all points in time, we want our headline text inside this banner to be perfectly centred, both horizontally and vertically. Here's the typical markup:

```html
<div class="banner">
  <div class="banner__content">
    <h2 class="banner__title">Symmetrical Perfection</h2>
    <span class="banner__sub">A beautiful sight, achieved with flexbox.</span>
  </div>
</div>
```

This time, we'll also leverage the `justify-content` property which will allow us to distribute space around items along the flex line. Here's the CSS:

```css
.banner {
  display: flex;
  align-items: center;
  justify-content: space-around;
  height: 180px;
  background-color: #e7e7e7;
}

.banner__content {
  text-align: center;
}

.banner__title,
.banner__sub {
  margin: 0;
  padding: 0;
  line-height: 1.5;
}

@media all and ( min-width: 480px ) {

  .banner {
    height: 240px;
  }

}

@media all and ( min-width: 768px ) {

  .banner {
    height: 360px;
  }

}

@media all and ( min-width: 960px ) {

  .banner {
    height: 480px;
  }

}
```

No matter how tall we make the banner, the content will remain vertically and horizontally centred, and such is the power of flexbox. Don't forget to [check out the demo.](http://callmenick.com/_development/flexbox-examples/index4.html)

## Support & Vendor Prefixes

Know your market...that is the key word. Flexbox is supported by all modern browsers, including IE10 and up. If you're building modern web applications, flexbox is a powerhouse of a tool, and I'd highly recommend it. If you're building or revamping a website, check your analytics for your target audience. Nowadays, it's highly likely that 99% and up of your audience is using a modern browser.

As far as vendor prefixes go, flexbox is full of them. It's straight up insane to even think about using flexbox seriously, and hardcoding all the vendor prefixes yourself. I personally use a task runner (Gulp) to auto-prefix my CSS. You should certainly look into implementing a task runner into your workflow, even if you're not using flexbox.

## Wrap Up

And that’s a wrap! I'll end off on this note. If you're looking for a production website that heavily employs flexbox, look no further. The new callmenick.com uses it for just about everything! Thanks for reading, and don’t forget, you can view the demo and download the source by clicking the links below. If you have and questions, comments, or feedback, you can also leave them below.

<p class="text-align--center">
<a href="https://github.com/callmenick/Flexbox-Examples" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/flexbox-examples/" class="button button--inline-block button--medium">View Demo</a>
</p>