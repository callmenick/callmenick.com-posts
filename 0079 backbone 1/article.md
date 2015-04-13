<p class="text-align--center">
<a href="https://github.com/callmenick/Backbone-Users-Directory/tree/part-1" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/backbonejs/users-directory/" class="button button--inline-block button--medium">View Demo</a>
</p>

## Foreword

This is part 1 of 4 of an ongoing experiment, where I document my steps building with Backbone JS – a client side framework that I’m beginning to love. Each part will build upon the previous, and will ultimately evolve into a little sample users-directory application.

1. Part 1 – displaying users
2. Part 2 – sorting users (coming soon)
3. Part 3 – single view of a user (coming soon)
4. Part 4 – adding/deleting/editing users (coming soon)

To keep up to date with future progress in this series, [sign up to the newsletter](http://callmenick.com/subscribe).

## What Is Backbone JS?

Backbone is among the top list of front-end structural libraries, allowing us to build web applications easily, without losing focus on mundane tasks like DOM querying, data binding, etc. Traditionally, when we build an application using JavaScript, we find ourselves entering callback-hell. Functionality is piled on top of functionality to allow us to alter content, run an AJAX call, change what’s seen on the screen, etc. Backbone JS is a framework that takes care of a lot of these redundant and hard-to-maintain tasks for us, letting us focus on what’s important – the end result.

With Backbone, data is represented as models. Models can be created, destroyed, validated, or saved to any server or local storage set up. Models are linked to “views”, meaning that the data is automatically bound to the view. Whenever a model changes (via user interaction, for example), the views are automatically updated. Backbone depends on only one other library – Underscore – a library providing us with a host of utility functions. Backbone is a minimal 6.5kb zipped, and underscore is even lower at 5.2kb, giving us a total minuscule overhead of 11.7kb. Not bad at all!

In this series of tutorials, I’m going to go through some iterations of building a simple users directory. I’ll use Backbone and Underscore as required, and also leverage Backbone.Native to handle some of Backbone’s functionality with native JavaScript that otherwise would require jQuery. You can read more about that here. At the end of the series, we’ll want to achieve the following:

1. Part 1 – display a table view of all the users (first name, last name, and email address), read from JSON data
2. Part 2 – the ability to sort the table by either of the three fields
3. Part 3 – a single view of each user
4. Part 4 – the ability to add, edit, and remove users from the table.

Alright, let’s get started!

## Getting Started 

In a real world scenario, you’ll probably be pulling down some users from a database somewhere. But for the sake of staying on the point of Backbone itself, we’re not going to do that. We’ll store our users in a simple JSON file, and reference that in our directory. Speaking of directories, let’s set up ours like this:

```
- index.html
assets/
    - users.json
css/
    - main.css
js/
    - app.js
    lib/
        - backbone.js
        - backbone.native.js
        - underscore.js
```

Let’s initially breakdown each of our files.

## The Index & CSS Files

Our index file is standard to start. We’ll doctype it, reference the CSS and JS files, and set up an empty table that will ultimately house the users directory. Here’s a look at the empty shell:

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Users Directory</title>
<meta name="description" content="A little experiment on generating a users directory with BackboneJS and JSON data.">
<link rel="stylesheet" href="css/main.css">
</head>
<body>

...

<table id="users">
  <thead>
    <tr>
      <th>First Name</th>
      <th>Last Name</th>
      <th>Email Address</th>
    </tr>
  </thead>
  <tbody id="users__body"></tbody>
</table>

...

<script src="js/lib/underscore.js"></script>
<script src="js/lib/backbone.js"></script>
<script src="js/lib/backbone.native.js"></script>
<script src="js/app.js"></script>

</body>
</html>
```

This is where all our content will reside, and where the content generated from the views will be output. This is also where we’ll reference our files. Take note of the id on the tbody element. This will come in handy for reference later when we want to output our view of all the users, and append it to that element.

Of trivial importance to this tutorial is the CSS file. Everything will work as is, but naturally we want something that’s somewhat visually pleasing to look at. The CSS file has some basic styles to get us started. I won’t output them here, but you can take a look at them in the source on GitHub. Let’s take a look now at our JSON file.

## The User Data JSON File

As I mentioned before, in a real world scenario, you’d probably be pulling this data from a database of some sort. However, we’re just going to reference to file statically (for now), to get into the swing of things as soon as possible. Head over to [generatedata.com](http://www.generatedata.com/), and chose three columns:

* firstname
* lastname
* email

Generate the data for 100 rows, and save it to your assets directory as a JSON file. The data should look something this:

```javascript
[
  {
    "firstname": "Eric",
    "lastname": "Whitley",
    "email": "eleifend.non@sedorci.org"
  },
  {
    "firstname": "Calista",
    "lastname": "Fuentes",
    "email": "nonummy@Donecluctusaliquet.ca"
  }
  ...
]
```

Now, we’re ready to rock and roll with our app. From here on, we’ll be working inside the `app.js` file.

## The Application Scripts

Inside the `app.js` file, we’ll be running all of our code to make the application tick. As you’d have noticed in the `index.html` template, the `app.js` file is called after all other dependancies. We’ll run our code through a self-executing function, so barebones, our file should look like this:

```javascript
(function() {
  // some code here
})();
```

Alright, let’s begin coding!

## Building Up The App – The Model

To start with, we’re going to establish a new model called the `UsersModel`. This will act as a model for our output. According to the Backbone docs:

> Models are the heart of any JavaScript application, containing the interactive data as well as a large part of the logic surrounding it: conversions, validations, computed properties, and access control.

Models come with a host of associated functions (see the [docs](http://backbonejs.org/#Model)), but the only ones we’ll use for this iteration of our app will be the `extend` and `defaults` functions. The `extend` function allows us to create a model class of our own, and the `defaults` function allows us to specify the default attributes for our model. Our model should look like this:

```javascript
var UsersModel = Backbone.Model.extend({
  defaults: function() {
    return {
      firstname: "",
      lastname: "",
      email: ""
    }
  }
});
```

Notice how we’ve set up our attributes with parameters we’d expect to be able to retrieve in the future. Let’s quickly output a new instance of our `UsersModel` to the console to see what it returns:

```javascript
console.log(new UsersModel());
```

As you can see, it returns the `child` object, from which the attributes are available. Backbone offers us the `get` function to retrieve attributes from an instance of a model. Trying it at this point should yield an empty string (because we set the defaults to be empty strings), but let’s try anyways.

```javascript
console.log((new UsersModel()).get("firstname"));
// should return an empty string
```

Now that we understand how the model is working, and how to retrieve data from it, let’s establish a collection!

## Building Up The App – The Collection

So, what exactly are we talking about when we say “collection”? Let’s go straight to the Backbone [docs](http://backbonejs.org/#Collection). According to them:

>Collections are ordered sets of models. You can … fetch the collection from the server …

I’ve highlighted the two phrases of text that will be important to us for this iteration of our app. We’re going to take three parameters into consideration here when we’re generating our collection:

* `model` – specifies the model class that the collection contains
* `comparator` – used to maintain the collection in sorted order
* `url` – set the url on a collection to reference its location on the server

As you’d expect, the `model` in question would be our `UsersModel`, and the `url` would be the link to our JSON file. For starters, let’s set the `comparator` to `firstname`, meaning that our models will be sorted by first name. Here’s what it looks like:

```javascript
var UsersCollection = Backbone.Collection.extend({
  model: UsersModel,
  comparator: "firstname",
  url: "assets/users.json"
});
```

Now, if we output our collection to the console, we’ll notice that no models have been added yet. This is because we’re getting the data from a url, and need to wait for it to be successfully fetched. Backbone conveniently provides the `fetch` function, which will let us know when the data has been successfully retrieved. Now, we can expect to see the models added to our collection. Let’s test it out and see:

```javascript
var testCollection = new UsersCollection();

testCollection.fetch({
  success: function() {
    console.log(testCollection);
  }
});
```

Nice! We’re getting somewhere, but we’re actually going to save that `fetch` functionality when it comes to rendering our view. So let’s dig right into that.

## Building Up The App – The View

[Views](http://backbonejs.org/#View) have a pretty self-explanatory name. They are responsible for the view, or presentation of output to the user. They aren’t, however, responsible for manipulating and changing the data. In fact, they are meant to be coupled to models, so that when the model changes, the view automatically updates. This is the beauty of Backbone, as we now never have to look up elements and change HTML manually, keeping track of a million things at once.

For now, since we’re just outputting our data to an already existing DOM element, we’re only interested in the following:

1. `el` – the element, referenced by tag name, id, or class, in which we’ll display our view.
2. `initialize` – a function that will be called when the view is first created.
3. `render` – a function that will render the view template from model data.

Other than that, I’ll use one custom variable and one custom function for helping out:

* `collection` – a new instance of our collection.
* `output` – a function that gets called inside of `render` on each iteration over our collection.

So to break it down, we’ll want to display our view inside the `#users__body` element, grab a new instance of our collection, initialize the view, check to see if the collection is ready using the `fetch` function described before, and then render the view. Here’s the code:

```javascript
var UsersView = Backbone.View.extend({
  el: "#users__body",

  collection: new UsersCollection(),

  initialize: function() {
    var scope = this;
    this.collection.fetch({
      success: function() {
        scope.render();
      }
    });
  },

  render: function() {
    var scope = this;
    this.collection.forEach(function(model) {
      scope.output(model);
    });
    return this;
  },

  output: function(model) {
    var row = document.createElement("tr");
    row.innerHTML = "<td>" + model.get("firstname") + "</td>\
                     <td>" + model.get("lastname") + "</td>\
                     <td>" + model.get("email") + "</td>";
    this.el.appendChild(row);
  }
});
```

There are a few important things to note here, so that you’re not overwhelmed. The first is that when we loop over a collection using underscore’s `forEach` function as we have in the `render` function, the model is the only parameter that gets passed in. So we now have access to each model inside the loop, allowing us to display the data. Next is that in our `output` function, we retrieved model data for each model using the `get` function, as I mentioned above. Finally, we’re appending the generated HTML to the correct element by referencing it as `this.el`. And there you have it, our view has been generated!

## What Next?

So far, we’ve learned about Backbone, what its strong points are, and how to leverage it to build a web application. We’ve looked into retrieving data from a URL, though in a real life situation, you’d probably want to fetch the initial data from the server first. Next up, we look at sorting the data, followed by single views, adding, editing, and removing.

## Wrap Up

And that’s a wrap! Don’t forget, you can view the demo and download the source by clicking the links below, and if you have and questions, comments, or feedback, you can also leave them below.


<p class="text-align--center">
<a href="https://github.com/callmenick/Backbone-Users-Directory/tree/part-1" class="button button--inline-block button--medium">Get Source</a>
<a href="http://callmenick.com/_development/backbonejs/users-directory/" class="button button--inline-block button--medium">View Demo</a>
</p>