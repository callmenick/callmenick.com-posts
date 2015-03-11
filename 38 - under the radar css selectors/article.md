## CSS Selectors Are Great

Sometimes we want to achieve something a bit different to the norm in our web projects, and we over-think it a bit. If we take a simple look at some of the various CSS references out there, we’ll come across some super interesting CSS selectors that tend to slip under the radar. These selectors can fall right in line with the whole progressive enhancement movement, giving a little extras to users who have support for them. Let’s take a look at some of these under the radar CSS selectors. I’ll be referencing a lot to the [CSS developer guide from MDN](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS). It’s a fantastic reference, and you can find a lot of good and true maintained information here. Let’s dig in!

## 1) Element::before and element::after

The `::before` and `::after` CSS selectors have become two of my best friends when it comes to some neat CSS stuff. They allow you to truly separate style from content in some cases, leaving only the content to shine through. As long as you’re using it for aesthetic embellishments, the fallback in unsupported browsers is graceful (the embellishment just won’t show up). If you’re using it for structural purposes, make sure you have a suitable fallback. I demoed a basic but practical use of these CSS selectors in one of my more beginner tutorials - [“Styling Blockquotes with CSS Pseudo Selectors”](http://callmenick.com/2013/03/08/styling-blockquotes-with-css-pseudo-classes/). Here’s a pen that you can play around with though.

<p data-height="257" data-theme-id="5513" data-slug-hash="DjrAv" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/callmenick/pen/DjrAv/'>Pseudo Classes ::before and ::after</a> by Nick Salloum (<a href='http://codepen.io/callmenick'>@callmenick</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### The Underlying Beauty

The real beauty of using these pseudo CSS selectors is not having to repeat yourself in your HTML, and maintain it when something changes. You add the classes once in your stylesheet, and apply it wherever you want. Need a change? Just edit the one class in your CSS and it’ll change through out. Neat-o.

## 2) Element::selection

Ever been to a web page and selected the text, and seen the selection come up in a different colour than the dullish looking default browser style? Bright pink with white text, a dark grey with yellow text, or something like that? This is achieved by using the CSS selector `::selection` on whatever elements you want the selection style to exist on. Want to target your entire document? Use the universal `*` selector. Here’s an example of the `::selection` selector in action:

<p data-height="257" data-theme-id="5513" data-slug-hash="AuhGp" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/callmenick/pen/AuhGp/'>CSS Selector - Selection</a> by Nick Salloum (<a href='http://codepen.io/callmenick'>@callmenick</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### Be Mindful of Future Changes

According to the MDN, this feature is non-standard. They suggest not using it on production sites just yet, and mention that incompatibilities between implementations and the behaviour may change in the future. That shouldn’t prevent you from using it though…just be mindful of this, and make sure that you adjust accordingly in the future.

## 3) Element::first-letter

If you want to add a storybook type look to the text in your document, you might be interested in “drop cap” styling. Drop cap styling is when the first letter of the chapter (or page/document in our  case) is styled differently. Initial thoughts might lead us to thinking we need a span around that first letter. Tedious. Luckily, the CSS selector `::first-letter` handles this for us. It doesn’t actually add anything into the DOM, but you can imagine it working as if it were wrapping a span around the first letter of your tag, and styling it with CSS. Check out the example below to see it in action:

<p data-height="257" data-theme-id="5513" data-slug-hash="dFumH" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/callmenick/pen/dFumH/'>dFumH</a> by Nick Salloum (<a href='http://codepen.io/callmenick'>@callmenick</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### Some Points to Note

* This selector only has an effect on elements with a display value of block, inline-block, table-cell, list-item or table-caption. In all other cases, `::first-letter` has no effect.
* If you’re using the `:before` selector on your element, you might run into some conflicts if the `:before` pseudo class is content-containing. The `::first-letter` selector will try to target this instead.

## 4) Element::first-line

The `::first-line selector` is an interesting one, because it allows us to actually give a unique style to the first line of an element. I personally haven’t thought of a logical use for this selector, but I’m sure there are some creative things that can be done. Maybe you wanted to present the first line in a different colour or font. The cool thing about this selector though is that it’s dynamic, meaning that when you adjust the browser window, the first line automatically readjusts. Check out this example to see it in action:

<p data-height="257" data-theme-id="5513" data-slug-hash="iILeK" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/callmenick/pen/iILeK/'>first line selector</a> by Nick Salloum (<a href='http://codepen.io/callmenick'>@callmenick</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### Some Points to Note

* A ::first-line selector only has meaning on block level elements
* Only a small subset of CSS properties can be applied to the ::first-line selector
See the [MDN reference](https://developer.mozilla.org/en-US/docs/Web/CSS/::first-line) for more information.

## 5) Element:first-of-type, :last-of-type, and :nth-of-type

The CSS selector `:first-of-type` and `:last-of-type` have equal but opposite effects. The `:first-of-type` CSS selector targets the first sibling of its type in the list of children selectors. That is, if a `div` is targeted, and that `div` has multiple `span` and `strong` tags, the first of each `span` and `strong` tag will be targeted. You can target all elements of a type, or all children elements of an element. The `:last-of-type` works in the opposite way, and the `:nth-of-type` allows you to select which nth value selector to target. Here’s an example:

<p data-height="257" data-theme-id="5513" data-slug-hash="Ateqr" data-default-tab="result" class='codepen'>See the Pen <a href='http://codepen.io/callmenick/pen/Ateqr/'>first of type selector</a> by Nick Salloum (<a href='http://codepen.io/callmenick'>@callmenick</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//codepen.io/assets/embed/ei.js"></script>

### Some Notes

* The support for these selectors are high, so use them and be creative!

## Wrap Up

The capabilities of CSS are growing constantly, and it’s important to always be wary of what’s available and the capability. If a selector is low in support, be sure to only use it in a way that enhances a user experience, but not makes or breaks it. Don’t use nth-type selectors for structural components, for instance, if you’re supporting IE7 and down (good luck if you still are). I hope this post gave you some insight into some lesser known CSS selectors! The capabilities are endless. Feel free to post your suggestions below, and thanks for reading.