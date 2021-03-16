## A Guide to Clipping Paths

Seen a bunch about `clip-path` recently. And with `path` values now supported in major browsers, figured it was time to write up a guide.

What's a `clip-path`?
---------------------

It does what it says on the tin. It clips the viewable parts of an element to a given path.

    /* What shape would this give you? Try walking the coordinates in your head */
    .clipped {
      clip-path: polygon(100% 0, 20% 50%, 35% 50%, 0% 100%, 70% 50%, 50% 50%);
    }

When to `clip-path`?
--------------------

It comes in handy when a combination of `overflow`, `transform`, `border-radius`, and other properties won't cut it. In most cases, we're able to achieve what we want without reaching for it. But, some shapes are tricky without it.

For example, how to make a [star shape](https://codepen.io/jh3y/pen/XWNjBQM)!

Check out [this pen](https://codepen.io/jh3y/pen/XWNjBQM) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

How to `clip-path`?
-------------------

This [demo](https://codepen.io/jh3y/pen/XqVQqa) will throw you straight in so you can see some of the things we can do with it (_Best viewed in its own tab_).

Check out [this pen](https://codepen.io/jh3y/pen/XqVQqa) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

You'll notice that each value is using percentages. We could use other sizing units such as `px`, `rem`, etc. But, this will vary per design and use case. Using percentages has the benefit of keeping things responsive (_More on this later_). What are those values relative to? Clip paths work off the same coordinate system that we use with other CSS properties. Think about it the same as using `top`, `right`, `bottom`, and `left`. Each value or coordinate is relative to the element it's applied to.

* * *

With that out of the way, let's dig into the different values and what we can do with them.

Inset
-----

Define an inset rectangle where everything outside is hidden away. But, this is a little misleading. It doesn't have to be "inset". Inset has two particularly good use cases. Clipping blocks of an element and using it as a "controlled" overflow. Whereas `overflow: hidden` will hide everything outside the bounding box. We can use an inset `clip-path` to allow overflow in certain directions. And that's because we can use negative values with `clip-path`.

For example, this `clip-path` would allow overflow out of the top edge and not the others.

    .overflow--top {
      clip-path: inset(-100% 0 0 0);
    }

Consider this demo of an animated rocket. We want the rocket to only be clipped from the bottom as it comes out of the opening. This is a good use case for controlled overflow.

Check out [this pen](https://codepen.io/jh3y/pen/gOLwBdR) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

The last thing to mention for `inset` is that you can apply a corner radius. To do this, use `round` followed by a radius. This works with short-hand much like `padding`, `margin`, etc.

For example,

    .rounded-inset {
      clip-path: inset(0 0 50% 0 round 10% 25% 50% 0%);
    }
    .rounded-inset {
      clip-path: inset(0 0 50% 0 round 10% 25%);
    }

Have a play with [this demo](https://codepen.io/jh3y/pen/JjbRayB) to see how controlled overflow can work. Rounded corners are applied in the demo and notice how they disappear at certain values.

Check out [this pen](https://codepen.io/jh3y/pen/JjbRayB) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

Circle
------

Circle clips aren't ones that come up often. This is partly due to the fact we can achieve the same result with the use of `border-radius` in many cases.

    .circle-clip {
      clip-path: circle(50%);
    }

The value for `circle` is the `radius` relative to the element. A `50%` radius will create a circle that covers an element. Equal to using this.

    .circle-clip {
      border-radius: 50%;
      overflow: hidden;
    }

The magic with using `circle` is when we define both a `radius` and a `position`.

    .clipped-offset-circle {
      clip-path: circle(50% at 100% 25%);
    }

Those two final values are position `x` and position `y`. The use of positioning is what gives us interesting use cases for `circle` clipping. This defaults to `50% 50%` if not defined.

For example, how about an image reveal on `:hover`? In this demo, we overlay two images. Then we reveal a color version using a transitioned `clip-path`. Play with the values to make the image bloom from different positions.

Check out [this pen](https://codepen.io/jh3y/pen/VwmKVrr) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

Here's the same demo without the reveal. It will get you comfortable with `circle` clips. Try changing the positions and radius.

Check out [this pen](https://codepen.io/jh3y/pen/BaQLvjX) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

Ellipse
-------

The `ellipse` value works almost exactly the same as `circle`. The only difference is that we get to define both a horizontal and vertical radius. Again, the position is optional.

    .ellipse-clip {
      clip-path: ellipse(50% 25% at 50% 50%);
    }

Try it out!

Check out [this pen](https://codepen.io/jh3y/pen/KKNgbrq) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

Polygon
-------

Now things get interesting. The `polygon` clip allows you to define a polygon with as many points as you like for clipping an element. Each point is a coordinate relative to the element. When approaching a `polygon` clip, try walking the coordinates in your head. This I find is the best approach for `polygon` paths.

For example, start with this basic polygon.

    .polygon-clip {
      clip-path: polygon(0 0, 100% 0, 100% 100%, 0 100%);
    }

That's right! It would be the same as `inset(0 0 0 0)`. The clip we defined right at the top of this article?

    .clipped {
      clip-path: polygon(100% 0, 20% 50%, 35% 50%, 0% 100%, 70% 50%, 50% 50%);
    }

This would give you a thunderbolt like shape!

Check out [this pen](https://codepen.io/jh3y/pen/vYyXbKN) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

The cool thing about `polygon` is that you can create any polygon you can think of. For fun, I recreated the ever-popular ["Shapes of CSS"](https://css-tricks.com/the-shapes-of-css/) article shapes with `polygon` clips.

Check out [this pen](https://codepen.io/jh3y/pen/gOpLBEa) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

Try adding and removing nodes in this demo to make different polygons.

Check out [this pen](https://codepen.io/jh3y/pen/RwoGvZR) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

You may have noticed something when playing with the nodes. You can create "faux" breaks in the path depending on the route you take.

If you cross the paths between nodes, you're able to cut parts away.

Check out [this pen](https://codepen.io/jh3y/pen/dyOOOPB) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

If you duplicate some of the points and walk a line, you can create separated shapes.

Check out [this pen](https://codepen.io/jh3y/pen/QWGGGLv) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

This has the benefit of maintaining responsiveness.

Path
----

For everything else with `clip-path`, there's `path`. This one's the most flexible. It's now supported in all the major browsers! But, it has its caveats. Using a `path` value means passing in a path definition string. This is the same path we use for an SVG path. And it's relative to the dimensions of our element.

    /* Any guess at what shape this is? */
    .path-clipped {
      clip-path: path("M 10,30
               A 20,20 0,0,1 50,30
               A 20,20 0,0,1 90,30
               Q 90,60 50,90
               Q 10,60 10,30 z")
    }

It means we can create some interesting effects that could otherwise be very tricky. The alternative would be reaching for SVG or using CSS masking.

Check out [this pen](https://codepen.io/jh3y/pen/oNYzVNY) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

Consider this clip. It's a sunshine clip. The `path` breaks into different segments. This is something we can't achieve with the other clip values. Unless we use a lot of Math or the "faux" technique we mentioned with `polygon`. We're combining rounded shapes with polygons too. This is the power of using `path`. The ability to create irregular shapes or paths that break. This gives us lots of opportunity. To note, I created the path there by resizing an SVG icon in Figma, and then lifting the path definition out of the file. That took maybe a minute or so.

The caveat to using `path` is the path definition itself. As mentioned, it's relative to the dimensions of our element. This is where things get tricky. It's not responsive. A path definition uses pixels that are relative to an SVG viewBox. When applied to an element, the element's width and height become an implied viewBox. This is the same hurdle that CSS motion path faces. I wrote about [ways to tackle this before](https://css-tricks.com/create-a-responsive-css-motion-path-sure-we-can/). The best approach if we decide to use `path` is to go with concrete sizing. Or create scaled versions of our path to complement.

We should only reach for `path` when we need very irregular shapes. The lack of responsiveness adds a hurdle. With that in mind, if we can create the shape we need with regular "responsive" clips and shapes, do that. Alternatively, use SVG or CSS masking if possible.

* * *

Animation && Transition
-----------------------

As seen in some of the demos above, we can transition or animate `clip-path`. There's one condition. The `path` must have a consistent structure. For example, if we transition a polygon, that polygon must have a consistent number of points.

Consider this "Avengers" themed animation that cycles through different `polygon` clips.

Check out [this pen](https://codepen.io/jh3y/pen/xjYmVg) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

This demo for image reveals showcases a variety of different shapes being transitioned. The "start" and "end" clips are inlined as CSS variables for each image container.

Check out [this pen](https://codepen.io/jh3y/pen/LYGaNby) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

This will even work with `path` paths. Try hovering over the moon here.

Check out [this pen](https://codepen.io/jh3y/pen/qBqavMX) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

The key thing is that the paths transitioned to and from have the same number of nodes or points. And that they have the same type of `clip-path`.

Case Studies && Use Cases
-------------------------

There isn't a great place to put these observations. But, "Case Studies" kinda feels right. These are things that could be worth noting and make you consider approaches. In particular, whether `clip-path` is right for the job.

### Reveal Scenarios

The use of `clip-path` is perfect for any type of "Reveal" scenario. This is where we can make a clear distinction between a reveal and a scale. A reveal **does not** distort its content.

Consider this demo. It's a Twitch stream overlay concept where the banners animate in.

Check out [this pen](https://codepen.io/jh3y/pen/poNEXZo) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

But, the transition uses `scaleX` and that distorts the banner content. This is a great use case for a reveal.

In that demo, we're doing this.

    @keyframes growIn {
      from {
        transform: rotate(calc(var(--rotate, 0) * 1deg)) scaleX(0);
      }
    }

But, it would make more sense if we switched that animation around. For this, we could use scoped variables.

    .banner {
      clip-path: inset(0 0 0 0);
    }
    .banner--horizontal {
      --clip: inset(0 0 0 100%);
    }
    .banner--vertical {
      --clip: inset(0 100% 0 0);
    }
    @keyframes growIn {
      from {
        clip-path: var(--clip);
      }
    }

And now our banners would reveal the information and not distort it!

Check out [this pen](https://codepen.io/jh3y/pen/XWNjLyj) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

Reveals make a great use case for clip-path usage. We can use this image reveal demo again for another example.

Check out [this pen](https://codepen.io/jh3y/pen/LYGaNby) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

### Skewed Sections

This design phenomenon has taken off in recent years. The skewing of page sections and content. I got thinking about this when approached about how to create a certain effect. Let's look at some examples.

Consider this page with some basic sections.

Check out [this pen](https://codepen.io/jh3y/pen/bGBwJaE) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

We want each division to have an angle. It's tempting to reach for a `clip-path` here. Using `polygon` would seem apt.

    section {
      clip-path: polygon(0 20%, 100% 0%, 100% 80%, 0 100%);
    }

And, that works to an extent. All we'd need is a way to cover those corners for the first section.

Check out [this pen](https://codepen.io/jh3y/pen/ExNgJEL) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

We could change it so that it's applied to `even` sections. But, then we'll get an issue at the end of the document.

What if want to apply a background color to every section? That's tricky because the sections don't meet now that they're clipped.

Check out [this pen](https://codepen.io/jh3y/pen/dyOpLjr) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

We could make use of pseudo-elements and use coordinates that go outside the bounds of the section. But, that could get complicated and hard to maintain. We have to start managing overlaps and using extra elements. Plus we need to calculate the angle.

The trick here is to think about what we're trying to do. The clue is in the goal. We want to skew the sections. And CSS transforms will let us do this.

    section {
      transform: skewY(-11deg);
    }
    section > * {
      transform: skewY(11deg);
    }

That small snippet of CSS gets us most of the way and we don't need to handle clip management. This is a rudimentary approach.

Check out [this pen](https://codepen.io/jh3y/pen/xxREeeQ) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

Take it a little further with pseudo-elements and you've got something quite usable.

Check out [this pen](https://codepen.io/jh3y/pen/ExNgzxO) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

In fact, Nils Binder [covers different ways to approach these diagonal sections](https://9elements.com/blog/pure-css-diagonal-layouts/). This playground using the `transform` approach is perfect and handles how to cover the corners, etc.

Check out [this pen](https://codepen.io/jh3y/pen/yLyrmyg) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

The gist here is that because we can with `clip-path` doesn't mean we should. It's an interesting case of looking at different ways to approach the same problem. Something that often comes up when we do things with CSS.

### Glitchy Effects

Glitchy effects are a great use case for using `clip-path`. Because we can animate a `clip-path`, moving an `inset` clip over an element can create the effect. It's what's used for these CSS Cyberpunk 2077 buttons.

The "trick" for animated glitch effects is using an animated clip on a clone element.

Check out [this pen](https://codepen.io/jh3y/pen/jOMBRmY) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

A few steps later with the buttons and we get to this.

Check out [this pen](https://codepen.io/jh3y/pen/PoGbxLp) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

You can read about how to make them [here](https://jhey.dev/writing/css-cyberpunk-2077-buttons-taking-your-css-to-night-city/).

That's It!
----------

That's all you might need to know about `clip-path`... for now. Got a cool demo or a use case that's of interest. Share it with me!