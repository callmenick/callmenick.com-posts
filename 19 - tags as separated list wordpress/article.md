To display tags as a separated list in WordPress is an easy thing to do, and can be done with one simple function. In our case though, we’re going to add a little step up to the function. We’re going to check if the post actually has tags, and if it does, then display it. We’ll use the functions `get_the_tags`.

The first function will let us know if we have any tags, and the second will actually output the tags. In our case, we’ll output them as a comma separated list. WordPress has a function `get_the_tags_list` that will neatly output the tags, separate them with a delimiter, and also link to the pages. The reason for doing it this way though is to have more control over what is outputted, and if you want to put links or not. Note, this post has to be used inside the loop. Here’s a look at the function, and then the code.

```language-php
function list_tags() {
	$tags = get_the_tags();
	if ($tags) {
		$tag_array = array();
		foreach($tags as $tag) {
            $tag_id = $tag->term_id;
			$tag_name = "<a href=\"".get_tag_link($tag_id)."\">".$tag->name."</a>";
			array_push($tag_array, $tag_name);
		}
    	$tags_list = implode(", ", $tag_array);
        return "Tags: " . $tags_list;
	} else {
		return false;
	}
}
```

```language-php
<?php if ( have_posts() ) : while ( have_posts() ) : the_post(); ?>
    Post Date: 12/12/12
    <span>//</span>
    <?php if (list_tags()) { ?>
            <span>//</span>
            <?php echo list_tags(); ?>
    <?php } ?>
<?php endwhile; endif; ?>
```

## Notes

The function first checks for tags. If tags exist, we get them all and put them into an array. The object comes with a list of properties, from which we can get the tag ID, name, and link. We then implode this array, and separate it with commas and links to each tag archive. If no tags are returned, the function returns false.

Inside the loop, we can check if our function `list_tags()`  returns anything. If it does, we add our “//” meta separator and output the tags. We have the added benefit of not outputting our meta separator if no tags are returned, making sure our meta doesn’t look awkward. The beauty of this function lies in the fact that it’s highly customisable. You can change the comma separation to an unordered list, you can unlink the tag archive if you wanted. The point is we have access to each of the tags’ properties, giving us some fine tuned control on what we output and how.

I hope you enjoyed this tutorial, and feel free to leave any feedback below!