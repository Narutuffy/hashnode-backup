## CSS Animation Superpowers with @property

There's a new CSS feature on the way that will give you CSS animation superpowers!

For those in camp **TL;DR**, `@property` provides a way for the browser to type check your CSS variables. By doing that, the browser is then able to animate and transition those properties!

    @property --spinAngle {
      initial-value: 0deg;
      inherits: false;
      syntax: '<angle>';
    }
    
    @keyframes spin {
      to {
        --spinAngle: 360deg;
      }
    }

[Browser compatibility](https://caniuse.com/mdn-css_at-rules_property) is getting there too.

Have a play with the demos in this [CodePen collection](https://codepen.io/collection/AazyEP).

Check out [this pen](https://codepen.io/jh3y/pen/OJRLMxE) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

You can also watch this screencast where I go through how to use it.

For those sticking around, let's dig in and see what we can do with CSS variables and animation.

* * *

Something I've always enjoyed doing is trying to push the limits on my CSS animation. When CSS variables came about, one of my first questions was "Can we animate them?". The answer was "No".

But, we could still use them in our CSS animations for different reasons.

We couldn't animate the properties. But, we could use them to make our animation dynamic. In fact, I tweeted about it when I found out.

> [Check out this tweet!](https://twitter.com/jh3yy/status/1191695646752948225?ref_src=twsrc%5Etfw)

This demo even got printed in a magazine!

![Demo shown in net magazine - January 2020](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1615889346635/MU2dDXgho.gif)

This makes for some very interesting opportunities when you start inlining properties. Check out [this article](https://jhey.dev/writing/the-power-and-fun-of-scope-with-css-custom-properties/) about the power and fun of variable scope.

One of the neatest things you can do with CSS variables and animation is using them for composition. By that, we mean using them to define the speeds and delays of animations. That makes it easier to manage things like timelines. In fact, Carl Schoof did a fun animation challenge series on this. Here was my 3b solution.

Check out [this pen](https://codepen.io/jh3y/pen/dyYbxda) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

Here's a roundup of how this is working (Skip to the 6-minute mark).

The New @property
-----------------

What does the new `@property` give us then? Let's start by saying it's still making its way into browsers. Currently, Chrome and Edge have good support. You can check out [browser compatibility here](https://caniuse.com/mdn-css_at-rules_property).

This new feature allows us to define types for your custom properties. Types like `angle`, `number`, `percentage`, etc.

This "type checking" gives the browser extra contextual information. It can use this to transition and animate custom property values. That's the magic right there.

This means we can animate things that once we could not.

*   Color stops in a `linear-gradient`
*   Angle of a `conic-gradient`
*   Hue of a HSL color
*   Individual transform values
*   Whatever else your imagination can think of!

Check out [this pen](https://codepen.io/jh3y/pen/YzpKKoN) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

How do we register a custom property? It's as straight forward as

    @property --spinAngle {
      initial-value: 0deg;
      inherits: false;
      syntax: '<angle>';
    }

*   `inherits` - Whether the property inherits the value from its parent
*   `initial-value` - Fallback/initial value
*   `syntax` - Property type. Check out [other types here](https://web.dev/at-property/#syntax).

Examples
--------

Let's walk through some examples.

### Waves

Let's start with some waves. For this example, we don't even need any elements. We're going to animate a `linear-gradient` on the `body`.

Let's start by defining our custom property `--wave`.

    @property --wave {
      inherits: false;
      initial-value: 0%;
      syntax: '<percentage>';
    }

Then we'll create a `linear-gradient` that makes waves using `calc` and our `wave` property.

    background: linear-gradient(transparent 0 calc(35% + (var(--wave) * 0.5)), var(--wave-four) calc(75% + var(--wave)) 100%),
                linear-gradient(transparent 0 calc(35% + (var(--wave) * 0.5)), var(--wave-three) calc(50% + var(--wave)) calc(75% + var(--wave))),
                linear-gradient(transparent 0 calc(20% + (var(--wave) * 0.5)), var(--wave-two) calc(35% + var(--wave)) calc(50% + var(--wave))),
                linear-gradient(transparent 0 calc(15% + (var(--wave) * 0.5)), var(--wave-one) calc(25% + var(--wave)) calc(35% + var(--wave))), var(--sand);

That might look a little confusing. We're layering up background images to create many waves. We then use `calc` to calculate the color stops for each individual gradient.

Here are the static waves.

Check out [this pen](https://codepen.io/jh3y/pen/GRNKqWy) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

To bring them to life. Animate that value.

    body {
     animation: waves 5s infinite ease-in-out;
    }
    @keyframes waves {
      50% {
        --wave: 25%;
      }
    }

And we have animated waves

Check out [this pen](https://codepen.io/jh3y/pen/YzpKKoN) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

### Party Button

Playing with HSL is a lot of fun. But creating a good animation that loops through the hue wheel is a little tricky. It's doable with preprocessors. I've done things like this in the past

    @keyframes party
      for $frame in (0..100)
        {$frame * 1%}
          background 'hsl(%s, 65%, 40%)' % ($frame * 3.6)

But, with @property, we can animate the hue itself! Hover the button in this pen.

Check out [this pen](https://codepen.io/jh3y/pen/OJRLMxE) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

The border color will animate through the hue spectrum. And it's as easy as this

    @property --hue {
      syntax: '<integer>';
      inherits: true;
      initial-value: 0;
    }
    button {
      --border: hsl(var(--hue, 0), 0%, 50%);
      border-color: var(--border);
    }
    button:hover {
      --border: hsl(var(--hue, 0), 80%, 50%);
      animation: hueJump 0.75s infinite linear;
    }
    @keyframes hueJump {
      to {
        --hue: 360;
      }
    }
    

### Travelling Car

This last one's interesting. We want to make a little car go around the track. And we're going to use `transform`. This is already possible if we use wrapper elements. But, with `@property`, we won't have to. We can animate separate properties on the same keyframes.

Check out [this pen](https://codepen.io/jh3y/pen/vYyBKeW) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

To position the car, we've created three properties.

    @property --x {
      inherits: false;
      initial-value: -22.5;
      syntax: '<number>';
    }
    @property --y {
      inherits: false;
      initial-value: 0;
      syntax: '<number>';
    }
    @property --r {
      inherits: false;
      initial-value: 0deg;
      syntax: '<angle>';
    }

The `--x` and `--y` properties will dictate the x and y position of the car. Whilst, `--r` will dictate the rotation. We're using `number` because we want to keep the scene responsive and I know the road is a `50vmin` square.

The transform for the car looks like this:

    transform: translate(calc(var(--x) * 1vmin), calc(var(--y) * 1vmin)) rotate(var(--r));

To animate the car, we'll create one set of keyframes called "journey". And let's start by animating the `--x` property.

    @keyframes journey {
      0%, 100% {
        --x: -22.5;
      }
      25% {
        --x: 0;
      }
      50% {
        --x: 22.5;
      }
      75% {
        --x: 0;
      }
    }

That's going to give us the correct x-coordinate animation.

Check out [this pen](https://codepen.io/jh3y/pen/zYoOBWq) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

Let's add the `--y` property updates to the keyframes.

    @keyframes journey {
      0%, 100% {
        --x: -22.5;
        --y: 0;
      }
      25% {
        --x: 0;
        --y: -22.5;
      }
      50% {
        --x: 22.5;
        --y: 0;
      }
      75% {
        --x: 0;
        --y: 22.5;
      }
    }

And now the car moves in a diamond shape. Not quite right.

Check out [this pen](https://codepen.io/jh3y/pen/mdObELx) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

To combat that, we need to make sure we have the correct `--x` and `--y` at the corners. We could do that with some extra steps in the keyframes.

    @keyframes journey {
      0%, 100% {
        --x: -22.5;
        --y: 0;
      }
      12.5% {
        --x: -22.5;
        --y: -22.5;
      }
      25% {
        --x: 0;
        --y: -22.5;
      }
      37.5% {
        --y: -22.5;
        --x: 22.5;
      }
      50% {
        --x: 22.5;
        --y: 0;
      }
      62.5% {
        --x: 22.5;
        --y: 22.5;
      }
      75% {
        --x: 0;
        --y: 22.5;
      }
      87.5% {
        --x: -22.5;
        --y: 22.5;
      }
    }

And that gives us the car in the right position!

Check out [this pen](https://codepen.io/jh3y/pen/BaQBzVG) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

Last up, the rotation. For this, we can update the `--r` property where we want by adding in extra steps for our keyframes. Here, we've gone with a 5% window around the corners.

    @keyframes journey {
      0% {
        --x: -22.5;
        --y: 0;
        --r: 0deg;
      }
      10% {
        --r: 0deg;
      }
      12.5% {
        --x: -22.5;
        --y: -22.5;
      }
      15% {
        --r: 90deg;
      }
      25% {
        --x: 0;
        --y: -22.5;
      }
      35% {
        --r: 90deg;
      }
      37.5% {
        --y: -22.5;
        --x: 22.5;
      }
      40% {
        --r: 180deg;
      }
      50% {
        --x: 22.5;
        --y: 0;
      }
      60% {
        --r: 180deg;
      }
      62.5% {
        --x: 22.5;
        --y: 22.5;
      }
      65% {
        --r: 270deg;
      }
      75% {
        --x: 0;
        --y: 22.5;
      }
      85% {
        --r: 270deg;
      }
      87.5% {
        --x: -22.5;
        --y: 22.5;
      }
      90% {
        --r: 360deg;
      }
      100% {
        --x: -22.5;
        --y: 0;
        --r: 360deg;
      }
    }

And that's it! The little car image will make it's journey around the square. No wrapper elements required. We don't even need to write out extra transforms.

Check out [this pen](https://codepen.io/jh3y/pen/jOVNqjv) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

That's it!
----------

Check out the demos in this [collection](https://codepen.io/collection/AazyEP). There are exciting times ahead with CSS Houdini. I'm excited about it. I'm also excited to see what you'll make!

Check out [this pen](https://codepen.io/jh3y/pen/OJRLMxE) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).