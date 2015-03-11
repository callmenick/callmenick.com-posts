## Benefits of a Custom Loop

Learning how to create a custom WordPress loop with pagination is great. You won’t always want to use the default query on your page, so setting up a custom query on a page is quite handy. This is especially useful for times that you want to display all posts on a static front page, and you want to limit those posts to certain parameters. In our case, we’re going to assume the following parameters:

* __Type of post:__ post
* __\# of posts we want to display on our page:__ 5
* __Category that our post resides in:__ Tutorials
* __Pagination:__ Yes

We’re also going to set up a custom pagination function that will let us check if pagination exists/is available, and if so, we’ll display the number of pages, and links to successive pages for our query. We’re also going to be using this query on a Static Front Page, the most common place to set up a custom query. If you don’t know how to set up a static front page, I highly advise you take a quick read through this documentation on WordPress.

## The WP_Query Class

First, let’s head over to the class reference for WP_Query. Here, we can see everything we need to know about creating a new instance of WP_Query, and as a result, creating a new loop. It’s good to familiarise yourself with and bookmark this reference, because for any project requiring custom display of pages or posts, this is your go to guide. Of particular interest are the sections “methods and properties” and “parameters”. The methods and properties of our query give us ways in which we can interact with the query once we instantiate it, and the parameters are what we will set in the instantiation part of our query. These parameters will act as our arguments, and will allow us to tweak the query to display just the results we need.

## Interacting with WP_Query & Usage

To get started, you should read the interacting with WP_Query section to know how you can interact with it once you’ve set it up. Let’s take a look at the basic usage that is documented on the class reference page.

```language-php
<?php 
// the query
$the_query = new WP_Query( $args ); ?>

<?php if ( $the_query->have_posts() ) : ?>

  <!-- pagination here -->

  <!-- the loop -->
  <?php while ( $the_query->have_posts() ) : $the_query->the_post(); ?>
    <h2><?php the_title(); ?></h2>
  <?php endwhile; ?>
  <!-- end of the loop -->

  <!-- pagination here -->

  <?php wp_reset_postdata(); ?>

<?php else:  ?>
  <p><?php _e( 'Sorry, no posts matched your criteria.' ); ?></p>
<?php endif; ?>
```

In the code sample provided by WordPress, we see a few things happening. We create a variable called `$the_query`, and set it equal to a new instance of `WP_Query`. Inside `WP_Query`, we send a variable called `$args`. This variable contains all our arguments for the query. When the new query is set up, we can then interact with it. We begin by seeing if there are any posts to show based on our query. We do this by checking to see if `$the_query->have_posts()` returns true. If it does, then we can get into our loop. We do this by using a while loop with `have_posts()` as the condition. Then, in each iteration, we call `$the_query->the_post()` which sets up internal variables within `$the_query` and sets the global `$post` variable. Now, inside our loop, we can call on any loop functions and query methods and properties, and customise our loop. In the sample above, we’re outputting the title of each post in our query by using the function `the_title()`. It’s recommended that you restore the original post data of the page you’re on after using a custom query. This is done by calling the function  `wp_reset_postdata()`.

## Setting Up Our Arguments

As noted above, we have four arguments that we’d like to pass into our query. Here they are again for quick reference:

* __Type of post:__ post
* __\# of posts we want to display on our page:__ 5
* __Category that our post resides in:__ Tutorials
* __Pagination:__ Yes

Our arguments variable can be passed in as an array, so let’s do that. We’ll first create the array and assign it to a variable called $args, then pass that into our new instance of `WP_Query`. If we check out the parameters section in the table of contents, we’ll see links to everything we need. Let’s first check out “type parameters” which will allow us to set our post type.

## The Type Parameter

The type parameter allows us to show posts associated with certain type. There are a few available as seen in the documentation. In our case, we’re looking for a post. Luckily for us, this is the default type if you specify none, so we don’t actually need to specify it. But for the sake of redundancy and getting ourselves familiar, we’re going to set it anyway. So far, our arguments variable will look like this:

```language-php
$query_args = array(
  'post_type' => 'post'
);
```

Now, let’s set our category parameter. We’ll go check out the “category parameters” to see what we need to include.

## The Category Parameter

We know we want to display only posts from our category “Tutorials”, so let’s look at what’s available to us. We can immediately see that we have a parameter at our disposal that we can use. This is the “category_name” parameter. From the definition, we see that this parameter requires the category slug, not the actual name. In our WordPress admin panel, we can get the slug for our category in question. In this case, it is “tutorials”. We can now add this into our arguments variable, which should now look like this:

```language-php
$query_args = array(
  'post_type' => 'post',
  'category_name' => 'tutorials'
);
```

Let’s now take a look at displaying only a certain number of posts per page, and adding pagination our posts. Glancing at the table of contents, we see that we need to dig up the section called “pagination parameters”. This section tells us all about structuring posts into different pages.

## Pagination Parameters

This part of the `WP_Query` tends to confuse people, but we’re going to look into it and make it very simple. The first thing we want to do is limit the number of posts that show up on each page of our paginated query. To do this, we use the highly self explanatory “posts_per_page” parameter. In our case, we want 5 posts to show up per page. Our query will now look like this:

```language-php
$query_args = array(
  'post_type' => 'post',
  'category_name' => 'tutorials',
  'posts_per_page' => 5
);
```

Now, let’s look into paginating the posts. In the class reference, we see two definitions under the pagination parameters section. One for “paged”, and one for “page”. The two definitions are as follows:

* __paged (int)__ – number of page. Show the posts that would normally show up just on page X when using the “Older Entries” link.
* __page (int)__ – number of page for a static front page. Show the posts that would normally show up just on page X of a Static Front Page.

In our case, because we are setting up our query on a static front page, we need to use the “page” parameter. This is usually the part where a lot of people get mixed up, but the documentation is always here to the rescue. The following snippet of text is taken straight from the documentation:

> Pagination Note: Use get_query_var(‘page’); if you want your query to work in a Page template that you’ve set as your static front page…Display posts from current page on a static front page: `$paged = ( get_query_var('page') ) ? get_query_var('page') : 1;
$query = new WP_Query( array( 'paged' => $paged ) );`

How simple is that? They’ve given us the parameter we need to include in our query in order to allow for pagination. Now, our query should look like this for static home pages:

```language-php
$paged = ( get_query_var('page') ) ? get_query_var('page') : 1;
$query_args = array(
  'post_type' => 'post',
  'category_name' => 'tutorials',
  'posts_per_page' => 5,
  'paged' => $paged
);
```

Remember, for custom pages that are NOT static home pages, the `$paged` variable changes to this:

```language-php
$paged = ( get_query_var('paged') ) ? get_query_var('paged') : 1;
```

And with that, we’re ready to set up our loop, do our pagination checks, and display pagination links if they occur. They would only occur, of course, if our query yields more than the number of posts per page we specified. In this case, that number is 5. Let’s construct our loop!

## Constructing The WordPress Loop

Now, it’s time to construct the loop. Once we start the loop, we’ll have access to a host of WordPress functions that will allow us to display things like the title of the post, the post excerpt, and the post content. For the sake of simplicity, we’ll just output the post title. Of course, you might want to show more than this, so dig through the WordPress Function Reference for a host of functions available to you. Let’s take a look at “The Loop” documentation on the WordPress codex. After a quick glance, we can see that the loop is initialised like this:

```language-php
<?php if ( have_posts() ) : while ( have_posts() ) : the_post(); ?>
    <!-- some code here -->
<?php endwhile; else: ?>
    <p><?php _e('Sorry, no posts matched your criteria.'); ?></p>
<?php endif; ?>
```

This is the basic loop syntax, but because we’re using our custom query, we have to access the loop using our query variable that represents a new instance of `WP_Query`. From above, our query instance is noted by the variable  `$the_query`. Our loop, in this case, should look like this:

```language-php
<?php if ( $the_query->have_posts() ) : while ( $the_query->have_posts() ) : $the_query->the_post(); ?>
  <!-- some code here -->
<?php endwhile; else: ?>
  <p><?php _e('Sorry, no posts matched your criteria.'); ?></p>
<?php endif; ?>
```

The above set of code provides a certain set of functionality. First, it checks if any posts have been delivered as a result of our custom query. If this is true, and we have posts, we enter into a while loop. The while loop loops through our query, and then performs a set of code for each post returned. This is where we can output things like each post title etc. If no posts have been returned as a result of our query, we enter the else part of the block, and return an error statement notifying the user that no posts exist based on the query. Let’s enhance this loop a bit, and make it a bit more user friendly so it actually displays something if we have posts. Using the WordPress function `the_title()`, we’ll output the title of each post returned. Our loop now looks like this:

```language-php
<?php if ( $the_query->have_posts() ) : while ( $the_query->have_posts() ) : $the_query->the_post(); ?>
  <h1><?php echo the_title(); ?></h1>
<?php endwhile; else: ?>
  <h1>Sorry...</h1>
  <p><?php _e('Sorry, no posts matched your criteria.'); ?></p>
<?php endif; ?>
```

And voila! Our loop is now outputting posts based on our custom query. Now, time for some sweet functionality. We want to paginate our loop, which is especially handy when we have a lot of posts and want to display only a certain amount of posts per page. As we noted above, we’re only showing 5 posts per page based on our query. Let’s assume that our query is returning 20 posts (or any amount more than 5). We want the users to be able to go to older posts if they want. But how?

## Paginating The Custom Loop

Now that our loop is ready for pagination, let’s take a look at some core WordPress functions. If we look at the WordPress Function Reference, we’ll come across two functions called  `get_next_posts_link` and `get_previous_posts_link`. Note that there are also two functions  next_posts_link and  previous_posts_link, but according to the function reference, these do not work on static pages, hence making them void in our case. Let’s take a look at the two functions we need.

The `get_next_posts_link`  function, as described here in the function reference, gets a link to the previous set of posts within the current query. According to the reference:

> Because post queries are usually sorted in reverse chronological order, `get_next_posts_link()` usually points to older entries (toward the end of the set) and `get_previous_posts_link()` usually points to newer entries (toward the beginning of the set).

In simple terms, we can now display links very quickly and easily to older and newer sets of posts if they exist (i.e. if our query returned more posts than the value we set in the posts per page parameter). Let’s look at how we use the `get_next_posts_link`  function when we’re querying a loop with `WP_Query`. According to the documentation,

Add the `$max_pages` parameter to the `get_next_posts_link()` function when querying the loop with `WP_Query`. To get the total amount of pages you can use the `max_num_pages` property of the custom `WP_Query` object.

## The Full Query With Next & Previous Post Links

Now, our full custom query, with simple pagination links (i.e. links pointing to pages with older and newer posts) will look like this:

### Static Home Page

```language-php
<?php
  // set up or arguments for our custom query
  $paged = ( get_query_var('page') ) ? get_query_var('page') : 1;
  $query_args = array(
    'post_type' => 'post',
    'category_name' => 'tutorials',
    'posts_per_page' => 5,
    'paged' => $paged
  );
  // create a new instance of WP_Query
  $the_query = new WP_Query( $query_args );
?>

<?php if ( $the_query->have_posts() ) : while ( $the_query->have_posts() ) : $the_query->the_post(); // run the loop ?>
  <article>
    <h1><?php echo the_title(); ?></h1>
    <div class="excerpt">
      <?php the_excerpt(); ?>
    </div>
  </article>
<?php endwhile; ?>

<?php if ($the_query->max_num_pages > 1) { // check if the max number of pages is greater than 1  ?>
  <nav class="prev-next-posts">
    <div class="prev-posts-link">
      <?php echo get_next_posts_link( 'Older Entries', $the_query->max_num_pages ); // display older posts link ?>
    </div>
    <div class="next-posts-link">
      <?php echo get_previous_posts_link( 'Newer Entries' ); // display newer posts link ?>
    </div>
  </nav>
<?php } ?>

<?php else: ?>
  <article>
    <h1>Sorry...</h1>
    <p><?php _e('Sorry, no posts matched your criteria.'); ?></p>
  </article>
<?php endif; ?>
```

### Custom Static Page (that isn’t home page)

```language-php
<?php
  // set up or arguments for our custom query
  $paged = ( get_query_var('paged') ) ? get_query_var('paged') : 1;
  $query_args = array(
    'post_type' => 'post',
    'category_name' => 'tutorials',
    'posts_per_page' => 5,
    'paged' => $paged
  );
  // create a new instance of WP_Query
  $the_query = new WP_Query( $query_args );
?>

<?php if ( $the_query->have_posts() ) : while ( $the_query->have_posts() ) : $the_query->the_post(); // run the loop ?>
  <article>
    <h1><?php echo the_title(); ?></h1>
    <div class="excerpt">
      <?php the_excerpt(); ?>
    </div>
  </article>
<?php endwhile; ?>

<?php if ($the_query->max_num_pages > 1) { // check if the max number of pages is greater than 1  ?>
  <nav class="prev-next-posts">
    <div class="prev-posts-link">
      <?php echo get_next_posts_link( 'Older Entries', $the_query->max_num_pages ); // display older posts link ?>
    </div>
    <div class="next-posts-link">
      <?php echo get_previous_posts_link( 'Newer Entries' ); // display newer posts link ?>
    </div>
  </nav>
<?php } ?>

<?php else: ?>
  <article>
    <h1>Sorry...</h1>
    <p><?php _e('Sorry, no posts matched your criteria.'); ?></p>
  </article>
<?php endif; ?>
```

Our loop will now look like this on the first page our our posts:

<img class="aligncenter size-full wp-image-389" src="http://www.callmenick.com/wp-content/uploads/2014/02/layout-1.jpg" alt="layout-1" width="720" height="280" />

And it will look like this on pages that have older posts, and hence older/newer posts links:

<img class="aligncenter size-full wp-image-390" src="http://www.callmenick.com/wp-content/uploads/2014/02/layout-2.jpg" alt="layout-2" width="720" height="280" />

This is cool, and it works, but what if we wanted a more advanced pagination structure? What if we wanted to display the number of pages, links to the actual page numbers themselves, and links to the last and first pages if we have a lot of pages? For this, we need to write a slightly more complex function based on the same principals as above. We know some things already that will help us:

* We can check if pagination exists
* We can check the max number of pages or query returns based on the number of posts returned vs the number of posts per page being displayed

Let’s now look into setting up our function.

## Advanced Pagination Function

We want to create a function that accepts three values:

* The number of pages, i.e. the number of pages of posts returned by our query. This is accessed by the property  `$max_num_pages`.
* The page range. This is a range of pages that we will display for the user to be able to click on. We want it to be an even number
* The `$paged` value, which will be different for static home pages and other custom pages.

We also want our function to have fallback values, so that we can use this function in any custom query we set up, and also on any pages that have global queries in place (for example a category archive page). So far, our function should look like this:

```language-php
function custom_pagination($numpages = '', $pagerange = '', $paged='') {
 
  if (empty($pagerange)) {
    $pagerange = 2;
  }
 
  /**
   * This first part of our function is a fallback
   * for custom pagination inside a regular loop that
   * uses the global $paged and global $wp_query variables.
   * 
   * It's good because we can now override default pagination
   * in our theme, and use this function in default quries
   * and custom queries.
   */
  global $paged;
  if (empty($paged)) {
    $paged = 1;
  }
  if ($numpages == '') {
    global $wp_query;
    $numpages = $wp_query->max_num_pages;
    if(!$numpages) {
        $numpages = 1;
    }
  }
 
}
```

Now that everything is set up, we need to figure out how to output the pagination structure. Luckily, WordPress handles this awesomely for us with a core function  paginate_links. Let’s head over to the function reference for this function and see what it has to offer. Here’s a definition of the function straight from the documentation:

>Retrieve paginated link for archive post pages. Technically, the function can be used to create paginated link list for any area ( e.g.: « Prev 1 … 3 4 5 6 7 … 9 Next » )

If we read through, we notice that the function takes on an array of arguments. It’ll be a good idea to read through this and get familiar with what each argument means. For now though, let’s scroll down to the section “example with a custom query”. We’re going to use this as a sample, but edit a couple things. First of all, we’ve already established the number of pages (i.e. the max number of pages) by the variable `$numpages`. We also know our current page number set by the variable `$paged`. The ‘mid_size’ argument represents “how many numbers to either side of current page, but not including current page”. We will use our `$pagerange` variable here, which defaults to 2 if not passed through the function. Finally, we will set our previous and next buttons to double arrows, and output our pagination in the plain format. The two important arguments that we’re going to edit are:

* __base:__ we will not use the default suggestion from the reference, because on higher pages than 1, page 1 references back to "pageurl/page/1", and we want it to point back to the reference “pageurl”. We will use a more unusual WordPress function that isn’t documented in the function reference, called `get_pagenum_link()`. This function returns the url of a paged page when you pass in the page number. If the page number is 1, it defaults to the base page url. According to the definition too, “%\_%” in this argument will be replaced by the “format” argument per page. Our ‘base’ argument should then look like this:  `'base' => get_pagenum_link(1)` . '%\_%'
* __format:__ in my case, I’m using pretty permalinks, so I won’t follow the default format. I’ll use the format “page/%#%”, which will combine with the ‘base’ argument to output the url in this format:
    - if it’s page 1, it will output “pageurl”
    - any page higher than page 1 will give an output of “pageurl/page/pagenumber”

Our arguments should now look like this:

```language-php
/** 
 * We construct the pagination arguments to enter into our paginate_links
 * function. 
 */
$pagination_args = array(
  'base'            => get_pagenum_link(1) . '%_%',
  'format'          => 'page/%#%',
  'total'           => $numpages,
  'current'         => $paged,
  'show_all'        => False,
  'end_size'        => 1,
  'mid_size'        => $pagerange,
  'prev_next'       => True,
  'prev_text'       => __('&laquo;'),
  'next_text'       => __('&raquo;'),
  'type'            => 'plain',
  'add_args'        => false,
  'add_fragment'    => ''
);
```

Finally, let’s output everything. We pass our arguments into the function paginate_links, which will return a pagination structure if we have more than one page. We can then check if paginate_links  returns anything, and if so, run the output including some additional stuff like “page x of y”. We’ll add a few CSS classes, and also check the classes of our output, and add styles to our stylesheet accordingly. We should have something looking like this:

```language-php
$paginate_links = paginate_links($pagination_args);

if ($paginate_links) {
  echo "<nav class='custom-pagination'>";
    echo "<span class='page-numbers page-num'>Page " . $paged . " of " . $numpages . "</span> ";
    echo $paginate_links;
  echo "</nav>";
}
```

With some CSS, our pagination structure should now look a little like this:

<img class="aligncenter size-full wp-image-391" src="http://www.callmenick.com/wp-content/uploads/2014/02/layout-3.jpg" alt="layout-3" width="720" height="180" />

## The Full Code

Let’s take a look at the full code for custom paginate queries on a static home page, and a custom page template that isn’t the home page.

### Static Home Page

```language-php
<?php
/**
 * Template Name: Home Page
 * The home page template file
 */
?>

<?php get_header(); ?>

<h2>Posts</h2>

<?php 

  $paged = ( get_query_var('page') ) ? get_query_var('page') : 1;
  
  $query_args = array(
      'post_type' => 'post',
      'posts_per_page' => 2,
      'paged' => $paged,
      'page' => $paged
    );

  $the_query = new WP_Query( $query_args ); ?>
 
  <?php if ( $the_query->have_posts() ) : ?>
   
    <!-- the loop -->
    <?php while ( $the_query->have_posts() ) : $the_query->the_post(); ?>
      <article class="loop">
        <h3><?php the_title(); ?></h3>
        <div class="content">
          <?php the_excerpt(); ?>
        </div>
      </article>
    <?php endwhile; ?>
    <!-- end of the loop -->
 
    <!-- pagination here -->
    <?php
      if (function_exists(custom_pagination)) {
        custom_pagination($the_query->max_num_pages,"",$paged);
      }
    ?>
 
  <?php wp_reset_postdata(); ?>
 
  <?php else:  ?>
    <p><?php _e( 'Sorry, no posts matched your criteria.' ); ?></p>
  <?php endif; ?>

<?php get_footer(); ?>
```

### Custom Page That Isn’t Static Home Page

```language-php
<?php
/**
 * Template Name: Custom Page
 * The custom page template file
 */
?>

<?php get_header(); ?>

<h2>Posts</h2>

<?php 

  $paged = ( get_query_var('paged') ) ? get_query_var('paged') : 1;
  
  $custom_args = array(
      'post_type' => 'post',
      'posts_per_page' => 2,
      'paged' => $paged
    );

  $custom_query = new WP_Query( $custom_args ); ?>
 
  <?php if ( $custom_query->have_posts() ) : ?>
   
    <!-- the loop -->
    <?php while ( $custom_query->have_posts() ) : $custom_query->the_post(); ?>
      <article class="loop">
        <h3><?php the_title(); ?></h3>
        <div class="content">
          <?php the_excerpt(); ?>
        </div>
      </article>
    <?php endwhile; ?>
    <!-- end of the loop -->
 
    <!-- pagination here -->
    <?php
      if (function_exists(custom_pagination)) {
        custom_pagination($custom_query->max_num_pages,"",$paged);
      }
    ?>
 
  <?php wp_reset_postdata(); ?>
 
  <?php else:  ?>
    <p><?php _e( 'Sorry, no posts matched your criteria.' ); ?></p>
  <?php endif; ?>

<?php get_footer(); ?>
```

### Function.php File

```language-php
function custom_pagination($numpages = '', $pagerange = '', $paged='') {
 
  if (empty($pagerange)) {
    $pagerange = 2;
  }
 
  /**
   * This first part of our function is a fallback
   * for custom pagination inside a regular loop that
   * uses the global $paged and global $wp_query variables.
   * 
   * It's good because we can now override default pagination
   * in our theme, and use this function in default quries
   * and custom queries.
   */
  global $paged;
  if (empty($paged)) {
    $paged = 1;
  }
  if ($numpages == '') {
    global $wp_query;
    $numpages = $wp_query->max_num_pages;
    if(!$numpages) {
        $numpages = 1;
    }
  }
 
  /** 
   * We construct the pagination arguments to enter into our paginate_links
   * function. 
   */
  $pagination_args = array(
    'base'            => get_pagenum_link(1) . '%_%',
    'format'          => 'page/%#%',
    'total'           => $numpages,
    'current'         => $paged,
    'show_all'        => False,
    'end_size'        => 1,
    'mid_size'        => $pagerange,
    'prev_next'       => True,
    'prev_text'       => __('&laquo;'),
    'next_text'       => __('&raquo;'),
    'type'            => 'plain',
    'add_args'        => false,
    'add_fragment'    => ''
  );

  $paginate_links = paginate_links($pagination_args);
  
  if ($paginate_links) {
    echo "<nav class='custom-pagination'>";
      echo "<span class='page-numbers page-num'>Page " . $paged . " of " . $numpages . "</span> ";
      echo $paginate_links;
    echo "</nav>";
  }
 
}
```

### Style.css File

```language-css
/* ============================================================
  CUSTOM PAGINATION
============================================================ */
.custom-pagination span,
.custom-pagination a {
  display: inline-block;
  padding: 2px 10px;
}
.custom-pagination a {
  background-color: #ebebeb;
  color: #ff3c50;
}
.custom-pagination a:hover {
  background-color: #ff3c50;
  color: #fff;
}
.custom-pagination span.page-num {
  margin-right: 10px;
  padding: 0;
}
.custom-pagination span.dots {
  padding: 0;
  color: gainsboro;
}
.custom-pagination span.current {
  background-color: #ff3c50;
  color: #fff;
}
```

## Wrap Up

Phew! That all seemed like a lot of work, but I wanted to get through the core functionality so you would understand exactly what’s going on. The WordPress function reference bailed us out on a lot of work this time, and made it easier than it could’ve been. Now you have it, and you can apply it across your site. I hope you enjoyed this tutorial! Feel free to leave any comments, feedback, or questions below, and you can download the source files created for this demo below also.