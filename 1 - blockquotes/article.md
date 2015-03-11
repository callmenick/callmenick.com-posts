<p class="text-align--center">
<a href="http://callmenick.com/lab-demos/1-blockquote-styling/1-blockquote-styling.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/lab-demos/1-blockquote-styling/" class="button button--inline-block button--medium">View Demo</a>
</p>

## Getting Started

We’re going to use the tag blockquote to put our element in. According to W3, the blockquote element represents a quoted section. We’re going to strip down the default styling on our blockquote, and add some style of our own to make it stand out from the rest of the text. We’re also going to use the CSS pseudo classes :before and :after to give our blockquote a bit of style, by wrapping it in big open and close quotation marks. Let’s take a look at the HTML.

```html
<p>Lorem ipsum dolor sit amet, consectetur adipisicing elit...</p>
<p>
    <blockquote>Quote here...</blockquote>
</p>
<p>Lorem ipsum dolor sit amet, consectetur adipisicing elit...</p>
```

## Notes about the HTML

The HTML is super straightforward. We have some text, then our blockquote, then some more text. With that, let’s get into the CSS.

```css
/* =Structure
-------------------------------------------------------------- */
p {
    margin-bottom: 30px;
}
p:last-child {
    margin-bottom: 0;
}
 
/* =Blockquote
-------------------------------------------------------------- */
blockquote {
    position: relative;
    marign: 0;
    padding: 30px 120px;
    text-align: center;
    font-size: 30px;
}
blockquote:before, blockquote:after {
    position: absolute;
    width: 60px;
    height: 60px;
    font-size: 120px;
    line-height: 1;
}
blockquote:before {
    top: 0;
    left: 0;
    content: "\201C";
}
blockquote:after {
    top: 0;
    right: 0;
    content: "\201D";
}
```

## About The CSS

Our `p` tags are given some basic style for aesthetic purposes. The `blockquote` tag is given a padding and a bigger font size. We also relatively position it so that our pseudo classes can be absolutely positioned inside of the `blockquote` itself. We give both `:before` and `:after` a `position` of `absolute`, and a `width` and `height` of 60px. We increase the font size even more so that it fills it. Depending on the font you’re using, you might want to change this to suit. Then, the `:before` class gets positioned to the top-left of the `blockquote` container, with the open quotation unicode character in the content. The `:after` class goes to the top-right, with the closed quotation character in the content.

## Wrap Up

And there you have it! A simple wireframe for creating stand-out blockquotes. I added a few media queries in mine so that it looks good at smaller screen widths (as you can see in the demo), so adjust yours to suit. You can download the source code and leave any comments you like below. Thanks for checking it out!

<p class="text-align--center">
<a href="http://callmenick.com/lab-demos/1-blockquote-styling/1-blockquote-styling.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/lab-demos/1-blockquote-styling/" class="button button--inline-block button--medium">View Demo</a>
</p>