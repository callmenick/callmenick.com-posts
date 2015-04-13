This snippet will allow you to remove default WordPress gallery CSS styles, and add your own. The first part is a filter in our functions.php file, and the second is a list of styles to get you started for the gallery styling.

## The Functions File

Add this line of code in your functions.php file:

```language-php
// clear default wordpress gallery stuff
add_filter( 'use_default_gallery_style', '__return_false' );
```

## The CSS File

These CSS classes will get you started with re-styling your gallery the way you want it. The trend is fairly obvious depending on the number of columns you give your gallery, so style accordingly. Here’s what the CSS looks like:

```language-css
.gallery {

}
.gallery-item {
    float:left;
}
.gallery-columns-0 .gallery-item {
    width: 100%;
}
.gallery-columns-1 .gallery-item {
    width: 100%;
}
.gallery-columns-2 .gallery-item {
    width: 50%;
}
.gallery-columns-3 .gallery-item {
    width: 33.33%;
}
.gallery-columns-4 .gallery-item {
    width: 25%;
}
.gallery-columns-5 .gallery-item {
    width: 20%;
}
/* etc etc etc */
```

And that’s it! Some more control over the appearance of your galleries. Consider checking out the WordPress default caption classes to add a little more niceness to your galleries. Also, don’t forget, WordPress spits out the gallery in a definition list, so make sure to target and reset those styles to suit your needs. Everything is wrapped inside a .gallery class, so with that in mind, you should have no problem targeting all the child elements.