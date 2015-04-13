The PHP if else statement is one of the most important building blocks in the PHP language. It will give us the blocks where we can display or perform certain operations depending on if a condition holds true or not. Below, we go through three different PHP if else syntax structures (common, alternative, and inline shorthand), and examples for each.

## 1) Common If Else Syntax and Examples

This version of the PHP if else statement is probably the first you’ve come across. It uses brackets to denote the condition, and curly braces to denote the conditional  blocks. Here are some examples:

```language-php
/* outputs "we have broccoli!" */
$veggie = "broccoli";
if ( $veggie == "broccoli" ) {
	echo "we have broccoli!";
} else {
	echo "no more broccoli";
}

/* outputs "we have veggies" */
$veggies = true;
if ( $veggies ) {
	echo "we have veggies";
} else {
	echo "no veggies";
}

/* outputs "we have something else" */
$veggies = "snow peas";
if ( $veggies == "broccoli" ) {
	echo "we have broccoli";
} elseif ( $veggies == "carrot" ) {
	echo "we have carrots";
} else {
	echo "we have something else";
}
```

## 2) Alternative If Else Syntax and Examples

The alternative PHP if else syntax is very similar to the traditional one, except it’s written a bit differently. This syntax is generally great when you have to output a lot of HTML in the conditional blocks. Here’s the structure, and some examples:

```language-markup
<!-- outputs the if block -->
<?php $num_posts = 10; ?>
<?php if ( $num_posts > 0 ) : ?>
	<section>
		<h1>Recent Posts</h1>
		<p>Here's a display of all our recent posts.</p>
	</section>
<?php else : ?>
	<section>
		<h1>No Posts</h1>
		<p>Sorry, but we found no posts.</p>
	</section>
<?php endif; ?>
```

## 3) Inline Shorthand If Else Syntax

The inline shorthand version of the if else statement is pretty handy, but takes a little longer to master than the common and alternative syntaxes. It uses ternary logic to return the correct set of code based on the condition. There are some advantages of using ternary logic in your code -

* You can write simple if else statements quickly, compactly, and in one line
* You can maintain and edit the code easier

However, it’s important to not nest too deeply and confuse yourself (or other co-workers). My take on it is that this syntax style is best for single conditions. Here are some examples:

```language-php
/* syntax */
$VAR = ( CONDITION ? OUTPUT IF CONDITION TRUE : OUTPUT IF CONDITION FALSE );

/**
 * basic true/false usage, sets $likes_broccoli to TRUE if condition met, 
 * else sets it to false
 */
$likes_broccoli = ( $user['likes_broccoli'] == "yes" ? true : false );

/**
 * inline echo statement usage, outputs "I love broccoli!" if condition met,
 * or "I hate boccoli!" if codition not met.
 */
echo "I " . ( $user['likes_broccoli'] == "yes" ? "love" : "hate" ) . " broccoli!";
```

## Wrap Up

Each of these usages has its benefits, and it’s up to you to decide which syntax suits your needs. The if else statements such important building blocks in any project, so master them, and make them your friend. I hope you enjoyed and learned!