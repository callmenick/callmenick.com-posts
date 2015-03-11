<p class="text-align--center">
<a href="http://callmenick.com/development/icons-css3-transitions/icons-css3-transitions-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/development/icons-css3-transitions/" class="button button--inline-block button--medium">View Demo</a>
</p>

Icons. They exist everywhere, in all our apps and websites these days, and they all perform important functions. They guide users into various interactions, or send them to external links such as various social media platforms. Icon sprites are commonplace and important from a development standpoint, and allow us to engage users a bit with hover classes. These hover classes have the potential to add a neat layer of interactivity and style to our apps/sites. In this tutorial, I'm going to guide you through 5 different icon hover states, each utilizing CSS3 transitions and transforms, engaging the end users a bit more, and possibly leading to higher click-through rates.

## Marking Up Our Icons

People have different ways of structuring how an icon should exist in the markup. In this tutorial, the icons actually link to somewhere else, so I'll be using the a tag to house the icons. For presentational icon markup (like icons before headings and such), I've seen a lot of people use the `i` tag. Even though it's not semantic, it is very performant as it's a one-lettered tag.

For this tutorial, we're going to use icons that link elsewhere, because links are associated with hover states. The most common set of icons around the web that act as familiar links are social icons. I'll use the following 5 so that we can see different icons in action for each of the demos:

* facebook
* twitter
* google plus
* github
* rss

Each of the icons will contain three different classes:

* the `icon` class, which is an overall common class for each icon
* the `icon-type` class, which is unique for each demo
* the `name` class, which will allow us to position our background image depending on which icon is being displayed, plus use various colors etc.

So in general, our icons will follow this structure:

```html
<a href="#" class="icon icon-TYPE facebook">facebook</a>
<a href="#" class="icon icon-TYPE twitter">twitter</a>
<a href="#" class="icon icon-TYPE googleplus">google+</a>
<a href="#" class="icon icon-TYPE github">github</a>
<a href="#" class="icon icon-TYPE rss">rss</a>
```

Now, let's dig into some common CSS.

## Common CSS For Our Icons

Each of our icons will be an `inline-block` displayed element, but you can change this to block to suit your needs. Because we'll be utilizing pseudo classes for aesthetics, the default overflow will also be hidden. We also don't want our text to display, so we'll hide that. Here's the CSS:

```css
.icon {
  display: inline-block;
  vertical-align: top;
  overflow: hidden;
  margin: 4px;
  width: 96px;
  height: 96px;
  font-size: 0;
  text-indent: -9999px;
}
```

## A Quick Note

Each example uses an image sprite. Image sprites perform fantastically well, especially in the case of displaying a second version of the icon on hover. The sprites are all available in the downloadable source code (link at the top and bottom of this article). Now, let's dig into each of our examples.

## Style 1 – Monochromatic Icon With Transitioning Background

We're going to make use of two critical things in this demo:

1. Transitioning the background property
2. Using a suitable icon sprite

If we have a sprite, and each icon has two versions (a grey and a white in this case), we can transition the `background` property, making the icon slide to a new one on hover. Since we're transitioning the `background` property, we can also change the `background-color` on hover for added enhancement. Here's our CSS:

```css
.icon-mono {
  background: url("../img/icons1.png");
  background-image: url("../img/icons1.svg"), none;
  background-color: #595959;
  -webkit-transition: background 0.3s;
          transition: background 0.3s;
}

/* facebook */
.icon-mono.facebook {
  background-position: 0 0;
}

.icon-mono.facebook:hover {
  background-color: #3b5998;
  background-position: 0 -96px;
}

/* twitter */
.icon-mono.twitter {
  background-position: -96px 0;
}

.icon-mono.twitter:hover {
  background-color: #4099ff;
  background-position: -96px -96px;
}

/* google plus */
.icon-mono.googleplus {
  background-position: -192px 0;
}

.icon-mono.googleplus:hover {
  background-color: #d34836;
  background-position: -192px -96px;
}

/* github */
.icon-mono.github {
  background-position: -288px 0;
}

.icon-mono.github:hover {
  background-color: #333333;
  background-position: -288px -96px;
}

/* rss */
.icon-mono.rss {
  background-position: -384px 0;
}

.icon-mono.rss:hover {
  background-color: #ee802f;
  background-position: -384px -96px;
}
```

For quick reference, head to the demo link for this style [here](http://callmenick.com/development/icons-css3-transitions/index.html).

## Style 2 – Upward Nudge With Transitioning Border

In previous experiments, I've noticed that borders don't transition super smoothly. So in this case, I'll simulate a border using a pseudo element, and animate it by transitioning its height. The pseudo element will have a background color, and the icon will nudge upwards the same distance as the animated height of the pseudo element, giving it a “nudge” feeling. Here's the CSS:

```css
.icon-nudge {
  position: relative;
  background: url("../img/icons2.png");
  background-image: url("../img/icons2.svg"), none;
  -webkit-transition: background 0.2s;
          transition: background 0.2s;
}

.icon-nudge::after {
  display: block;
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 0;
  content: "";
  -webkit-transition: height 0.2s;
          transition: height 0.2s;
}

.icon-nudge:hover::after {
  height: 8px;
}

/* facebook */
.icon-nudge.facebook {
  background-color: #3b5998;
  background-position: 0 0;
}

.icon-nudge.facebook:hover {
  background-position: 0 -8px;
}

.icon-nudge.facebook::after {
  background-color: #1e2e4f;
}

/* twitter */
.icon-nudge.twitter {
  background-color: #4099ff;
  background-position: -96px 0;
}

.icon-nudge.twitter:hover {
  background-position: -96px -8px;
}

.icon-nudge.twitter::after {
  background-color: #0065d9;
}

/* google plus */
.icon-nudge.googleplus {
  background-color: #d34836;
  background-position: -192px 0;
}

.icon-nudge.googleplus:hover {
  background-position: -192px -8px;
}

.icon-nudge.googleplus::after {
  background-color: #86291d;
}

/* github */
.icon-nudge.github {
  background-color: #333333;
  background-position: -288px 0;
}

.icon-nudge.github:hover {
  background-position: -288px -8px;
}

.icon-nudge.github::after {
  background-color: #4d4d4d;
}

/* rss */
.icon-nudge.rss {
  background-color: #ee802f;
  background-position: -384px 0;
}

.icon-nudge.rss:hover {
  background-position: -384px -8px;
}

.icon-nudge.rss::after {
  background-color: #a9500e;
}
```

For quick reference, head to the demo link for this style [here](http://callmenick.com/development/icons-css3-transitions/index2.html).

## Style 3 – Icon Slide In Left

For this style, a normal monochromatic icon will be visible by default. When a user hovers, we'll make it seem as if a colored version of the icon is sliding in from the left, and pushing the monochromatic version out of the way. We'll use pseudo elements again, this time for both the default and hover view, leaving the default `.icon` element as a container. The pseudo elements will slide in and out of the container via `left` positioning, and the sliding of the elements will be smooth due to the transition property set on them. Here's the CSS:

```css
.icon-slide {
  position: relative;
}

.icon-slide::before,
.icon-slide::after {
  display: block;
  position: absolute;
  top: 0;
  width: 100%;
  height: 100%;
  background: url("../img/icons3.png");
  background-image: url("../img/icons3.svg"), none;
  content: "";
  -webkit-transition: left 0.3s;
          transition: left 0.3s;
}

.icon-slide::before {
  left: 0;
  background-color: #595959;
}

.icon-slide::after {
  left: -100%;
}

.icon-slide:hover::before {
  left: 100%;
}

.icon-slide:hover::after {
  left: 0;
}

/* facebook */
.icon-slide.facebook::before,
.icon-slide.facebook::after {
  background-position: 0 0;
}

.icon-slide.facebook::after {
  background-color: #3b5998;
}

/* twitter */
.icon-slide.twitter::before,
.icon-slide.twitter::after {
  background-position: -96px 0;
}

.icon-slide.twitter::after {
  background-color: #4099ff;
}

/* google plus */
.icon-slide.googleplus::before,
.icon-slide.googleplus::after {
  background-position: -192px 0;
}

.icon-slide.googleplus::after {
  background-color: #d34836;
}

/* github */
.icon-slide.github::before,
.icon-slide.github::after {
  background-position: -288px 0;
}

.icon-slide.github::after {
  background-color: #333333;
}

/* rss */
.icon-slide.rss::before,
.icon-slide.rss::after {
  background-position: -384px 0;
}

.icon-slide.rss::after {
  background-color: #ee802f;
}
```

For quick reference, head to the demo link for this style [here](http://callmenick.com/development/icons-css3-transitions/index3.html).

## Style 4 – Solid Background To Thin Border Animation

For this demo, icons will have a solid background colour by default. On hover, the background will transition from the centre into a thin border. As I mentioned before, I'm not a fan of transitioning borders, so this time, I'll re-create the effect using the `box-shadow` property, and setting it to `inset`. The box shadow will exist on a pseudo element behind the icon, which will exist on another pseudo element. The shadow itself will be maxed out with no blur, depicting a solid background. On hover, we'll change it to a smaller value. Here's the CSS:

```css
.icon-border {
  position: relative;
}

.icon-border::before,
.icon-border::after {
  display: block;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  content: "";
}

.icon-border::before {
  z-index: 1;
  -webkit-transition: box-shadow 0.3s;
          transition: box-shadow 0.3s;
}

.icon-border::after {
  z-index: 2;
  background: url("../img/icons4.png");
  background-image: url("../img/icons4.svg"), none;
}

/* facebook */
.icon-border.facebook::before {
  box-shadow: inset 0 0 0 48px #3b5998;
}

.icon-border.facebook:hover::before {
  box-shadow: inset 0 0 0 4px #3b5998;
}

.icon-border.facebook::after {
  background-position: 0 0;
}

/* twitter */
.icon-border.twitter::before {
  box-shadow: inset 0 0 0 48px #4099ff;
}

.icon-border.twitter:hover::before {
  box-shadow: inset 0 0 0 4px #4099ff;
}

.icon-border.twitter::after {
  background-position: -96px 0;
}

/* google plus */
.icon-border.googleplus::before {
  box-shadow: inset 0 0 0 48px #d34836;
}

.icon-border.googleplus:hover::before {
  box-shadow: inset 0 0 0 4px #d34836;
}

.icon-border.googleplus::after {
  background-position: -192px 0;
}

/* github */
.icon-border.github::before {
  box-shadow: inset 0 0 0 48px #333333;
}

.icon-border.github:hover::before {
  box-shadow: inset 0 0 0 4px #333333;
}

.icon-border.github::after {
  background-position: -288px 0;
}

/* rss */
.icon-border.rss::before {
  box-shadow: inset 0 0 0 48px #ee802f;
}

.icon-border.rss:hover::before {
  box-shadow: inset 0 0 0 4px #ee802f;
}

.icon-border.rss::after {
  background-position: -384px 0;
}
```

For quick reference, head to the demo link for this style [here](http://callmenick.com/development/icons-css3-transitions/index4.html).

## Style 5 – Rotating Cube Icon

This style makes use of [CSS3 perspectives](http://callmenick.com/post/css-transitions-transforms-animations-perspective/), enabling us to create shapes that look like they have depth in the 3D plane. I'll use the default element as the container, and the two pseudo elements as the front and bottom faces of the cube. The cube will rotate about the x-axis on hover, causing a neat effect. Here's the CSS:

```css
.icon-cube {
  position: relative;
  -webkit-perspective: 800px;
          perspective: 800px;
  overflow: visible;
}

.icon-cube::before,
.icon-cube::after {
  display: block;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: url("../img/icons5.png");
  background-image: url("../img/icons5.svg"), none;
  content: "";
  -webkit-transition: all 0.3s;
          transition: all 0.3s;
}

.icon-cube::before {
  z-index: 2;
  background-color: #595959;
}

.icon-cube::after {
  z-index: 1;
  opacity: 0;
  -webkit-transform: translateY(48px) rotateX(-90deg);
          transform: translateY(48px) rotateX(-90deg);
}

.icon-cube:hover::before {
  opacity: 0;
  -webkit-transform: translateY(-48px) rotateX(90deg);
          transform: translateY(-48px) rotateX(90deg);
}

.icon-cube:hover::after {
  opacity: 1;
  -webkit-transform: rotateX(0);
          transform: rotateX(0);
}

/* facebook */
.icon-cube.facebook::before,
.icon-cube.facebook::after {
  background-position: 0 0;
}

.icon-cube.facebook::after {
  background-color: #3b5998;
}

/* twitter */
.icon-cube.twitter::before,
.icon-cube.twitter::after {
  background-position: -96px 0;
}

.icon-cube.twitter::after {
  background-color: #4099ff;
}

/* google plus */
.icon-cube.googleplus::before,
.icon-cube.googleplus::after {
  background-position: -192px 0;
}

.icon-cube.googleplus::after {
  background-color: #d34836;
}

/* github */
.icon-cube.github::before,
.icon-cube.github::after {
  background-position: -288px 0;
}

.icon-cube.github::after {
  background-color: #333333;
}

/* rss */
.icon-cube.rss::before,
.icon-cube.rss::after {
  background-position: -384px 0;
}

.icon-cube.rss::after {
  background-color: #ee802f;
}
```

For quick reference, head to the demo link for this style [here](http://callmenick.com/development/icons-css3-transitions/index5.html).

## Browser Support

These icon styles all rely heavily on pseudo elements, transitions, and transforms, so check for browser support before implementing. Of course, a fallback to a plain square icon with background image is very easy, and transitions/transforms will just be neglected in unsupported browsers.

## Wrap Up

And that's a wrap! Don't forget, you can view the demo and download the source by clicking the links below, and if you have and questions, comments, or feedback, you can also leave them below.

<p class="text-align--center">
<a href="http://callmenick.com/development/icons-css3-transitions/icons-css3-transitions-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/development/icons-css3-transitions/" class="button button--inline-block button--medium">View Demo</a>
</p>