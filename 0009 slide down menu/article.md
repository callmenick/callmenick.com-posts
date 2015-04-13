<p class="text-align--center">
<a href="http://callmenick.com/_development/slide-down-menu/slide-down-menu-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/slide-down-menu/" class="button button--inline-block button--medium">View Demo</a>
</p>

In this lab, we take on the classic drop down menu, but with a little animated slide using jQuery. The slide down and up animation adds a nice touch to the menu for the user, and the menu is also scalable to add multiple levels of menu items. Enjoy!

## Getting Started

Let’s first get started with some basic html markup. Here, we use an unordered list inside of a `nav`  tag. We now construct our unordered list in typical fashion. A bunch of `li`  and `a`  tags, nested inside a `ul`  tag. Then, for further sub-navigation items, we nest another `ul` tag and more `li` and `a` tags. Our navigation structure should now look something like this -

```html
<nav>
    <ul>
        <li><a href="#">Nav 1</a></li>
        <li><a href="#">Nav 2</a></li>
        <li class="dropdown">
            <a href="#">Nav 3</a>
            <ul class="sub-menu">
                <li><a href="#">Nav 3.1</a></li>
                ...
                <li class="dropdown">
                    <a href="#">Nav 3.4</a>
                    <ul class="sub-menu">
                        <li><a href="#">Nav 3.4.1</a></li>
                        <li class="dropdown">
                            <a href="#">Nav 3.4.2</a>
                            <ul class="sub-menu">
                                <li><a href="#">Nav 3.4.2.1</a></li>
                                ...
                            </ul>
                        </li>
                        ...
                    </ul>
                </li>
                <li><a href="#">Nav 3.5</a></li>
            </ul>
        </li>
        ...
    </ul>
</nav>
```

Notice two things here. Firstly is that I added a class of `dropdown` to list items that have a sub-menu attached to them. Secondly is that I added a class of `sub-menu` to `ul`  tags that housed a sub-menu.

## Digging Into The CSS

As it stands, this menu is pretty terrible looking. It’s just a long, indented looking menu. So without further ado, let’s style it up with some CSS. The first step is to remove the default styling from the list tags, give our navigation container a little style, and position the list elements properly. I used the CSS property display:inline-block here, and gave them a `position:relative` (we’ll see why soon). So far, our CSS looks like this -

```css
nav {
    background-color:rgb(255,100,100);
    padding:10px 0;
}
nav ul {
    list-style-type:none;
    margin:0;
    padding:0;
}
nav ul li {
    display:inline-block;
    position:relative;
}
```

Our next step is to style and position the sub-navigation menus, so that they actually look like sub-navigation menus. To do this, we give the parent `ul`  of sub-navigation lists a `position:absolute`. This works, because we already specified that `li`  items have a position of relative, so the `ul` tags will be positioned relative to the `li` tags. This targets all sub-menus, but we need second-level sub-menus and higher to have a different style than first-level sub-menus, so our CSS compensates for that by targeting `li ul` and `li li ul`. Here’s what we add to our stylesheet now.

```css
/* sub navigation */
nav li ul {    
    background-color:rgb(225,75,75);
    position:absolute;
    left:0;
    top:40px; /* make this equal to the line-height of the links (specified below) */
    width:200px;
}
nav li li {
    position:relative;
    margin:0;
    display:block;
}
nav li li ul {
    position:absolute;
    top:0;
    left:200px; /* make this equal to the width of the sub nav above */
    margin:0;
}
```

Now, it’s just a simple task of styling our lists and links, and adding some little arrows for menu items that have a class of dropdown. We add this to our CSS now -

```css
/* style all links */
nav a {
    line-height:40px;
    padding:0 12px;
    margin:0 12px;
}
nav a {
    color:#fff;
    text-decoration:none;
    display:block;
}
nav a:hover,
nav a:focus,
nav a:active {
    color:rgb(50,50,50);
}

/* style sub level links */
nav li li a {
    border-bottom:solid 1px rgb(200,50,50);
    margin:0 10px;
    padding:0;
}
nav li li:last-child a {
    border-bottom:none;
}

/* show arrows for dropdowns */
nav li.dropdown > a {
    background-image:url('../img/arrow-down.png');
    background-position:right 20px;
    background-repeat:no-repeat;
}

nav li li.dropdown > a {
    background-image:url('../img/arrow-right.png');
    background-position:right 16px;
    background-repeat:no-repeat;
}
```

Our final task CSS wise is to hide our sub-level menus from automatic display, and this is made easy because we added the class sub-menu to our sub-menus! Just add this in to the CSS, and we’re on our way to completion now -

```css
/* hide sub menu links */
ul.sub-menu {
    display:none;
}
```

## Show and Slide-Down Effect with jQuery

Now, we want the sub-menus to slide down and display when a user hovers over the correct list item. This is exactly why we added the class of dropdown to menu items that had child menus. Make sure to include jQuery in your script, and then use the hover function to target list items that have the `dropdown` class. I used the `children()` jQuery function to then target direct child lists that had a class of `sub-menu`, and used the `slideup()` and `slidedown()` functions for display. Here’s what the Javascript looks like -

```javascript
$(document).ready(function() {
    $( '.dropdown' ).hover(
        function(){
            $(this).children('.sub-menu').slideDown(200);
        },
        function(){
            $(this).children('.sub-menu').slideUp(200);
        }
    );
}); // end ready
```

## Wrap Up

And voila! There you have it, a neat, functional, and user friendly slide down menu using CSS and jQuery. Don’t forget, you can download the source and view the demo by clicking the buttons below, and if you have any questions of comments, please feel free to leave them below. Thanks, and I hope you enjoyed! Coming soon – how to make a menu turn into a mobile friendly button that slides down on click.

<p class="text-align--center">
<a href="http://callmenick.com/_development/slide-down-menu/slide-down-menu-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/slide-down-menu/" class="button button--inline-block button--medium">View Demo</a>
</p>