<p class="text-align--center">
<a href="http://callmenick.com/_development/simple-responsive-navigation/simple-responsive-navigation.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/simple-responsive-navigation/" class="button button--inline-block button--medium">View Demo</a>
</p>

In this lab, we build a simple responsive navigation menu using HTML, CSS, and a bit of jQuery. This navigation menu is one level deep, and at a certain screen width, it will turn into a neat “menu” button. When we click this button, the navigation will be toggled.

## Getting Started

With the web being accessed across various devices with various screen sizes, it’s important we adapt and start building responsively. Navigation menus are important on any website, but can sometimes look obtrusive on small devices. A neat little trick for mobile devices is to have a button that toggles the navigation menu at smaller screen widths, while showing the full menu at larger screen widths. With some neat CSS styles and some help from jQuery, we can achieve this easily. First of all, let’s take a look at some HTML markup.

```html
<nav id="nav-main">
    <ul>
        <li><a href="">Home</a></li>
        <li><a href="">About</a></li>
        <li><a href="">Gallery</a></li>
        <li><a href="">Tutorials</a></li>
        <li><a href="">Contact</a></li>
    </ul>
</nav>
<div id="nav-trigger">
    <span>Menu</span>
</div>
<nav id="nav-mobile"></nav>
```

## Notes About The HTML Markup

In our markup, we see 3 things going on. The first is our navigation menu, set inside a `nav` tag with and id of `nav-main`. The other two things we see are a `div` with an id of `nav-trigger`, and another `nav` tag with an id of `nav-mobile`. The main navigation will be visible on wider screen widths, while the mobile navigation will be visible on smaller screen widths, and will be triggered by the navigation trigger. The mobile navigation tag is left empty on purpose. This serves two purposes -

* We don’t want to have duplicate markup in our HTML
* We don’t want to have to keep track of maintaining the exact same menu twice

So how do we get the main menu contents into the navigation menu? jQuery, of course. But first, a little bit of CSS.

```css
/* =Nav
-------------------------------------------------------------- */
#nav-trigger {
  display: none;
  text-align: center; }
  #nav-trigger span {
    display: inline-block;
    padding: 10px 30px;
    background-color: #ff0032;
    color: white;
    cursor: pointer;
    text-transform: uppercase; }
    #nav-trigger span:after {
      display: inline-block;
      margin-left: 10px;
      width: 20px;
      height: 10px;
      content: "";
      border-left: solid 10px transparent;
      border-top: solid 10px #fff;
      border-right: solid 10px transparent; }
    #nav-trigger span:hover {
      background-color: #e6002d; }
    #nav-trigger span.open:after {
      border-left: solid 10px transparent;
      border-top: none;
      border-bottom: solid 10px #fff;
      border-right: solid 10px transparent; }

nav {
  margin-bottom: 30px; }

nav#nav-main {
  background-color: #ff0032;
  padding: 10px 0; }
  nav#nav-main ul {
    list-style-type: none;
    margin: 0;
    padding: 0;
    text-align: center; }
  nav#nav-main li {
    display: inline-block;
    border-right: solid 1px #cc0028;
    padding: 0 5px; }
    nav#nav-main li:last-child {
      border-right: none; }
  nav#nav-main a {
    display: block;
    color: white;
    padding: 10px 30px; }
    nav#nav-main a:hover {
      background-color: #e6002d;
      color: #fff; }

nav#nav-mobile {
  position: relative;
  display: none; }
  nav#nav-mobile ul {
    display: none;
    list-style-type: none;
    position: absolute;
    left: 0;
    right: 0;
    margin-left: auto;
    margin-right: auto;
    text-align: center;
    background-color: #ff0032; }
  nav#nav-mobile li {
    display: block;
    padding: 5px 0;
    margin: 0 5px;
    border-bottom: solid 1px #cc0028; }
    nav#nav-mobile li:last-child {
      border-bottom: none; }
  nav#nav-mobile a {
    display: block;
    color: white;
    padding: 10px 30px; }
    nav#nav-mobile a:hover {
      background-color: #e6002d;
      color: #fff; }

/* =Media Queries
-------------------------------------------------------------- */
@media all and (max-width: 900px) {
  #nav-trigger {
    display: block; }

  nav#nav-main {
    display: none; }

  nav#nav-mobile {
    display: block; }
}
```

## Some Notes About The CSS

Our main navigation is a straightforward unordered list. The styling applied to it is straightforward also, and can be easily adjusted to suit your needs. The mobile navigation menu takes on a more linear style though, where each list item falls under the other. We need a bit of foresight into what we want to achieve here. In our case, we want the mobile navigation menu to be a dropdown that is triggered when the `nav-trigger` is clicked. For this reason, we set the mobile navigation container to a position:relative, and the list to a `position:absolute`. Also, on regular view (i.e. large widths), we don’t want the navigation toggle or the mobile menu to show at all. For this reason, we set these two to `display:none`. We also set out mobile unordered list to display:none, so that we can accomplish a jQuery slide down effect later on.

Now, further down in the CSS, we look at our media query. After some experimenting, I noticed that an optimum screen width for my navigation to transform into the mobile version would be at 900px. Therefore, at this width, I change the display properties of the main navigation, mobile navigation, and navigation trigger, so that the main `nav` is hidden, and the mobile `nav` and trigger are now visible. Remember, we set our mobile navigation unordered list to be hidden by default too, so even though the mobile `nav` is now displayed, the list itself is hidden from view. We want this functionality because we only want the list to slide down into view when the navigation trigger is clicked. So how do we achieve this final piece of functionality? We use a little bit of Javascript/jQuery! Here is is -

```javascript
$(document).ready(function(){
    $("#nav-mobile").html($("#nav-main").html());
    $("#nav-trigger span").click(function(){
        if ($("nav#nav-mobile ul").hasClass("expanded")) {
            $("nav#nav-mobile ul.expanded").removeClass("expanded").slideUp(250);
            $(this).removeClass("open");
        } else {
            $("nav#nav-mobile ul").addClass("expanded").slideDown(250);
            $(this).addClass("open");
        }
    });
});
```

## Some Notes About The Javascript/jQuery

We place all our jQuery inside the document ready function, because we only want to change things up when all DOM contents have been loaded. The first step is to get the html from the main navigation menu into the mobile navigation menu. This is done easily with the `.html()`  function from jQuery.

Then, we look for a click event on the navigation trigger. When it is clicked, we do a few things -

* We check if our mobile navigation has a class of expanded.
* If it does, it means our mobile navigation is expanded. We need to remove this class and slide it back up, and we also need to remove the class open from our navigation trigger.
* If it doesn’t, it means our mobile navigation is not in sight, so we need to add an expanded class and slide it into view. We also need to add the class open to our navigation trigger.

## Wrap Up

And there you have it! A simple, one-level, responsive navigation menu, that functions nicely and fits neatly across all device screen sizes. I hope you enjoyed this tutorial! Feel free to download the source or view the demo by clicking the links below, and leave any questions, feedback, or comments below too.

<p class="text-align--center">
<a href="http://callmenick.com/_development/simple-responsive-navigation/simple-responsive-navigation.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/simple-responsive-navigation/" class="button button--inline-block button--medium">View Demo</a>
</p>