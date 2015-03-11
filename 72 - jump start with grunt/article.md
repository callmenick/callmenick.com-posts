We're hearing a lot about [GruntJS](http://gruntjs.com/) these days, and rightfully so. All the cool kids are using it, and I've taken some time recently to just plunge into it and implement it in my projects. I haven't looked back since. From the outskirts, it might seem like a daunting set up, and one that you can do without. But in 2014, I'll highly advise to lose that mentality, and wrap your head around this awesome tool. Not only will it make your life easy, but it'll boost the performance of your apps. Before we get into the inner workings of Grunt, let's bring forward some info about what exactly Grunt is.

## What Is Grunt?

In just a few short and sweet words, Grunt is a JavaScript task runner. It's a NodeJS application, and runs through the command line with the simplest of all commands:

```bash
grunt
```

I jumped ahead of myself a bit here, but with the proper set up and basic knowledge, it's really that simple. To run Grunt, you'll need to have the following in your project directory:

* A `Gruntfile.js` file, which describes the tasks Grunt should perform when run
* A `package.json` file that allows us to reference values from it inside our Grunt file
* A `node_modules` directory, which houses all the Grunt task plugins

To make it easier, just keep in mind that Grunt is simply a command line tool. When the `grunt` command is run from the command line inside your directory, it reads through the `Gruntfile.js` file, and loads and runs the necessary tasks that are set up in the Grunt file.

## Why You Should Use Grunt

You might be thinking already that this all seems like a lot of bother for something that you already have figured out by use of various apps. When I first heard about Grunt, I was a bit apprehensive to get into it, but only because it seemed hard to wrap my head around. It's an age of information, and it's really hard to swallow everything at once. But once I sat down and got my mind into it, it was a matter of minutes before I was running my first Grunt tasks. And once I passed that point, there was no looking back.

Let's demonstrate it a bit more, instead of just heaping all this praise upon it. Inside your project folder, you'll probably have a bunch of files in their development format. Some examples that immediately come to mind are:

* CSS file (or files)
* JS file (or files)
* Images

Maybe before you go into production, you run your necessary assets through your favourite apps, concatenators, minifiers, prefixers, optimisers, etc. And that's great, you should be doing that. But then in a week, you need some changes, and you have to run everything again. It adds up. Enter Grunt, an automated task runner.

Now I'm not saying there's anything against these apps that your using. In fact, they're probably really good. But guess what? They aren't automated. And guess what else? A lot of these top apps can also be set up as Grunt tasks. Do you imagine a development environment where your task-runner is actually watching for changes and automating the processes of your choice? Grunt gives you all of that. Let's look into how we would set up a grunt project, include a couple plugins, and automate the whole thing.

## Setting Up A Grunt Project

Before you can use Grunt, remember that it runs on Node. So if you don't have Node installed on your machine, [do that](http://nodejs.org/). Also, to effectively use Grunt through the command line, you should have [Node Package Manager](https://www.npmjs.org/) up and running too. Don't worry, you don't actually need to know Node to use Grunt. 

Now that you're set up, you'll want to make Grunt available in the CLI globally. Do that by entering this command:

```bash
npm install -g grunt-cli
```

I mentioned before also that our project should have two files and one folder for Grunt to be in operation, so let's create those. Our project directory should look like this now:

```
app/
  -- node_modules/
  -- Gruntfile.js
  -- package.json
```

Let's talk about these two files a bit:

1. Package.json - this file is used by npm to store metadata for projects published as npm modules. You will list grunt and the Grunt plugins your project needs as devDependencies in this file (taken from the Grunt docs).
2. Gruntfile - this file is named `Gruntfile.js` or `Gruntfile.coffee` and is used to configure or define tasks and load Grunt plugins (taken from the Grunt docs). For this article, I'll be using `Gruntfile.js`.

The Grunt docs also gives us some starter documentation for our package.json file. In its barebones state though, it should look like this:

```javascript
{
  "name": "my-project-name",
  "version": "0.1.0",
}
```

Now, we need to actually install Grunt in our project directory. Installing Grunt and Grunt plugins follows the same format, and the easiest way is to run the command `npm install <module> --save-dev`. It will save the module locally, and also update the package.json file, adding the module to the `devDependencies` section. To install the latest version of Grunt, run this command:

```bash
npm install grunt --save-dev
```

Check out your `node_modules` directory and your newly updated `package.json` file. The directory should now have the `Grunt` module, and the package file should look something like this:

```javascript
{
  "name": "my-project-name",
  "version": "0.1.0",
  "devDependencies": {
    "grunt": "^0.4.5"
  }
}
```

Pretty neat! Let's move onto the Gruntfile now. The Gruntfile is any valid JavaScript or CoffeeScript (JS in our case) and contains a wrapper function, the project and task configurations, the grunt plugins and tasks, and the custom tasks. In its barebones state, it looks like this:

```javascript
module.exports = function(grunt) {

  // Project configuration.
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json')
  });

};
```

The "wrapper" function is the `module.exports` function. Then we're calling on grunt to run our initial configuration, and telling it to read our `package.json` file. This will essentially be the stripped down version of a Gruntfile for any new Grunt project. Pretty simple stuff so far! But where does the automation kick in? Let's add a plugin.

## Adding A Plugin

Let's assume we development version of a JavaScript file for this sweet new app we're building. When production time comes, we're going to want to call upon that `sweetapp.min.js` file. A minifier plugin is in order. If we head over to the [official Grunt plugins directory](http://gruntjs.com/plugins), we'll find that uglify plugin easily. Follow the link to the [npm page for the plugin](https://www.npmjs.org/package/grunt-contrib-uglify), and scroll down to the read me section, and have a quick skim through.

Back in our command line, we'll run this command like the instructions said:

```bash
npm install grunt-contrib-uglify --save-dev
```

Check out the updated package file and modules directory to see the changes. Now we need to enable the plugin inside our Gruntfile as they mentioned. Our Gruntfile should now look like this:

```javascript
module.exports = function(grunt) {

  // Project configuration.
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json')
  });

  // Load plugins.
  grunt.loadNpmTasks('grunt-contrib-uglify');

};
```

And in just a couple minutes, we've successfully added a plugin and are ready to wire it up.

## Wiring Up Grunt Plugins

If we continue reading through the plugin documentation, we'll see that there are a host of commands at your disposal. For now, we'll just use it in its basic form to see how it works. In our Gruntfile in the project and task configuration section, initiate uglify like this:

```javascript
uglify: {
}
```

Let's assume our project directory has the following structure and development files:

```
app/
  -- Gruntfile.js
  -- js/
     -- sweetapp.js
  -- node_modules/
     -- grunt/
     -- grunt-contrib-uglify/
  -- package.json
```

We want to uglify our `sweetapp.js` development file, and save a copy of it as `sweetapp.min.js`. Using the basic compression setup from the docs, we include this:

```javascript
my_target: {
  files: {
    'js/sweetapp.min.js': ['js/sweetapp.js']
  }
}
```

Last but not least, we need to tell grunt what tasks to perform when the `grunt` command is run from the terminal. For this, we'll need the `registerTask` command, and we'll call it on the Grunt default for now. To do it, add this at the bottom of your wrapper function:

```javascript
// Default tasks.
grunt.registerTask('default', ['uglify']);
```

Just to do a quick recap, our Gruntfile in its entirety should look like this now:

```javascript
module.exports = function(grunt) {

  // Project and task configuration.
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    uglify: {
      my_target: {
        files: {
          'js/sweetapp.min.js': ['js/sweetapp.js']
        }
      }
    }
  });

  // Load plugins.
  grunt.loadNpmTasks('grunt-contrib-uglify');

  // Default tasks.
  grunt.registerTask('default', ['uglify']);

};
```

Now, let's run the `grunt` command. If all is a success, you should see something like this in your console:

```bash
Running "uglify:my_target" (uglify) task
>> 1 file created.

Done, without errors.
```

Go and check up on your JS directory and you should see your new uglified, production-ready file. Pretty sweet! As it stands right now though, we still have to run `grunt` manually from the command line each time we want to perform our tasks. We're still missing out on a very important part of Grunt, which is automation.

## Automating Tasks With Grunt

Ideally, we want our grunt command to be "watching" for changes to our files in question in our tasks. Guess what? This is easier than you can imagine, because of the "watch" plugin. Let's [head over to the plugin page](https://www.npmjs.org/package/grunt-contrib-watch) and install it according to the readme. You'll notice it's similar to when we installed the grunt plugin. This is the command:

```bash
npm install grunt-contrib-watch --save-dev
```

And once again, our modules directory and package file will automatically be updated. Check it to make sure. Make sure to update your Gruntfile to load the npm task also:

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

Now, we can run this command to watch for changes:

```bash
grunt watch
```

First though, we have to set up our "watch" task so that it runs whatever tasks we want when changes are detected. For our set up, we want to watch for changes and run the uglify task automatically. To do that, we just need to follow the pattern in the plugin documentation, and tailor it to our project. Here's what our complete Gruntfile should look like:

```javascript
module.exports = function(grunt) {

  // Project and task configuration.
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    uglify: {
      my_target: {
        files: {
          'js/sweetapp.min.js': ['js/sweetapp.js']
        }
      }
    },
    watch: {
      scripts: {
        files: ['js/sweetapp.js'],
        tasks: ['uglify']
      },
    },
  });

  // Load plugins.
  grunt.loadNpmTasks('grunt-contrib-uglify');
  grunt.loadNpmTasks('grunt-contrib-watch');

  // Default tasks.
  grunt.registerTask('default', ['uglify']);

};
```

Now, when we run the `grunt watch` command, we can just forget about it. Automation at its finest! Go on, run the command and start to type some JavaScript in your `sweetapp.js` file. Now save it, and check out the command line. You should see something like this:

```bash
>> File "js/sweetapp.js" changed.
Running "uglify:my_target" (uglify) task
>> 1 file created.

Done, without errors.
```

Check out the `sweetapp.min.js` file and you'll see that it's been automatically uglified and saved, without having to run any more commands. That, ladies and gentlemen, is awesome.

## Wrap Up

So there you have it. Task running meets automation meets all your grunt work being a breeze. Get your head into it, and start implementing it in your projects today. Don't forget that there's a whole host of plugins ranging from Sass processing to image optimisation to handlebars compilation. That, combined with the [documentation](http://gruntjs.com/getting-started), should leave you in really good standing to take your projects to the next level professionally. 

I hope you enjoyed this getting started tutorial, and found it useful. Please feel free to leave any questions, comments, or feedback below. Thanks for reading!