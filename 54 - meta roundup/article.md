## An Introduction to Meta Tags

Meta tags are some of the first tags that browsers see when looking at our site. They provide a range of functionalities, from defining character sets and descriptions, to the more recent open graph data tags for social media. This set of snippets digs into some of the more useful tags out there, and a description on how, when, and why to implement them.

## Meta - Charset

The `charset` meta tag is highly useful and important as it declares the character encoding of the page. In older versions of HTML, the `charset` was defined in a more lengthy tag that included the `http-equiv` and `content` declarations, but those are obsolete in HTML5 documents. For the absolute majority of cases, you'll include this meta tag like this:

```html
<meta charset="UTF-8">
```

This meta tag must be inside the `head` tag of your HTML document, and within the first 512 bytes of the page. Get it in there as soon as possible!

## Meta - Name

The `name` meta tag presents us with a few options for getting valuable information into our document. These bits of information can tell the browser what our page is about, who the author is, and other bits of info. Let's take a look at some important ones:

### Description

The `description` declaration describes what your document is about. It's useful for crawlers to get an inside scoop on the contents of your document, and it acts as the little snippet that shows up under the title in a search engine's results. Here's what it looks like:

```html
<meta name="description" content="Description of your page here...">
```

### Author

The `author` declaration let's search engines and users know who the author of the document is. Here's what it may look like:

```html
<meta name="author" content="Nick Salloum">
```

### Robots

The `robots` declaration defines the behaviour that crawlers should have when indexing your page. It is up to the search engine to respect this, however. Here's an example that will instruct crawlers to not index your page (should they choose to follow it):

```html
<meta name="robots" content="noindex">
```

### Viewport

The `viewport` declaration is probably one of the more important tags you'll be using today, especially considering that we are in the age of responsive design. We have a host of options to choose from when using this declaration that allow us to set widths, heights, and scaling abilities. The options are: `width`, `height`, `initial-scale`, `maximum-scale`, `minimum-scale`, and `user-scalable`. For the `width` and `height` options and variants, we can specify either a positive integer in pixels, or the `device-width` or `device-height`. For the `scale` variants, we can specify a positive number between 0.0 and 10.0. The `user-scalable` option takes on a boolean value of "yes" or "no".

This use of the `viewport` declaration will force the page to start at a width of 320px and height of 480px:

```html
<meta name="viewport" content="width=320,height=480">
```

This example will force the browser window to fit the device width, and disable user-scaling:

```html
<meta name="viewport" content="width=device-width,user-scalable=no">
```

And for the most part, if you're building and producing responsive web projects (which you should be!), you'll want to use this declaration:

```html
<meta name="viewport" content="width=device-width,initial-scale=1.0">
```

This sets the initial view to a 1.0 scale and the width at the user's device width, which should be the desired starting point if your website/app is fully responsive. It also allows users to scale up and down at will, which is recommended for accessibility/usability purposes.

## Meta - http-equiv

You'll most likely use this declaration if you want to implement a page refresh after a certain amount of time. Some of its parameters are obsolete, so I'd stay clear from them. Here's how we'd use this declaration to implement a page refresh every 10 seconds:

```html
<meta http-equiv="refresh" content="10">
```

If we wanted to re-direct the page after 10 seconds, we'd declare it like this:

```html
<meta http-equiv="refresh" content="10;url=http://redirect-url.com">
```

## Open Graph Meta Tags

You won't find the collection of open graph (OG) meta tags and their meanings on any of the spec sites for HTML. However, the OG tags serve a highly functional purpose when i comes to social media linking and sharing. These tags tell the various social media channels what data to pull in. This data includes the title, description, image, fallback image, and author of a page. If you're blogging and pushing your articles to social media channels, it's highly recommended to implement these tags. We define an OG tag like this:

```html
<meta property="og:_____" content="...">
```

Here are the basic options that are recommended to implement in your page:

```html
<meta property="og:title" content="The Page Title">
<meta property="og:site_name" content="Site Name">
<meta property="og:url" content="http://pageurl.com">
<meta property="og:image" content="http://pageurl.com/images/fallback.jpg">
<meta property="og:image" content="http://pageurl.com/images/page-image.jpg">
```

The open graph meta tags go into a great deal of detail, and you can find more information on the [Open Graph Protocol](http://ogp.me/) site. If you're blogging on a platform like WordPress, it's highly likely that there are plugin solutions to take care of the implementation of OG tags and data automatically.

## Wrap Up

It's very apparent that meta tags serve a big and important purpose in our projects. It's good to have a good grasp of them and know which ones to include where. These are the meta tags that I tend to find very important and useful. For more information on meta tags, check out the [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta). I hope you enjoyed this collection of snippets and found it useful! Thanks for reading, and feel free to leave any questions, comments, or feedback below.