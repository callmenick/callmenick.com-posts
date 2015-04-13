## WordPress Excerpt Default Functionality

The default WordPress excerpt limits your post to 55 characters, ends it with “[...]“, and wraps it in p tags. This isn’t always desirable. If you write your own custom excerpts for example, and they’re always more than 55 characters, you wouldn’t want WordPress to truncate it. If you also wanted the excerpt length longer in general, you won’t want default functionality. On top of all that, I often find myself wanting an unwrapped excerpt. Just the excerpt, plain ol’ text, no HTML tags. This way I can put it where I want, and style it accordingly. This snippet will allow us to edit the default WordPress excerpt length and “more” text. It will also strip out the automatic p tag. It should be placed inside your functions.php file in your theme’s folder. Then, instead of calling the regular excerpt function `the_excerpt()`, call the new and improved function `custom_excerpt()` inside the loop.

```language-php
function custom_excerpt_length( $length )       { return 100; }     // change the excerpt length
function new_excerpt_more( $more )              { return ' ...'; }  // change the "more" text that follows the excerpt

function custom_excerpt() {
    add_filter( 'excerpt_length', 'custom_excerpt_length' );        // add filter for length
    add_filter('excerpt_more', 'new_excerpt_more');                 // add filter for "more" text
    remove_filter('the_excerpt','wpautop');                         // remove auto p wrapping
    return the_excerpt();                                           // return the_excerpt
}
```

```language-php
<?php while ( have_posts() ) : the_post(); ?>
    <?php echo custom_excerpt(); ?>
<?php endwhile; ?>
```