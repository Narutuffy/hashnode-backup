## Rate my SVG art! [Compared to CSS art]

Loads of people like to do CSS art. It is really impressive but next to pointless for anything other than a cool demonstration. 

Why don't we see what you should **actually** use for art...SVG

Inspired by this post where @afif once again blows my mind with his CSS ninja skills!

{% post https://dev.to/afif/rate-my-first-css-drawing-51m5 %}

As a first CSS drawing...my rating is a 10!

But what if a zero skilled noob like myself wanted to create an image? Well I would use SVG of course!

## Let's compare the results

let's see the three items side by side:

### Original image
![Livai Ackerman](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1632744508512/RH7CAbwzk.jpeg)

### CSS version
{% codepen https://codepen.io/t_afif/pen/MWoXXRd default-tab=result, css, html %}

### SVG version
{% jsfiddle https://jsfiddle.net/n3cepyrx/ result html %}

<center>**Yes the third image is an SVG, I haven't just included a base64 encoded image or anything, you should check out the source code!**</center>
<p>&nbsp;</p>
<p>&nbsp;</p>
---



## Comparison of effort etc.
Let's compare how long they took etc.

### Time to complete
**CSS version:** 6 hours ❌
**SVG version:** <30 minutes (would take longer without reference image to copy!) ✅

### File Size
**CSS Version:** 3kb gzipped ✅
**SVG version:** 16kb gzipped (but can be made a lot smaller at the expense of accuracy) ❌

### Accuracy
**CSS Version:** Good ❌
**SVG Version:** Extremely Good ✅

### Scalability
**CSS Version:** Will scale almost infinitely ✅
**SVG Version:** Will scale almost infinitely  ✅
(A draw! They are both effectively vector images)


### Compatibility

**Note:** this point is actually not entirely fair so should be disregarded. Most CSS art will not have issues with compatibility and as @afif pointed out the issues were more to do with `aspect-ratio` than the main CSS. I have left it in so the comments make sense!

**CSS Version:** Doesn't work well in Safari (the new IE!) ❌
**SVG Version:** works all the way back to IE9! ✅

### Flexibility
**CSS Version:** Works well on a web page (assuming all CSS properties are supported). ❌
**SVG Version:** Works on a web page, but can also be sent for professional printing. As an added bonus you can include it as an image on a web page (or you can inline it to reduce the need for a network request). You can even export it as a JPEG or PNG etc. for image sharing sites. ✅

### Skill Level Required
**CSS Version:** Simply blows my mind ✅
**SVG Version:** Meh, SVG is pretty easy! ❌



## The winner?
SVG without a doubt, even the one item it didn't win (file size) was due to the massive complexity / detail increase.

Obviously it isn't as impressive though and I suppose if you want to show off you should use CSS, for everything else there is ~~mastercard~~ SVG!

## So which should you use?
If you enjoy making CSS art it is far more impressive than SVG and is a great way to learn some advanced CSS tricks.

But for production, in the real world, where time is important, SVG wins hands down.

It will also just work on any browser, no worrying about supported properties or anything like that!

So while I love CSS art as it is always amazing to see how people make things work, SVG creation is probably the tool you want in your arsenal when designing things!

## What do you think?
Let me know what do you think 👇👇

✔️ SVG is amazing, I am going to go learn it!
❌ I won't use SVG 🤮, I want overly complex CSS imagery and lots of headaches instead!

p.s. I think I need to add here that I am baiting people when I say CSS art is useless, it isn't, I am just being my normal mischievous self 🤣

## Update
As everyone loves CSS art - I made CSS art, check that out too!

{% post https://dev.to/inhuofficial/fine-you-want-css-art-you-got-it-my-first-ever-css-art-2ka7 %}











