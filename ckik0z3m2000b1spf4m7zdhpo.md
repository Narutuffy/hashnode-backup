## Keep Your Website in Ratio (Polymer Inside)

One of the things I hate the most while wandering in the web is to tackle into websites with annoying content flow. You can find yourself reading a text and in a matter of seconds it is already gone because an image has just loaded and moved all the content way down. To keep our users satisfied, we have to deliver the best user experience and I want to share with you a quick tip that will take you one step closer to it.Sometimes when loading content asynchronously you know beforehand the size of the content or better say the aspect ratio (e.g. images, embedded content), so you can put this content inside a container (a simple `div`) and set its size accordingly. This way the container will already hold the place for its child and prevent content from flowing all over the place. Most of the times you would like to set the width in percentage units, so it might be tricky to set the content aspect ratio with pure css. To solve this, let me tell you something about browsers (it might sound counterintuitive at first but wait for it…): padding &amp; margin (both vertical and horizontal) are being **calculated by the width** of the element only! Which means that if we set the following css to an element `width: 100px; padding-top: 20%`. It will result in `padding-top` equals to `20px`. Now that we know how browsers work, we can use this for our own good. Set the `padding-top` of the container’s `:before` pseudo element according to the aspect ratio (e.g for 16:9 content set `padding-top: 56.25%`) and set the child `position` to `absolute `along with `top: 0; left: 0; width: 100%; height: 100%`. This way we get perfectly sized content which will not cause content flowing even when loading asynchronously.

Here is a link to a codepen with demo of this technique [https://codepen.io/idoshamun/pen/PGxNKr](https://codepen.io/idoshamun/pen/PGxNKr)

![Polymer](//images.ctfassets.net/de3wzrfouuq1/5ye85SEA85G3xbcHLnusUY/ad94101756faac3555a3166b385273cb/1.png)


If you are a Polymer lover like myself, you will have easy time implementing this into your project using our `tem-container` web component.

Check out the [GitHub page](https://github.com/elegantmonkeys/tem-container) and [the demo!](https://elegantmonkeys.github.io/PolymerElements/).
