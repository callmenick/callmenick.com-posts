[Previously](http://callmenick.com/post/box-sizing-reset-border-box), I wrote in some detail about the default box model we're given with, its strange behaviour, and how we can (and should) reset it to spare us quite a bit. To recap quickly, the box model refers to the space occupied by an element. It takes into consideration the margin, padding, border, and content. By default, if a width is explicitly set on an element and some padding and a border are added, the box will grow in size to compensate for these dimensions, making us perform countless pointless mathematical operations to get sizing correct. In the aforementioned article, I showed you some reset styles to help out greatly.

## The Original Reset

The original reset looked something like this:

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

I can't stress enough how helpful this is, and it's really been one of my greatest CSS "gotchas" to date. At this point, CSS and I truly became friends. There's absolutely nothing wrong with this, and 99.9999% of times, you're unlikely to encounter any problems. But in the spirit of best practices and avoiding that 0.0001% chance of conflict, there's now a better way to handle this reset.

## The New Reset

Imagine that you import some external component, and that component just so happens to explicitly declare the following rule:

```css
.component {
  box-sizing: content-box;
}
```

You might just run into some trouble. Instead, we should make use of inheritance. That way, our own stylesheet will inherit the `box-sizing` reset down from the top of the tree, and external components won't be affected. Here it is in action:

```css
*,
*::before,
*::after {
  box-sizing: inherit;
}

html {
  box-sizing: border-box;
}
```

Same result, but with an ever so slight optimisation boost and a 1-up on the best practices scale.

## Credits

I came across this trick today on [CSS Tricks](https://css-tricks.com/inheriting-box-sizing-probably-slightly-better-best-practice/), which is a phenomenal resource in general. They paid tribute to a [comment on the Treehouse blog](http://blog.teamtreehouse.com/box-sizing-secret-simple-css-layouts#comment-50223) posted by [Jonathan Neal](http://www.jonathantneal.com/), saying that's where they first came across the idea. The article on CSS Tricks includes a little more info and a CodePen demo too, so check it out, and change up that old reset of yours!