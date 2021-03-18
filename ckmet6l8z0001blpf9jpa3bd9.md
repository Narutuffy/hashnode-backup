## Get the Picture - Responsive Image Sizing && Formatting

How do you display an image in the browser?

    <img src="awesome.png" alt=""/>

If it's decorative, you use CSS?

    .decorative { background-image: url(awesome.png); }

Both are valid solutions. But.

*   What do you do when you want to make your images responsive?
*   What about different screen resolutions?
*   What if you want to display modern formats to the user?
*   What's the `picture` element?

These are things I wish someone had told me in a concise way earlier in my career. Things that we're often abstracted away from depending on the tools we use. This is one of the more complicated HTML elements to deal with.

Let's dig in!

The TL;DR
---------

Use the `<picture>` element to wrap your images and provide modern image formats to your users. You can also use it for "Art Direction". Although, it's not uncommon to CSS properties leveraged for this now.

Why? Speed up page load times and reduce bandwidth. This provides a better experience for your users. Modern image formats can reduce file size whilst retaining image quality.

Check out this demo across different browsers at different viewport sizes 👍

Check out [this pen](https://codepen.io/jh3y/pen/WNGWgoZ) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

And here's what the code might look like 🤓

    <picture>
      <source
        type="image/avif"
        srcset="lion.avif 400w,
                lion--large.avif 800w"
        sizes="(min-width: 800px) 800px,
               400px"/>
      <source
        type="image/webp"
        srcset="lion.webp 400w,
                lion--large.webp 800w"
        sizes="(min-width: 800px) 800px,
               400px"/>
      <source
        type="image/png"
        srcset="lion.png 400w,
                lion--large.png 800w"
        sizes="(min-width: 800px) 800px,
               400px"/>
      <img src="lion.png" alt=""/>
    </picture>

Browsers adopt different approaches for loading responsive images. You can see this by loading up the demos in this article and resizing the viewport, refreshing, disabling the cache etc. For example, Chrome will always try to load the largest image from cache. Other browsers will respond to viewport resizing and you can see the image changes in realtime. Cache first is a fine approach as you can't resize a mobile device 😅 But, it's an interesting observation for those that like to play with viewport resizing. We see you!

Why?
----

It comes down to two things.

1.  Reducing download sizes for your users
2.  Making things look good across the board

### Reducing Download Size

This might not seem _important_. But consider users that are viewing your site on a mobile device or smaller viewport. Or even those who don't have a great internet connection. A reduced download size results in a quicker rendering time. That means content getting delivered to your audience quicker.

We have different ways to combat this.

1.  Provide smaller sized images to the browser.
2.  Provide images with modern formats that are smaller in file size but also keep quality.

The only issue here is that newer formats such as `avif` and `webp` aren't supported by all browsers. This means falling back to supported formats that might have a larger file size. But, if we're able to tell the browser to grab a smaller sized image when we're on a mobile device, that's going to help.

### Making Things Look Good Across the Board

When your users are on a smaller device, delivering desktop-sized images may not be ideal. This is where the term "Art Direction" comes in. You might show a high detail image on the desktop. But when you're users are viewing on mobile, they can't squint to see the important details. That's where serving a smaller cropped image could make sense.

And we can do this with the `picture` element or via the `srcset` attribute.

![Possible phone mocks comparison](/assets/images/lion-mocks.png)

Possible phone mockup comparison

It's not uncommon to see "Art Direction" handled to an extent with CSS these days. Properties like `object-fit` make this kinda possible. But, we wouldn't be taking advantage of the fact you can serve up a smaller file to your users.

* * *

How?
----

We know what we want to do. But how do we go about doing it?

### `srcset` Attribute

The `srcset` attribute gives us a way to define a set of images for the browser to choose from.

    <img srcset="cat.png 300w,
                 cat--large.png 600w"
         src="awesome.png"/>

We provide a comma-separated list with the format `<URL> <SIZE>w`. The `w` refers to the image's intrinsic width. Use the `src` attribute as a fallback for browsers that don't support `srcset`.

The browser will load whichever image it deems appropriate and for the most part, it will get it right. This relies on your assets having appropriate sizing 👍

Try resizing this [demo](https://codepen.io/jh3y/pen/RwGOMPE) to see it for yourself! (_IMO, the behavior for Firefox is most intuitive_).

Check out [this pen](https://codepen.io/jh3y/pen/RwGOMPE) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

Note how the image stays the same size in the demo. That's because we're using CSS to constrain its size. And that's an important thing to note. It's not uncommon that we size our images with CSS and then forget about it. More on this below.

Resolution
----------

You can also use the `srcset` attribute to define the image that should be should at different screen resolutions. This is often referred to as DPR(Device Pixel Ratio). You can even simulate this in DevTools.

    <img srcset="cat.png,
                 cat--high-res.png 2x"
         src="cat.png"/>

Try it out with this [demo](https://cdpn.io/jh3y/debug/WNGWyRP).

Check out [this pen](https://codepen.io/jh3y/pen/WNGWyRP) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

It comes in useful when you want to serve high-res images for retina screens for example. This was something that was more of an issue with icon sets. But, these days, most are using SVG which scale well on their own.

`sizes` Attribute
-----------------

The `sizes` attribute provides the browser a more informed choice about which image to display.

    <img srcset="cat.png 400w,
                 cat--large.png 800w"
         sizes="(min-width: 800px) 800px,
                400px"
         src="cat.png"/>

Again, a comma-separated list, the browser will choose the size based on a media-condition. It will choose the first size that has a matching media condition. And then the browsers chooses the appropriate image from the `srcset` using that size. This would mean showing `cat.png` at viewports smaller than `800px` in this example. You can use any sizing unit to define a side aside from percentage.

Try resizing your browser window with this demo.

Check out [this pen](https://codepen.io/jh3y/pen/LYRvmxP) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

The `picture` element
---------------------

If you only need to provide different sizes for an image, we can finish up there. But, the magic comes when we bring the `picture` element in. This allows us to define "Art Direction" or provide modern image formats to our users if we have them.

    <picture>
      <source
        type="image/avif"
        srcset="lion.avif 400w,
                lion--large.avif 800w"
        sizes="(min-width: 800px) 800px,
               400px"/>
      <source
        type="image/webp"
        srcset="lion.webp 400w,
                lion--large.webp 800w"
        sizes="(min-width: 800px) 800px,
               400px"/>
      <source
        type="image/png"
        srcset="lion.png 400w,
                lion--large.png 800w"
        sizes="(min-width: 800px) 800px,
               400px"/>
      <img src="lion.png" alt=""/>
    </picture>

There are plenty of image converters out there. You could even use a Node package to generate the images for you. I use [eleventy-img](https://www.11ty.dev/docs/plugins/image/) for [jhey.dev](https://jhey.dev).

What's happening here then? What we have here is like a super-powered `srcset`. We've wrapped our `img` in a `picture` tag to start. But, we've also introduced the `source` element.

### `source` Element

Imagine picture is like a big `srcset`. We work our way down the source elements to find the first one whose conditions pass. The `srcset` and `sizes` attribute of that source element are then used to display our image.

You may have noticed the `type` attribute. And that does exactly what you'd expect. It defines the image type for that source. And this is how the fallback works. In our example, if there is `avif` support, we use the `avif` source. Else, we check for `webp` support and so on. If there is no support for any source, we fallback to the `img`.

![Hovering element to see current source](/assets/images/chrome-avif-hover.png)

Hovering element in DevTools to see current source

You can check which source is being used by checking the `Network` tab in your DevTools. That will show you which format and size gets downloaded.

![Inspecting Network panel for image type](/assets/images/avif-network-panel.png)

Inspecting Network panel for image type

The source tag does support one other attribute which is `media`. As you can guess, this attribute is another condition that source _could_ meet. But, if you use `media`. Don't use media conditions in your sizes too!

* * *

That's it!
----------

That's the info you need to get started with responsive images and dynamic formats. It's important to know about. "Art Direction" is important in some situations. But, knowing these things becomes vital when you start looking into site performance. Being able to get content to your users quicker by not consuming bandwidth is excellent UX.

To put it in numbers, I've seen some image file sizes reduce to a third of the size by using modern formats.

Have a play with the demo and I hope this helps you out! You can visibly see the image quality change if you resize it in Firefox.

Check out [this pen](https://codepen.io/jh3y/pen/WNGWgoZ) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).