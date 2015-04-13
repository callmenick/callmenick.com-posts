<p class="text-align--center">
<a href="http://callmenick.com/_development/css-diagonal-menu/css-diagonal-menu-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/css-diagonal-menu/" class="button button--inline-block button--medium">View Demo</a>
</p>

## The Age-Old Horizontal Navigation Menu

The horizontal navigation menu is an all-time classic, and people have found ways to keep them interesting and stylish over and over again. It’s functional and practical, so there’s no surprise that we see them in use all the time. In the past though, when we wanted different target shapes, we required some imagery, preferably in the form of an image sprite. With the responsive age upon us though, image sprites are just not cutting it anymore. Fear not, CSS3 comes to the rescue again.

In this tutorial, we’re going to change up the classic horizontal navigation menu by giving the targets a diagonal (or slightly skewed) appearance. We’ll take on a mobile-first approach, so on small screens, we’ll have just the un-skewed text links. As we reach our minimum-width break point, the navigation elements will transition nicely into skewed items. We’ll use some CSS3 transitions and transforms to achieve the desired effect, and the click/tap target will be exactly the shape of the skewed element (as opposed to a regular box with a fake skewed background). This will give us our pure CSS diagonal menu effect, which is a nice alternative styling technique to the classic horizontal menu. Let’s dig in!

## Creating The CSS Diagonal Menu

The markup is nothing we haven’t seen before – an unordered list inside a `nav` tag. The only difference is that the content inside the link must be wrapped in a `span` tag. This serves a purpose, and we’ll see why further down when we look at the CSS. Here’s the markup:

```html
<nav>
  <ul>
    <li><a href="#"><span>Home</span></a></li>
    <li><a href="#"><span>Tutorials</span></a></li>
    <li><a href="#"><span>Snippets</span></a></li>
    <li><a href="#"><span>Articles</span></a></li>
    <li><a href="#"><span>Resources</span></a></li>
    <li><a href="#"><span>Contact</span></a></li>
  </ul>
</nav>
```

Pretty straightforward stuff. Now let’s give our elements some style. We’re going to start off at the smallest screen widths, and work our way up. We’ll use one breakpoint at 690px, giving us a nice responsive layout. Bigger screens will see the diagonal menu in full effect, and smaller screens will have a regular inline text menu. This is done for simplicity’s sake. You can, of course, give the menu a little more style at smaller screen widths, or include the skew as well with smaller dimensions.

Our list elements are set to `display: inline-block`, and our a elements are set to `display: block` to fill up the space. The `span` tags are also set to `display: block` for the same reason. At our breakpoint, we apply a `transform: skewX(n deg)` to our list elements, where “n” is the skew angle. In order to get the text un-skewed though, we need to set a negative skew of “-n” degrees on the `span` elements. We can easily set this negative skew on the `a` elements, but this would unfortunately reset the click target to a box shape. Here’s the CSS:

```css
nav ul {
  list-style: none;
  margin: 0;
  padding: 0;
  text-align: center;
}
nav li {
  display: inline-block;
  margin: 0 5px;
  transition: all 0.3s;
}
nav a {
  display: block;
  color: #285064;
  transition: all 0.3s;
}
nav a:hover {
  color: #12242d;
}
nav span {
  display: block;
}

@media all and (min-width: 690px) {
  nav li {
    margin: 0;
    transform: skewX(-10deg);
  }
  nav a {
    padding: 10px 20px;
    color: #fff;
    background-color: #285064;
  }
  nav a:hover {
    color: #fff;
    background-color: #12242d;
  }
  nav span {
    transform: skewX(10deg);
  }
}
```

And voila, a pure CSS diagonal menu! Neat-o.

## Support & Fallbacks

Support for this of course depends on CSS3 transition/transform browser support, so you’re looking at IE9 and up. The fallback is very graceful though, and doesn’t require any work at all. In unsupported browsers, the elements will just display as regular rectangular elements.

## Wrap Up

And that’s a wrap! With some simple CSS3 techniques, we were able to add a little extra punch to our horizontal navigation menu. I hope you enjoyed this tutorial, and don’t forget you can download the source and view the demo by clicking the links below! If you have any questions, comments, or feedback, feel free to leave them below. Thanks for stopping by.

<p class="text-align--center">
<a href="http://callmenick.com/_development/css-diagonal-menu/css-diagonal-menu-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/css-diagonal-menu/" class="button button--inline-block button--medium">View Demo</a>
</p>