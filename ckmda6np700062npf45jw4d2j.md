## CSS Animated Google Fonts

Google Fonts now supports variable fonts! That's pretty neat.

> [Check out this tweet!](https://twitter.com/jh3yy/status/1278290708626104322?ref_src=twsrc%5Etfw)

What are variable fonts?
------------------------

If you're not familiar with variable fonts. They're pretty cool. The biggest win is having access to font variations with fewer network requests. This results in reduced file size for your fonts!

All made possible by cool new features. Features that allow us to change the characteristics of a font such as the `weight` and `slant` at runtime. Besides the practical advantages, it means we can also animate these characteristics!

We refer to these characteristics as the axis. And a variable font doesn't have to support them all, it may choose to only support one or two.

Axis name

CSS value

Weight

wght

Width

wdth

Slant

slnt

Optical Size

opsz

Italics

ital

Grade

GRAD

For more on variable fonts, [check this article!](https://web.dev/variable-fonts/)

What about Google Fonts then?
-----------------------------

Well, there's one thing that isn't clear when using variable fonts from Google Fonts. How do you get the entire font?

Let's say we choose "Roboto Mono" and set that up.

    @import url('https://fonts.googleapis.com/css2?family=Roboto+Mono&display=swap');
    
    h1 {
      font-family: 'Roboto Mono', monospace;
    }

Now we decide to change the variable font settings. We know that "Roboto Mono" supports "Variable weight axis". If we try updating the weight in our example, nothing happens...

    h1 {
      font-variation-settings: 'wght' 300;
    }
    h1 {
      font-variation-settings: 'wght' 700;
    }

Hmm. What's wrong?

Check out [this pen](https://codepen.io/jh3y/pen/GRoOboo) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

If we check out a particular weight and then use "Select this style" we can grab the weights we want!

![Snap of Google Fonts variable weight range configuration](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1615975488189/FCNwfH68A.png)

And updating our CSS will reflect that weight.

Check out [this pen](https://codepen.io/jh3y/pen/gOPXNMr) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

For many weights, enter the weights followed by a semi-colon.

    @import url('https://fonts.googleapis.com/css2?family=Roboto+Mono:wght@200;700&display=swap')

Hover the word in this demo to transition to a higher `wght`.

Check out [this pen](https://codepen.io/jh3y/pen/wvMPLzJ) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

I have noticed something interesting here though. Enter any two values from the range and you'll have access to the entire range!

For example, here I use

    @import url('https://fonts.googleapis.com/css2?family=Roboto+Mono:wght@699;700&display=swap')

But I still have access to weights from the entire supported range.

Check out [this pen](https://codepen.io/jh3y/pen/OJMOepW) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

Wonder if this will change in the future? Or maybe it's a case of entering any two values triggers the fact you want the variable version?

How do we animate them?
-----------------------

No different from a normal CSS animation. We can animate the "font-variation-settings" property.

    @keyframes breathe {
      50% {
        font-variation-settings: 'wght' 700;
      }
    }

Here we use some variables to dictate the lower and upper band.

Check out [this pen](https://codepen.io/jh3y/pen/RwrjzZa) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

How do we take it further and stagger the characters? For this, we can split up the characters in our HTML.

    <h1>
      <span style="--index: 0">W</span>
      <span style="--index: 1">a</span>
      <span style="--index: 2">a</span>
      <span style="--index: 3">a</span>
      <span style="--index: 4">v</span>
      <span style="--index: 5">e</span>
    </h1>

And notice how we are using an inline CSS variable for the character index? We can use that to dictate the animation delay of a character. A scoped variable means less CSS.

    h1 span {
      animation-delay: calc((var(--index) - 6) * 0.25s);
    }

And that will give us this.

Check out [this pen](https://codepen.io/jh3y/pen/bGEYPLo) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

We could take that a little further and add some color and stack the words 😎 Mix in what we have some CSS transforms and there's a bunch we can do!

Check out [this pen](https://codepen.io/jh3y/pen/WNrXqYz) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

For my original demo, I threw something together with GreenSock and Splitting.

Check out [this pen](https://codepen.io/jh3y/pen/OJMOmRL) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

But, there's no reason we can't do something similar with CSS alone.

Check out [this pen](https://codepen.io/jh3y/pen/GRoOVNy) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

We can swap the saturation out as we were by creating a similar keyframe.

    @keyframes rise {
      50% {
        font-variation-settings: 'wght' var(--upper);
        color: hsl(var(--hue), 80%, 65%);
      }
    }

That's it!
----------

Grab some variable fonts from Google Fonts and have a play! I haven't been through all the fonts yet. As of yet, I've only seen support for the `weight` axis.

If you find any that support the other axis, let me know!

Got any good variable fonts resources? I'd love to see them!