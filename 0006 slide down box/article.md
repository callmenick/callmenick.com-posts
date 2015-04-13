In this lab, we demonstrate how to make a div box slide down on click using the JQuery click function. When we click our login button, a div box slides down with the login form, allowing users to login on the same page instead of going to a different page. To do this, we create a login link in our navigation, hide the login box with css, and then apply the toggle function after the login button is clicked. This slides down the menu, and swaps out the login button class to show an expanded image. Here it is!

## The Concept

The concept is fairly simple. We create a navigation bar with regular navigation links, and a login link too. However, to have a smoother user interface and work flow, instead of the login button directing the user to a new page, it toggles the login form on the same page. The user can then fill out the form and submit it on the same page, making the login process seamless and smooth. We style our navigation like we usually would, with some `ul` tags and list items. Then our login button gets special treatment, with a plus sign image, indicating that it is an expandable box. When the user clicks the login box and it slides down, the plus image turns to a minus image, indicating that the user can click the login button again to slide the box up. Here’s what our HTML should look like for the basic navigation.

```language-markup
<nav id="nav-container" class="clearfix">
    <nav id="links">
        <ul>
            <li><a href="">nav 1</a></li>
            <li><a href="">nav 2</a></li>
            <li><a href="">nav 3</a></li>
            <li><a href="">nav 4</a></li>
            <li><a href="">nav 5</a></li>
        </ul>
    </nav>

    <nav id="login">
        <p>login</p>
        <div id="login-form">
            <form action="#" method="post">
                <label for="name">Username:</label>
                <input type="text" id="name" />

                <label for="password">Password:</label>
                <input type="password" id="password" />

                <input type="submit" value="Log In" />
            </form>
        </div>
    </nav>
</nav>
```

Next up is our CSS. We style the links on the left normally, using an unordered list and list items. On the right though, we go about it a bit differently. We style the login text to have a background image of the plus sign. We also position this login text relatively. The reason we do this is so that when we absolutely position our login form, we can position it relative to the button. Adding absolute position to the login form now, we set the top and left values to meet our desire. The top value is set to 40px because this is the height of the navigation bar, and the left value is set to 0, because we want it left aligned to the login button. Finally, we cap it off by adding `display:none`, thus hiding it from view when we first load the page. Here’s what our CSS should look like.

```language-css
/* =The style for the lab
-------------------------------------------------------------- */
nav#nav-container {
    border-top:solid 1px rgb(220,220,220);
    border-bottom:solid 1px rgb(220,220,220);
    margin-bottom:18px;
    line-height:40px;
}

nav#links {
    width:724px;
    float:left;
}

nav#links ul {
    list-style-type:none;
    margin:0;
    padding:0;
}

#links ul li {
    margin:0 12px;
    padding:0;
    float:left;
    text-align:center;
}

#links ul li a {
    display:block;
    padding:0 12px;
}

nav#login {
    width:300px;
    float:left;
    position:relative;
}

#login p {
    margin:0;
    padding-left:24px;
    cursor:pointer;
    background-image:url('../img/open.png');
    background-position:left;
    background-repeat:no-repeat;
}

#login p.close {
    background-image:url('../img/close.png');
}

#login-form {
    position:absolute;
    z-index:999;
    background-color:#fff;
    border:solid 1px rgb(220,220,220);
    padding:12px;
    width:300px;
    box-shadow:1px 1px 4px rgb(220,220,220);
    top:40px;
    left:0;
    display:none;
}

#login-form label {
    display:block;
}

#login-form input[type="text"],
#login-form input[type="password"] {
    display:block;
}

#login-form input,
#login-form label {
    margin-bottom:4px;
}
```

Now for the fun part. To add the interactivity, we’re going to make use of three JQuery functions – `click()`, `slideToggle()`, and `toggleClass()`. Make sure you have JQuery loaded, and then open up a set of script tags. First, we target the `p` tag inside the `#login` navigation section, and apply the `click()` function to it. Then, we give the `#login-form` box the function `slideToggle()`, which toggles it into view. Then, targeting the current p tag in question by using `$(this)`, we apply the function `toggleClass()` to it, toggling the new class. When the button is clicked again, the class gets “un-toggled”, and the box slides back up. Here’s what the JQuery should look like.

```language-javascript
$(document).ready(function() {
    $('#login p').click(function() {
        $('#login-form').slideToggle(300);
        $(this).toggleClass('close');
    });
}); // end ready
```

And that’s about it! Simple and effective, and it can be used on anything from login forms to contact forms to newsletter signups, the possibilities are endless. I hope you enjoyed this tutorial and learned from it, and please feel free to view the demo or download the source below. Also, if you liked what you’ve read, leave a comment below!