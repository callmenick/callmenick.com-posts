Ah, the good old console in our browsers. Web tools have been providing is with amazing ways to debug our code, and log messages in our scripts using the `console.log` method. But what about debugging in Sass? [Sass](http://callmenick.com/2014/04/29/an-introduction-to-sass-scss/), a CSS pre-processor, has taken the front-end world by storm. Sass allows us to declare variables, write functions, check conditions, perform loops, calculate math, and a whole host of other awesome stuff. With all that though, comes the need for debugging.

Enter the `@debug` directive. According to the Sass docs:

> The `@debug` directive prints the value of a SassScript expression to the standard error output stream.

Let's take a look at how to use it, and a more complicated example and its results.

## The @debug Directive In Action

Using the `@debug` directive is as simple as you'd imagine. Just call the method on something you want output.

```scss
@debug (12px + 4px);
```

The above will output the following to your error stream (in my case, terminal):

```bash
DEBUG: 16px
```

The brackets around what you want evaluated are actually optional, but I use them out of habit, and for easier readability. Let's look at a slightly more complicated example.

```scss
@for $i from 1 through 3 {
  $n: $i*2;
  @debug ($i, $n);
}
```

Here, we're looping over the number 1 to 3 using the `@for` expression, and assigning each iteration to the variable `$i`. We're also reassigning `$n` to `$i*2` on each iteration, and outputting both `$i` and `$n` to the console. The output should look like this:

```bash
DEBUG: 1, 2
DEBUG: 2, 4
DEBUG: 3, 6
```

Pretty neat, huh? This is just a touch on the surface of the usefulness of the `@debug` directive, and also an insight to the beauty of Sass. Building components that rely on iterated evaluations just got a whole lot easier, and checking out the outputs before actually outputting the CSS is a breeze with the `@debug` directive!