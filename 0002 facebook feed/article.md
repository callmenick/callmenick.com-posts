<p class="text-align--center">
<a href="http://callmenick.com/_development/facebook-feed/facebook-feed-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/facebook-feed/" class="button button--inline-block button--medium">View Demo</a>
</p>

**Disclaimer: This tutorial was published on March 14th 2013, using version 3.0.0 of the SDK (available in the source download). Since then, SDK’s have changed a bit, and some set ups. For the most part, it should work. If you’re getting any errors, please read through the comments below and leave yours. I’ll try to help as much as possible.**

A Facebook page feed can be easily displayed using Facebook’s social plugins, but the default display is very blue and Facebook, and not the best looking. It’s also not customizable at all, leaving you with little options. In this tutorial, we take a look at using the Facebook PHP SDK to connect to a Facebook app, retrieve the graph data from a Facebook page, and output it in a neat, nice looking format.

## Getting Started

In simple terms, when you want to get the down and dirty with Facebook interaction on your website, you need to create a Facebook app. This app then interacts with your website and Facebook’s various "graphs", and you can then use this graph info or Facebook functionality to add stuff to your website. Think of things like log in/log out, comment feeds, etc. Facebook has various SDK’s to help out with these operations. We’re going to use the PHP SDK, which helps you add authentication and graph API support to your desktop web or mobile web app. For reference, SDK stands for “Software Development Kit”, so basically a library. Here’s our first set of steps:

1. Go to this url: [developers.facebook.com/](developers.facebook.com/)
2. Log in or create a developers account
3. Go to "apps" in the navigation bar at the top
4. Create a new app. Call it anything you want. For the sake of this tutorial, I called mine "Call Me Nick Page Feed"
5. Take note of your App ID and App Secret. You can always refer back to your app page in your developer account to get them
6. Now, let’s download the PHP SDK. [You can download it here from the official Facebook Git repo](https://github.com/facebook/facebook-php-sdk)
7. Put the SDK into a folder inside your site root, and remember the path to it

## Installing & Initialising the SDK

This is widely documented, but I’ll go through it here anyways. Here’s a copy and paste from the Facebook Site:

>To install the SDK, extract the downloaded files and copy the files from the src/ directory to a directory on the server where you will host your app, for example php-sdk. Then, just include php-sdk/facebook.php wherever you want to use the SDK.

You use the SDK by instantiating a new Facebook object with, at a minimum, your app id and app secret

```php
// include the facebook sdk
require_once('src/facebook.php');
 
// connect to app
$config = array();
$config['appId'] = 'APP_ID';
$config['secret'] = 'APP_SECRET';
$config['fileUpload'] = false; // optional
 
// instantiate
$facebook = new Facebook($config);
```

Replace `APP_ID` and `APP_SECRET` with your respective values that you obtained when you created your app, and we’re ready to start moving on.

## How Does It Work?

I won’t go in to too much detail, but it’s worth mentioning how this all works. Basically, Facebook generates "graphs" of information that store EVERYTHING. In other words, the Graph of a facebook page will store an array of posts, and in this array it has things like the post content, links, number of likes, comments, who liked it, photos associated with it, etc etc. Using the Facebook SDK allows us to access these Graphs and manipulate the data.

The Facebook SDK initialisation generates an access token of sorts, and uses it. However, in the case of apps accessing user information, permission has to be granted. No information can be gathered from a user unless the user approves it.

So in a nutshell, you create a Facebook application (like we did in part 1), you connect the app with the SDK (part 2), and you start pulling in the data from the graphs. In order to pull in data, an ID or Access Token must be supplied. This can only be supplied by the user if they agree to allow Facebook to access the information. After that, the Access Token of the user/page/whatever is used to gather info. This is how apps like Instagram work, where you log in with Facebook on the app, and can publish to your wall and see what friends are also using the app.

## Displaying The Page Feed

Moving on, we need the page id for our Facebook page. We can grab this by simply typing this in to the url bar in your browser:  `graph.facebook.com/xxxxx`.

This is simply your Facebook page url with “graph.” in front of it. I’m going to grab the feed for my facebook page, [Nick Salloum – Web Design & Development](https://www.facebook.com/callmenick1), and use that in this example. This is what I entered into my broswer:  `graph.facebook.com/callmenick1`.

Nothing here is sensitive information, so don’t worry. It’s just an array of info that is available on your page. Find where it says `id`, and grab that number. Another thing that is good to keep handy is the page’s access token. The final thing you might need, and I use it because it makes the graph’s easier to read, is an application access token. To get this, enter this in your browser in ONE LINE WITH NO SPACES. Replace `APP_ID` with your application id, and App Secret with your application secret:

```
https://graph.facebook.com/oauth/access_token?client_id=APP_ID&client_secret=APP_SECRET&grant_type=client_credentials
```

This is what we need to proceed.

Now, let’s go to the API reference on Facebook and then take a look at the Graph API documentation. This gives us a good idea of some things we can access, and how we can access them. In the case of using a feed from a page/profile, scroll down and check out "feed". If you click on it and use the graph explorer, it’ll say something like you need an access token. That’s exactly what we’ve done using our SDK! If you browse the examples, you will see that you can access a graph by doing the following:

```php
$facebook->api('somthing here');
```

Great! Now we can see that we need to have something of the form:

```php
$facebook->api('/PROFILE_ID/feed');
```

So far, our PHP code should look like this:

```php
// include the facebook sdk
require_once('resources/facebook-php-sdk-master/src/facebook.php');
 
// connect to app
$config = array();
$config['appId'] = 'YOUR_APP_ID';
$config['secret'] = 'YOUR_APP_SECRET';
$config['fileUpload'] = false; // optional
 
// instantiate
$facebook = new Facebook($config);
 
// set page id
$pageid = "YOUR_PAGE_ID";
 
// now we can access various parts of the graph, starting with the feed
$pagefeed = $facebook->api("/" . $pageid . "/feed");
```

If we try to do `echo $pagefeed;`, we will notice that it outputs "Array". This is good news, because it means now that we’ve accessed our page feed, and have it stored in an array. To see what the contents of this array are, do this:

```php
echo "<pre>";
print_r($pagefeed);
echo "</pre>";
```

This might take a while to load, because it is now displaying ALL feed interactions on your page. That is, when you posted a status, a picture, an event, a note, if someone liked it, if they commented on it, if someone wrote on your page’s wall, etc. Don’t worry, we’re going to filter through this mega array and make it look pretty. Remember though, if you took note of the access token from before, you can also access this super array (graph) right in your browser by entering this into the url bar:

```
https://graph.facebook.com/YOUR_PAGE_ID/feed?access_token=APP_ACCESS_TOKEN
```

Browsing through the mega array, you might notice that every interaction is stored in it’s own sub-array, governed by the boss array “data”. You’ll also notice that every sub-array has the parameter “type”. This is crucial, because we might want to display links differently to statuses, and photos differently to those. Let’s make a little progress here, and break down our array in a nice `foreach` loop. Also, we’ll limit this to 10 posts so that our page isn’t mega long. Let’s add this to our code:

```php
// set counter to 0, because we only want to display 10 posts
$i = 0;
foreach($pagefeed['data'] as $post) {
 
    echo $post['type'] . "<br />";
 
    $i++; // add 1 to the counter
 
    //  break out of the loop if counter has reached 10
    if ($i == 10) {
        break;
    }
}
```

This now shows us the type of post that the last 10 posts on our page were. Now is where it gets fun! We need to do a bunch of conditional looping to make our page feed display the way we want. If you notice, each of these ‘types’ have different parameters associated with them. Example: `type => photo` has a story and a photo url associated with it, while `type => status` has a message. Let’s bust into some conditions here. So far, on my page, I only have photos, links, and statuses, so I’m going to wrap everything in a condition:

```php
if ($post['type'] == 'status' || $post['type'] == 'link' || $post['type'] == 'photo') {
 
    // do something
    $i++; // add 1 to the counter if our condition for $post['type'] is met
 
}
```

We moved our `$i` counter inside the if statement, so that we only add 1 to `$i` if our condition is met. This ensures that we always get 10 posts. Now, with some more conditional looping, observation of our array, and some simple html/css markup, our code may look something like this:

```php
echo "<div class=\"fb-feed\">";
 
// set counter to 0, because we only want to display 10 posts
$i = 0;
foreach($pagefeed['data'] as $post) {
 
    if ($post['type'] == 'status' || $post['type'] == 'link' || $post['type'] == 'photo') {
 
        // open up an fb-update div
        echo "<div class=\"fb-update\">";
 
            // post the time
 
            // check if post type is a status
            if ($post['type'] == 'status') {
                echo "<h2>Status updated on: " . date("jS M, Y", (strtotime($post['created_time']))) . "</h2>";
                echo "<p>" . $post['message'] . "</p>";
            }
 
            // check if post type is a link
            if ($post['type'] == 'link') {
                echo "<h2>Link posted on: " . date("jS M, Y", (strtotime($post['created_time']))) . "</h2>";
                echo "<p>" . $post['name'] . "</p>";
                echo "<p><a href=\"" . $post['link'] . "\" target=\"_blank\">" . $post['link'] . "</a></p>";
            }
 
            // check if post type is a photo
            if ($post['type'] == 'photo') {
                echo "<h2>Photo posted on: " . date("jS M, Y", (strtotime($post['created_time']))) . "</h2>";
                if (empty($post['story']) === false) {
                    echo "<p>" . $post['story'] . "</p>";
                } elseif (empty($post['message']) === false) {
                    echo "<p>" . $post['message'] . "</p>";
                }
                echo "<p><a href=\"" . $post['link'] . "\" target=\"_blank\">View photo &rarr;</a></p>";
            }
 
        echo "</div>"; // close fb-update div
 
        $i++; // add 1 to the counter if our condition for $post['type'] is met
    }
 
    //  break out of the loop if counter has reached 10
    if ($i == 10) {
        break;
    }
} // end the foreach statement
 
echo "</div>";
```

With some simple css styling, you can get it to look really nice and pretty! Thanks for reading, and feel free to download the source. If you have any questions or suggestions, leave a comment below and I’ll get back to you as soon as possible.

<p class="text-align--center">
<a href="http://callmenick.com/_development/facebook-feed/facebook-feed-source.zip" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/facebook-feed/" class="button button--inline-block button--medium">View Demo</a>
</p>