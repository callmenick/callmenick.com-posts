## The Importance of Archive Pages

Archive pages are a very good tool for any writer/blogger’s website. It gathers all your content into a neat, concise, and easy to traverse page. It’s important then that we display this page in a presentable and attractive manner. This snippet will show you how to create a WordPress custom archive page by using a custom query, and doing some conditional checks. We don’t just want to spit out the titles of all our posts on one page. Instead, let’s customise the loop to group our posts on a month by month basis. Each month will have a big heading, and underneath it, a list of all post titles, published dates, and permalinks to the posts from that month. If no posts got published that month, the loop would automatically leave out that heading.

## Creating Our Custom WordPress Archive Page

The first thing you’ll want to do is create a new custom template file, and name it “Archive Page”. The name of the actual file should be something like page-archive.php, and should exist in your theme folder. Load up your normal theme template for a page, and right where you’d run your normal loop, delete all the code there. We’re going to create a new instance of WP_Query, and customise the output. We’ll start by setting up our arguments array, then passing it into WP_Query. After that,  we’ll run the loop on our new query, and do some conditional checks. Let’s first look at the code, then dig into some explanations about what’s going on.

```php
<?php
/**
 * Template Name: Archive Page
 * The template for the archive page.
 *
 */
?>

<?php get_header(); ?>

<div class="content">  

  <h1>
    <?php the_title(); // this should output Archive Page ?>
  </h1>

  <?php

    // set up our archive arguments
    $archive_args = array(
      post_type => 'post',    // get only posts
      'posts_per_page'=> -1   // this will display all posts on one page
    );

    // new instance of WP_Quert
    $archive_query = new WP_Query( $archive_args );

  ?>

  <div class="loop-archive">

    <?php $date_old = ""; // assign $date_old to nothing to start ?>

    <?php while ( $archive_query->have_posts() ) : $archive_query->the_post(); // run the custom loop ?>

      <?php $date_new = get_the_time("F Y"); // get $date_new in "Month Year" format ?>

      <?php if ( $date_old != $date_new ) : // run the check on $date_old and $date_new, and output accordingly ?>
        <h2><?php echo $date_new; ?></h2>
      <?php endif; ?>

      <article <?php post_class(); // output a post article ?>>
        <h4><a href="<?php echo the_permalink(); ?>"><?php the_title(); ?></a></h4>
        <span><?php the_time("jS M, Y"); ?></span>
      </article>

      <?php $date_old = $date_new; // update $date_old ?>

    <?php endwhile; // end the custom loop ?>

  </div>

  <?php wp_reset_postdata(); // always reset post data after a custom query ?>

</div>

<?php get_sidebar(); ?>
<?php get_footer(); ?>
```

## Digging Into The Code

First, we’re creating a variable called `$date_old` as empty. This variable will be updated with each loop iteration. Then, we’re going to create a variable called `$date_new`, which will get assigned the date of the published post (in month-year format) on each iteration. We’re going to check if `$date_old == $date_new` at the beginning of each loop iteration. If they are equal, it means we’re still in the same month-year period. If not, it means we’ve advanced on month, and we should change the month output. These month outputs act as our custom archive section headings.

Under each month-year heading, we now output all posts filed under that month. I used the post title, published date, and the permalink on the post title itself. Remember, you’re inside a loop now so you can access anything you normally would be able to from the post object. The output is really up to you. Applying some basic CSS should get you the style you want.

## Wrapping Up

This little addition to your site can go a long way. Archive pages gives us an extra bonus, especially to users who are looking for old posts that they can’t seem to find. As long as you creatively and cleverly go about your display and creation of it, it can keep users more engaged on your site, and have them travel through your site more and more. I hope you enjoyed this tutorial on creating a  WordPress custom archive page! If you have any comments or suggestions, feel free to leave them below. Thanks for reading!