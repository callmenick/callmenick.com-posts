<p class="text-align--center">
<a href="http://callmenick.com/_development/css3-transitions-compilation/css3-transitions-compilation-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/css3-transitions-compilation/" class="button button--inline-block button--medium">View Demo</a>
</p>

## Introduction To CSS3

CSS3 allows us to do some amazing things, and there’s a common set of properties and functionality at our disposal that enable us to improve user interaction easily. The three main features I’m going to give examples for in this tutorial are CSS3 transitions, transforms, and animations. These examples are ready to drop into any of your projects, and should give you an idea of what’s possible in the world of CSS3. In the examples below, I’ll be using link targets as the source of user interaction. Very often, users have some kind of interaction with links, so this is a great place to start. The majority of the transitions/transforms/animations below will take place on hover, but one will be a repeat animation to demonstrate this side of animations. Let’s get started.

## Getting Started

Each section will contain a link, and each link will be targeted by a class name. There are nine in all, so that’s nine examples below. Our markup will be similar for all links, just with a different class name. Here’s a look at the HTML:

```html
<section>
  <a href="#" class="cmn-t-colour">Colour</a>
</section>

<section>
  <a href="#" class="cmn-t-bg">Background</a>
</section>

<section>
  <a href="#" class="cmn-t-border-radius">Border Radius</a>
</section>

<section>
  <a href="#" class="cmn-t-scale">Scale Up</a>
</section>

<section>
  <a href="#" class="cmn-t-translate-bshadow">Translate &amp; Box Shadow</a>
</section>

<section>
  <a href="#" class="cmn-t-border">Inner Border</a>
</section>

<section>
  <a href="#" class="cmn-t-shake">Lateral Shake</a>
</section>

<section>
  <a href="#" class="cmn-t-pulse">Constant Pulse</a>
</section>

<section>
  <a href="#" class="cmn-t-underline">Underline Me!</a>
</section>
```

For simplicity sake, we’re going to apply the transition property to everything. In practice though, only apply it to declarations that you want transitioned to boost efficiency a bit. In the individual examples, there’s a commented transition property that will be the real world declaration for that particular example. Here’s the CSS applied across all elements:

```css
section {
  margin: 0 0 20px 0;
  text-align: center;
}
section:last-child {
  margin-bottom: 0;
}
section a {
  display: inline-block;
  font-size: 16px;
  transition: all 0.2s;
}
```

There’s some media queries in the source code to make links bigger at bigger screen widths, so check those out or add your own queries. Remember, the CSS above and below does not include prefixed properties. You should include prefixed properties to maximise compatibility. The downloadable source code contains prefixed CSS. Now let’s get in to each of our transitions!

## \#1) Colour

![Colour](/files/2015-03/1_colour.png)

The colour transition is one of the most basic implementations of CSS3 transitions, and is commonly seen on inline text links. On hover, the colour will smoothly transition into a different one. Here’s our CSS:

```css
.cmn-t-colour {
  color: #ff3296;
  /* transition: color 0.2s; */
}
.cmn-t-colour:hover {
  color: #98004a;
}
```

## \#2) Background

![Background](/files/2015-03/2_background.png)

The background transition is also common, and adds a smooth effect when the background changes from one colour to another. This is more often seen on bigger link targets, like buttons. On hover, the background colour smoothly transitions from one colour to another. Here’s our CSS:

```css
.cmn-t-bg {
  padding: 20px 40px;
  color: #fff;
  background-color: #ff3296;
  /* transition: background 0.2s; */
}
.cmn-t-bg:hover {
  color: #fff;
  background-color: #98004a;
}
```

## \#3) Border Radius

![Border radius](/files/2015-03/3_border_radius.png)

This effect has a similar reasoning behind it as the background effect, working best on links and targets that are meant to be bigger and bolder and stand out from the pack. On hover, the rectangular border will smoothly transition into a rounded corner rectangle. Here’s the CSS:

```css
.cmn-t-border-radius {
  padding: 20px 40px;
  color: #fff;
  background-color: #ff3296;
  /* transition: border-radius 0.2s; */
}
.cmn-t-border-radius:hover {
  color: #fff;
  border-radius: 40px;
}
```

You can transition the other way too, by setting the initial `border-radius` to 40px, and the `:hover` `border-radius` to 0.

## \#4) Scale Up

![Scale up](/files/2015-03/4_scale_up.png)

So far, we’ve only looked at transitions on basic CSS properties. For the scale up effect, we’ll be using a CSS `transform`. On hover, the element will smoothly scale up to a slightly bigger size. Here’s the CSS:

```css
.cmn-t-scale {
  padding: 20px 40px;
  color: #fff;
  background-color: #ff3296;
  /* transition: transform 0.2s; */
}
.cmn-t-scale:hover {
  color: #fff;
  transform: scale(1.1);
}
```

To scale down, just set the scale value to something less than 1 on hover.

## \#5) Translate & Box Shadow

![Translate](/files/2015-03/5_translate_box_shadow.png)

This effect gives the illusion that the target is popping out of position. It will smoothly move down and to the right, with a darker colour appearing behind it marking its initial position. The box is moved with the CSS3 `transform` property `translate`, and the `box-shadow` is applied on hover too. The box shadow size will be the equivalent of the distance moved by the box. Here’s the CSS:

```css
.cmn-t-translate-bshadow {
  padding: 20px 40px;
  color: #fff;
  background-color: #ff3296;
  /* transition: box-shadow 0.2s, transform 0.2s; */
}
.cmn-t-translate-bshadow:hover {
  color: #fff;
  box-shadow: -10px -10px 0 #98004a;
  transform: translate(10px, 10px);
}
```

## \#6) Inner Border

![Inner border](/files/2015-03/6_inner_border.png­)

This effect smoothly creates a border from the edge slightly inward. We’ll actually use the `box-shadow` property to simulate the border, by insetting it. This way, the border will never interfere with the contents of our box, and the size of it either. Here’s the CSS:

```css
.cmn-t-border {
  padding: 20px 40px;
  color: #fff;
  background-color: #ff3296;
  /* transition: box-shadow 0.2s; */
}
.cmn-t-border:hover {
  color: #fff;
  box-shadow: inset 0 0 0 10px #98004a;
}
```

## \#7) Lateral Shake

![Lateral shake](/files/2015-03/7_lateral_shake.png)

This will be our first foray into creating our own animation, and applying it on hover. On hover, we want our link target to move left to right a couple times then back to the start position. We’ll use `translateX` to achieve this, nudging our block slightly right and left every 20% of the animation, then finally back to start at 100%. We’ll use the CSS `@keyframe` syntax to create our animation, then apply the `animation` on hover. Because our animation only uses transforms, we only need to apply the transition to the transforms in a real world scenario (as shown in the comments). Here’s the CSS:

```css
.cmn-t-shake {
  padding: 20px 40px;
  color: #fff;
  background-color: #ff3296;
  /* transition: transform 0.2s; */
}
.cmn-t-shake:hover {
  color: #fff;
  animation: shake .5s ease-in-out;
}
@keyframes shake {
  0% {
    transform: translateX(0);
  }

  20% {
    transform: translateX(-10px);
  }

  40% {
    transform: translateX(10px);
  }

  60% {
    transform: translateX(-10px);
  }

  80% {
    transform: translateX(10px);
  }

  100% {
    transform: translateX(0);
  }
}
```

## \#8) Constant Pulse

![Pulse](/files/2015-03/8_pulse.png)

This time, we’ll set an infinite animation loop on our element, causing it to pulse. We’ll use the `@keyframe` syntax again, and combine it with scale as seen before. Here’s the CSS:

```css
.cmn-t-pulse {
  padding: 20px 40px;
  color: #fff;
  background-color: #ff3296;
  animation: pulse 1s ease infinite;
  /* transition: transform 0.2s; */
}
.cmn-t-pulse:hover {
  color: #fff;
}
@keyframes pulse {
  0% {
    transform: scale(1);
  }

  50% {
    transform: scale(1.1);
  }

  100% {
    transform: scale(1);
  }
}
```

## \#9) Underline

![Underline](/files/2015-03/9_underline.png)

We’re gonna get a little creative with this effect to simulate an underline being drawn. On hover, the text will be smoothly underlined from left to right. We’ll simulate the actual line with the CSS pseudo class `::after`, and `transition` it from a `width` of 0 to 100%. Remember, this time we need to add the transition on the `::after` pseudo class too, as it wasn’t previously defined in our common CSS. Here’s the CSS:

```css
.cmn-t-underline {
  position: relative;
  color: #ff3296;
}
.cmn-t-underline:after {
  display: block;
  position: absolute;
  left: 0;
  bottom: -10px;
  width: 0;
  height: 10px;
  background-color: #98004a;
  content: "";
  transition: width 0.2s;
}
.cmn-t-underline:hover {
  color: #98004a;
}
.cmn-t-underline:hover:after {
  width: 100%;
}
```

## Wrap Up

And that’s a wrap, my starter collection of simple transitions, transforms, and animations ready to go into any project. These should give you some insight too, and allow you to get creative and make your own. I hope you learned something and enjoyed this tutorial. Feel free to leave any comments, questions, or feedback below, and don’t forget to check out the demo or download the source if you need it.

<p class="text-align--center">
<a href="http://callmenick.com/_development/css3-transitions-compilation/css3-transitions-compilation-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/css3-transitions-compilation/" class="button button--inline-block button--medium">View Demo</a>
</p>