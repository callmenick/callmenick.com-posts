## A Quick Introduction To BEM

[BEM](https://en.bem.info/) stands for "block element modifier", and is a front-end methodology that lays out some naming rules and conventions. One of the primary goals of BEM syntax is to give CSS authors a set of naming rules to stand by. Another main goal is to enable longevity and scalability in CSS architecture. BEM, in a sense, is relatively strict, but the strictness is only ultimately beneficial to code reuse, scalability, and large teams. While blocks and pages can be [generated on the fly using node](https://en.bem.info/tutorials/quick-start-static/), we're going to stick to just the naming conventions and the implementations. This is what's really important to start implementing BEM in your projects today.

In this article, we're going to cover some BEM basics. We'll also build a BEM-structured component to see it in action, then compare it to a regular generically named one, shedding light on the efficiencies and usefulness of BEM. Finally, we'll assess the strictness of BEM, and point out a couple cases where you don't have to follow the rules all the time. Let's dive in!

## BEM Basics

As I mentioned before, BEM...

1. Block - 
2. Element - 
3. Modifier - 

Here are the combinations:

```css
/* the governing block */
.block {}

/* an element of the block */
.block__element {}

/* a modifier to the block */
.block--modifier {}

/* a modifier to an element of the block */
.block__element--modifier {}

/* an element of a modified block */
.block--modifier__element {}
```

## Example BEM Component - A Modal

## Modal Component Without BEM - A Comparison

## Strict, But Not Too Strict

## Wrap Up

When I first learned about BEM some time ago, I dabbled around and implemented it in a project. Since then, I haven't looked back. I employ this methodology in every single project I do now, and have been meaning to write about it for a while. I hope this article shed some light 