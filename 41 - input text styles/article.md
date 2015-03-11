<p class="text-align--center">
<a href="http://www.callmenick.com/tutorial-demos/input-text-styles/input-text-styles-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://www.callmenick.com/tutorial-demos/input-text-styles/input-text-styles-source.zip" class="button button--inline-block button--medium">View Demo</a>
</p>

## Adding Beauty To Your Forms

Website forms have long had a history of being boring, bland, and unappealing. In the past few years though, CSS and CSS3 have opened up some possibilities for us. I previously looked at [making stylish buttons with pure CSS](http://callmenick.com/2014/04/08/stylish-css-buttons/), and a lot of times, buttons follow input fields. Let’s take a look at some ways that we could prettify our forms, by implementing the following various CSS input text styles. Some of these are already popular around the web, and add an attractive and welcoming interface. Let’s get started!

## Common Markup & CSS

All our styles will take on a common set HTML markup and CSS. I’ll also be automatically displaying the :focus style by adding a .focus class. Each style is wrapped in a list element with a simple media query to make it respond to all screen sizes. I also reset all my input styles so that they are consistent across devices. Also, the CSS you read here isn’t prefixed, but the downloadable source is. If you’re copying straight from here, make sure you prefix properly! Let’s take a look.

## The HTML

```html
<section>
  <h2>Section Title</h2>
  <ul class="input-list style-1 clearfix">
    <li>
      <input type="text" placeholder=":input">
    </li>
    <li>
      <input type="text" placeholder=":focus" class="focus">
    </li>
  </ul>
</section>

<section>
  <h2>Section Title</h2>
  ...
</section>
```

## The CSS

```css
input[type="text"] {
  display: block;
  margin: 0;
  width: 100%;
  font-family: sans-serif;
  font-size: 18px;
  appearance: none;
  box-shadow: none;
  border-radius: none;
}
input[type="text"]:focus {
  outline: none;
}
```

Let’s take a look at some styles!

## \#1) The Glow

This is a pretty common style these days.  The input has a thin border, and when it is focused, an outer glow fades in. We’ll use the box-shadow property to achieve the glow, and fade it in with a CSS3 transition. Here’s what the CSS looks like.

### The CSS

```css
.style-1 input[type="text"] {
  padding: 10px;
  border: solid 1px #dcdcdc;
  transition: box-shadow 0.3s, border 0.3s;
}
.style-1 input[type="text"]:focus,
.style-1 input[type="text"].focus {
  border: solid 1px #707070;
  box-shadow: 0 0 5px 1px #969696;
}
```

Pretty simple and easy, right? Moving on.

## \#2) The Thick Border

The “thick border” style is common on minimalist sites or flat design sites. It’s easy, and just a thick border. The border will transition to a darker colour when focused using CSS3 transitions.

### The CSS

```css
.style-2 input[type="text"] {
  padding: 10px;
  border: solid 5px #c9c9c9;
  transition: border 0.3s;
}
.style-2 input[type="text"]:focus,
.style-2 input[type="text"].focus {
  border: solid 5px #969696;
}
```

Super simple, and looking good. On we go!

## \#3) The Double Border

The “double border” style is also common in flat design, and is a bit of a step up from the simple thick border style. We’ll use the border property as before, but also create the inner thin-lined border by using the box-shadow property.

### The CSS

```css
.style-3 input[type="text"] {
  padding: 10px;
  border: solid 5px #c9c9c9;
  box-shadow: inset 0 0 0 1px #707070;
  transition: box-shadow 0.3s, border 0.3s;
}
.style-3 input[type="text"]:focus,
.style-3 input[type="text"].focus {
  border: solid 5px #969696;
}
```
Neat! Amazing what a little bit of CSS can do…we just used a box-shadow to achieve a solid border look. Moving on!

## \#4) The Underline

This one is more along the lines of what you might see in a real-life pen & paper form. Just a line. We reset all borders, apply only a bottom border, and transition the colour to a darker one on focus.

### The CSS

```css
.style-4 input[type="text"] {
  padding: 10px;
  border: none;
  border-bottom: solid 2px #c9c9c9;
  transition: border 0.3s;
}
.style-4 input[type="text"]:focus,
.style-4 input[type="text"].focus {
  border-bottom: solid 2px #969696;
}
```

Simple, and effective.

## \#5) The Inset

This style, the “inset”, gives the user the feeling that the input is set into the page. It’s a beautiful and effective style that once required imagery to achieve. Now, plain and simple CSS and CSS3 can give us the effect we want. A simple transition will lighten it up when the user focuses.

### The CSS

```css
.style-5 input[type="text"] {
  padding: 10px;
  border: solid 1px #fff;
  box-shadow: inset 1px 1px 2px 0 #707070;
  transition: box-shadow 0.3s;
}
.style-5 input[type="text"]:focus,
.style-5 input[type="text"].focus {
  box-shadow: inset 1px 1px 2px 0 #c9c9c9;
}
```

Beautiful and effective!

## Wrap Up

As we’ve seen many times now, CSS and CSS3 open up a lot of options for us. Once we get creative and start thinking outside the box a bit, the possibilities really open up. I’ve only touched the surface here, so it’s up to you to use this knowledge and get creative. Thanks for reading, and feel free to view the demo, download the source, or leave feedback below.

<p class="text-align--center">
<a href="http://www.callmenick.com/tutorial-demos/input-text-styles/input-text-styles-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://www.callmenick.com/tutorial-demos/input-text-styles/input-text-styles-source.zip" class="button button--inline-block button--medium">View Demo</a>
</p>