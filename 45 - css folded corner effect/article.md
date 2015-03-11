<p class="text-align--center">
<a href="http://www.callmenick.com/tutorial-demos/css-folding-corner/css-folding-corner-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://www.callmenick.com/tutorial-demos/css-folding-corner/" class="button button--inline-block button--medium">View Demo</a>
</p>

## Introduction

The folded corner effect adds a nice little touch to your site, and can easily be done these days with just plain old CSS. Previously, we’d have to create PNG’s and apply them as background images or absolutely positioned elements. Nowadays, that’s just not good enough for maintainability and scalability. Luckily for us, the effect can be easily achieved with good old CSS. We don’t even need to add any markup to our HTML. Let’s first look at the HTML structure, which is beyond simple.

## The HTML Structure

The markup is simple. For our case, we’re going to assume a simple content area wrapped in a `section` tag. The HTML looks like this:

```html
<section>
  <h2>Folded Corner</h2>
  <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Commodi, sunt, quidem suscipit...</p>
</section>
```

Easy enough, right? But how can we achieve the effect with just one tag in the markup, and pure CSS? Let’s look below.

## CSS Pseudo Classes To The Rescue

The CSS pseudo classes `::before` and `::after` are two of my favourite things about CSS. They allow us to add some visual style to our content without having to touch the markup at all and break any kind of semantic structure. Because we’re adding some style properties that affect only the visual aesthetic of our semantics, browsers that don’t support the pseudo classes (IE7 and down) will fallback gracefully. Let’s dig in to our CSS. But first, it’s very important to note that the CSS below isn’t prefixed, and you should prefix where necessary. Also, add the `box-sizing: border-box` property throughout the document ([read why here](http://callmenick.com/2014/03/26/box-sizing-reset-border-box/)). OK, onward.

## Achieving The CSS Folded Corner Effect

![Flat corner](/files/2015-03/folded_corner_flat1.png)

The image above shows the effect in action. We have a rectangular section of content, and the top right corner looks like it’s folded over. To achieve this with CSS, we can think of that top right corner as two triangles:

* One white one, pointing north-east
* One  dark blue one, pointing south-west

Convenient then, that we have two pseudo classes at our disposal. One of them will implement the white triangle, and one will implement the dark blue triangle. We’ll use some [CSS triangle trickery](http://callmenick.com/2014/05/03/css-triangles/) inside our pseudo classes to achieve the effect. Here’s what the base CSS looks like:

```css
section {
  position: relative;
  margin-bottom: 40px;
  padding: 40px;
  background-color: #96a5be;
}
section:last-child {
  margin-bottom: 0;
}
section:before, section:after {
  display: block;
  position: absolute;
  width: 40px;
  height: 40px;
  top: 0;
  right: 0;
  content: "";
}
section:before {
  border-top: solid 20px #fff;
  border-right: solid 20px #fff;
  border-left: solid 20px transparent;
  border-bottom: solid 20px transparent;
}
section:after {
  border-bottom: solid 20px #5b7093;
  border-left: solid 20px #5b7093;
  border-right: solid 20px transparent;
  border-top: solid 20px transparent;
}
```

This will also serve as the basis for our next two styles. In other words, the next two styles will just be extensions of what we’ve seen above.

## Adding A Border Radius

![Rounded corner](/files/2015-03/folded_corner_rounded.png)

Adding a border radius is easy. We just need to be aware of which pseudo element we have to target, and which corner of our triangle we need it applied to. We’ll add a `.border-radius` class to our section for targeting purposes, so the HTML looks like this:

```html
<section class="border-radius">
  <h2>Folded Corner</h2>
  <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Commodi, sunt, quidem suscipit...</p>
</section>
```

Now, our CSS like this:

```css
.border-radius {
  border-radius: 10px;
}
.border-radius:after {
  border-radius: 0 0 0 10px;
}
```

## Adding A Shadow

![Shadow](/files/2015-03/folded_corner_shadow.png)

Adding a shadow is also fairly simple, and give a more realistic impression that the corner is folding over. We’ll add the box-shadow class to our markup for targeting, so the HTMl looks like this:

```html
<section class="shadow">
  <h2>Folded Corner</h2>
  <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Commodi, sunt, quidem suscipit...</p>
</section>
```

In our CSS, we want to hide the shadow if it’s outside the bounds of the folded corner area. This is done simply with the `overflow` property. Also, we need to make sure we’re targeting the correct pseudo class. Our CSS looks like this:

```css
.shadow {
  overflow: hidden;
}
.shadow:after {
  box-shadow: -1px 1px 5px #212835;
}
```

## Wrap Up

And that’s a wrap. Simple but effective CSS techniques to add a little variety to our content areas. This can be applied to buttons, links, content areas, inputs, CSS icons, etc, to add a little extra style to your appearance without the use of imagery. The beauty lies in the fact that it’s very easily modifiable. I hope you enjoyed this tutorial, and if you have any questions, comments, or feedback, leave it below. Thanks for stopping by, and don’t forget you can view the demo or download the source by clicking the links below.

<p class="text-align--center">
<a href="http://www.callmenick.com/tutorial-demos/css-folding-corner/css-folding-corner-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://www.callmenick.com/tutorial-demos/css-folding-corner/" class="button button--inline-block button--medium">View Demo</a>
</p>