## CSS Aspect Ratio

Aspect ratio is an interesting problem. How do you tackle it with CSS? Especially if you don't know the dimensions you're dealing with. CSS has a new property on the way to deal with this. We'll give this a _very_ quick look. And then, let's take a look at other ways you can handle this that currently have better browser support.

`aspect-ratio`
--------------

CSS has a new property on the way. It's for _prescribing_ aspect ratio to an element 🙌

And it's as straightforward as

    .element {
      aspect-ratio: 16 / 9;
    }

It's important to note that this property prescribes an aspect ratio. Our elements won't respect it if we define both a height and width for an element.

The [support isn't quite there yet](https://caniuse.com/mdn-css_properties_aspect-ratio) though.

Here's a [demo](https://codepen.io/jh3y/pen/OJRYmGr) to play with. It _should_ work in Chrome 88+.

Check out [this pen](https://codepen.io/jh3y/pen/OJRYmGr) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

And here's a screencast.

The `padding` Trick
-------------------

If you don't know the dimensions of your content but your content has a constrained width, you can use this. The trick is to wrap your content in a container and pad that container out using `padding-bottom`. You then absolutely position your element into that container.

    .wrap {
      overflow: hidden;
      padding-bottom: 56.25%
      position: relative;
      width: 100%;
    }
    .element {
      position: absolute;
      height: 100%;
      left: 0;
      object-fit: cover;
      top: 0;
      width: 100%;
    }

The trick here is to work out the correct padding for your desired aspect ratio.

If the ratio was `16:9`, the padding required is 56.25%. We can work this out with

    aspect ratio height / aspect ratio width
    

For example, `9 / 16 = 56.25`.

Check out [this pen](https://codepen.io/jh3y/pen/abmgRZe) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

This works because of the absolute positioning. We are letting the browser determine what height the container should be.

Use CSS Variables
-----------------

The last way works if you know one of the dimensions for your element. We can use CSS variables and `calc` to make CSS work out the element size for us. This is the gist of it.

    --aspect-ratio: calc(16 / 9);
    --width: 400px;
    --height: calc(var(--width) * var(--aspect-ratio));

You could swap that around so the width is being calculated. Or, you might use the aspect-ratio variable scoped when you need it.

    .element {
      width: 160px;
      height: calc(160px * var(--aspect-ratio));
    }

This way has a lot of possibilities. And it's down to your design how you'd go about using this approach.

Have a play with this [demo](https://codepen.io/MWjMzwW)

Check out [this pen](https://codepen.io/jh3y/pen/MWjMzwW) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

And here's a walkthrough using it.

* * *

That's it!
----------

That's a look at different ways to handle aspect ratio with CSS. The new way looks like it's going to be a great addition and will save us some styles.