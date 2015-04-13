## Code Testing - Why The Fuss?

Nowadays, applications of all sizes and types are being developed with testing in mind. But why all the fuss? In a small application, testing might seem like a pointless activity. It might still seem cumbersome and overdone in a larger one. The truth is, though, that once you wrap your head around testing and setting up a testing environment, working on your code and developing your functions become much easier. Put it this way - you always write a function to achieve an end result, and you know what you want the end result to be before you even start writing your function. By testing your code, you're able to describe expected outputs in advance, and build your function step by step, making sure each of the rules pass. If you really drill your function with a series of tests and it passes all, then you'll know for sure that it won't fail under any circumstances. This leads to less bugs, better maintainability, and greater understanding of your code and its behaviour, especially when other people begin to use it.

TDD and BDD stand for "test driven development" and "behaviour driven development" respectively. These two forms of testing are amongst a pool of testing methods. [Mocha](http://mochajs.org/) is a JavaScript testing framework, and [Chai](http://chaijs.com/) is a BDD / TDD assertion library. Both Mocha and Chai can run in Node environments or in the browser. In this tutorial, we're going to set up a testing environment using Mocha and Chai, and run our tests in the browser. Then, we'll create a simple function, and build it up bit by bit with some tests. Let's get started!

## Project Setup

We're working with a basic front end setup here, so let's forget about Node for this tutorial. We'll use [Bower](http://bower.io/), which is a package manager for the web. Let's start with this directory configuration:

```bash
-> js/
    -> functions.js
-> test/
    -> index.html
    -> test.js
```

Now `cd` into that directory and run the following command:

```bash
bower init
```

You'll be presented with a series of configuration steps to initialise Bower, but ultimately, the only parts that matter for now are the name and version. Your `bower.json` file should look something like this:

```json
{
  "name": "Test",
  "version": "1.0.0"
}
```

The `bower_components` directory should have also been created during this step. Now, let's pull in the necessary packages:

```bash
bower install mocha chai
```

Nice, we're ready to rock and roll! Let's get mocha running in the browser. If you take a look at the [Mocha docs](http://mochajs.org/), you'll realise that a lot of it is geared towards functionality within a Node environment and from the command line. Nevertheless, it can run perfectly fine in the browser in a non-node environment. [This section in the documentation](http://mochajs.org/#browser-support) assesses that while also giving us some boilerplate for the `index.html` file in the `test` directory. We'll use that boilerplate, but we'll need to update the paths, remove the unnecessary script references, and include Chai. Here's the final `index.html` file:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Mocha Tests</title>
  <link rel="stylesheet" href="../bower_components/mocha/mocha.css" />
</head>
<body>
  <div id="mocha"></div>
  <script src="../bower_components/chai/chai.js"></script>
  <script src="../bower_components/mocha/mocha.js"></script>
  <script src="../js/functions.js"></script>
  <script>mocha.setup('bdd')</script>
  <script src="test.js"></script>
  <script>
    mocha.run();
  </script>
</body>
</html>
```

Remember, this file is inside the `test` directory! If we navigate to that page in the browser, we should see the Mocha test page with 0 passes, 0 failures, and a duration of 0 seconds. We're now ready to write some tests!

## Testing With Mocha, Asserting With Chai

By default, you can use Mocha's assertion module (which is in fact Node's regular assertion module) to run your tests. However, it can be quite limiting. This is where assertion libraries like Chai enter the frame. We've already included chai in our `index.html` file above, so it's now accessible as a global object. Inside our `test.js` file, we can overwrite the default `expect` and `should` assertion like this:

```javascript
var expect = chai.expect;
var should = chai.should();
```

We'll stick to `expect` for now, but you can read up on the differences [here.](http://chaijs.com/guide/styles/) Writing a test is like constructing a perfect sentence in English. We describe an umbrella of tests, and state some expected outputs for various tests under that umbrella. For example, we could have a series of tests that compare numbers. Our shell would look like this:

```javascript
describe('Compare Numbers', function() {
});
```

Inside that shell, we may want to test two different possible outcomes - 

* equality
* greater than

Let's hard code the return values for now just to see how we'd construct the test. For our first one, let's test if 1 is equal to 1. We'll state our desired outcome, construct the test case, and then refresh the browser. Place the following code inside the shell above:

```javascript
it('1 should equal 1', function() {
  expect(1).to.equal(1);
});
```

Refresh the browser and you'll see that the test passed! Change one of the numbers to a 2 now, and you'll notice that the test fails, giving you a detailed description of where and why. We're now beginning to see the beauty of testing. Let's create our "greater than" test now. Just like before, we'll write it out in plain old English:

```javascript
it('2 should be greater than 1', function() {
  expect(2).to.be.greaterThan(1);
});
```

Refresh the browser again, and now we have two passed tests! For all the possible combinations of assertion constructs and chains, check out the [Chai API.](http://chaijs.com/api/bdd/) For now though, we have enough knowledge to put testing to actual use.

## Testing & Building An Actual Function

The above tests are just about useless, because we've hard-coded the results and forced them to work. In a real-world testing scenario, we want to apply the tests to a function in progress to make it bulletproof at every step. Let's demonstrate this with a very basic function. Our function, `isEven`, will accept one parameter (`num`), and return `true` if the number is even, and `false` if it's odd. It's a borderline trivial function, I know, but grasping the fundamentals will help you approach more complicated functions better. In our `functions.js` file, create the function:

```javascript
function isEven(num) {

}
```

Now, let's start developing it with testing. Remember, we pulled in our `functions.js` file in the test page, so we have access to the functions in it. We'll begin by creating our umbrella of tests:

```javascript
describe('Is Even Tests', function() {
});
```

Now let's create the first test. We know for sure that the return type must be a boolean - either true or false - so let's assert that:

```javascript
it('Should always return a boolean', function() {
  expect(isEven(2)).to.be.a('boolean');
});
```

Even though we're not testing for the passed in parameter yet, we still know that the return type must be a boolean. If we refresh our test page, we'll see that the test has failed, because our function returns nothing at all at the moment. Let's fix that by simply returning `true`:

```javascript
function isEven(num) {
  return true;
}
```

Our tests now pass, which is great, but we surely have more to do. Let's create two more tests:

1. We'll check `isEven(76)` to make sure it returns `true`.
2. We'll also check `isEven(77)` to make sure it returns `false`.

Here's the tests in action:

```javascript
it('Calling isEven(76) sould return true.', function() {
  expect(isEven(76)).to.be.true;
});

it('Calling isEven(77) sould return false.', function() {
  expect(isEven(77)).to.be.false;
});
```

If we refresh the browser, we'll notice that the second test passes (because we're returning `true` by default), but the third one fails. Let's tweak our function to make sure all tests pass. Here's the updated code:

```javascript
function isEven(num) {
  if (num%2 !== 0) return false;
  return true;
}
```

All our tests should pass now! Our function is fairly simple, so at this point, running more tests might seem a bit redundant. For a more complicated function though, you'd keep running tests for various inputs and combinations of inputs. The passing and failing of various assertions helps you to gradually build up your functions. More importantly, it makes refactoring code much, much easier, as you're always able to see exactly where it passes and fails. 

## Wrap Up

And that’s a wrap! In this tutorial, we took a nice look at the wonderful world of code testing, and got our feet a little wet with building up our own function. The river runs much deeper though, and once you open up your mind to testing, you won't look back. Side effects include a boost in efficiency, easier code refactoring, and better code maintenance. Thanks for reading, and if you have and questions, comments, or feedback, you can leave them below.