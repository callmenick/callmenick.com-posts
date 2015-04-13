## Why Is This Needed?

If you have multiple categories on your WordPress site, and each of them perform different functions, it’s likely that you might want to display them differently. For example, consider this category hierarchy:

* Recipes
    - Breakfast
    - Lunch
    - Dinner
* News

There’s a chance here that you’d want all posts filed under recipes, breakfast, lunch, and dinner, to have the same loop structure, and all posts filed under news to have a different loop structure. The following snippet will tap into the WordPress default query, and allot up do display a custom loop for our WordPress category including children.

## The Code

```language-php
$cat_id_current = $wp_query->queried_object->term_id;		// id of currently viewed category
$cat_id_parent = $wp_query->queried_object->parent;			// parent id of currently viewed category
$cat_id_SLUG = get_category_by_slug( 'SLUG' )->term_id;		// id of category we want to display differently

/**
 * Check first if the current category ID OR the parent category ID
 * is equal to the ID of the category we want to display differently.
 *
 * If this condition is true, follow through with this part of the loop
 */
if ( ( $cat_id_current == $cat_id_designs ) || ( $cat_id_parent == $cat_id_designs ) ) :

	// run the custom loop
	if ( have_posts() ) :
		while ( have_posts() ) : the_post();	
		endwhile;
	else:
	endif;

/**
 * If the above condition is false, display a default loop
 */
else:

	// run the default loop
	if ( have_posts() ) :
		while ( have_posts() ) : the_post();	
		endwhile;
	else:
	endif;

endif;
```

## Wrap Up

That was simple enough! Just a little conditional testing, checking the ID’s of the current category, the parent category, and the desired category that we want to display differently. Now, we’re able to display different looking loops depending on our category hierarchy, and add a little variety to our site.