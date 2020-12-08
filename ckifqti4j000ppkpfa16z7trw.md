## How We Built The Next Generation Of Promotional Websites Using Polymer

A month ago we, The Elegant Monkeys, decided that we have to rebuild the company’s website. We all agreed that this website has to be outstanding, not the simple one pager that everyone got. It has to emphasis our creativity and high execution skills. With that thought in mind, we kicked off the project.

Before I start, I encourage you to [visit our website](http://bit.ly/1LtB0qx) to get a better understanding about the topics below.

#### Polymer

I have built lots of websites before (both promotional and webapps) based on angular and jquery. But I knew that this time I have to find a different framework (or a library if you will). A framework based on the latest web concepts and utilizes the newest browser API. A while ago I heard about [Polymer](https://www.polymer-project.org), a web components library with a lot of goodies in it. After a bit of reading, I realized that the perfection we were looking for can be easily achieved with Polymer.

![Polymer](//images.ctfassets.net/de3wzrfouuq1/5ye85SEA85G3xbcHLnusUY/ad94101756faac3555a3166b385273cb/1.png)

#### Preparations

In order to deliver a high-end website as expected, I had to learn a lot more about polymer, browsers and web performance. So while my teammates made the specifications and designed the website, I went through all the videos and posts about web development out there in the wild. I found out that Google advocates are all over the place when it comes to web development and share all their knowledge through personal blogs and youtube series.

Here is a list of a few resources I found:

*   [Polycasts](https://www.youtube.com/playlist?list=PLNYkxOF6rcIDdS7HWIC_BYRunV6MHs5xo) — a youtube series by Rob Dodson (a polymer team member) about core concepts and tips about polymer, of course.
*   [Supercharged](https://www.youtube.com/playlist?list=PLNYkxOF6rcIBz9ACEQRmO9Lw8PW7vn0lr) — a youtube series by Paul Lewis (a chrome developer relations team member) about web performance and tips.
*   [Aerotwist](https://aerotwist.com/) — Paul Lewis personal blog about web performance .
*   [Polymer Blog](https://blog.polymer-project.org/)

There a lot more great resources which are very easy to find using your favorite search engine.
> _Give me six hours to chop down a tree and I will spend the first four sharpening the axe. — Abraham Lincoln_

#### Progressive Web Application

As developers, we always use our high-end development machines and big screens along with the latest chrome to build our websites. But most users don’t care about it, so they use your website with their tiny mobile screens or from internet explorer (god forbid) and they expect to get smooth experience. So you have to decide what is the lowest grade device you want to support and simulate it. After you are done developing your website to this specific device, you can start adapting the website to other screens. chrome devtools can really help us out using the built in device simulator. Adapting to different runtime environments is not just being responsive, it is also enhancing your website with features if they are available. For example, after visiting couple of times our website with chrome it will be cached and be available for offline usage using service worker. We also utilize the new [web app manifest](https://www.w3.org/TR/appmanifest/) which enables the user to add the website to home screen. Service workers, web app manifest, push notifications and theme color are just a few examples among the latest capabilities of browsers nowadays. Polymer makes all of these easy to use because there are already web components for these features. Using the [platinum elements](https://elements.polymer-project.org/browse?package=platinum-elements), a few html lines will enable caching for your website and much more.

![PWA](//images.ctfassets.net/de3wzrfouuq1/bofP7hCTnNJrorcFwbwiy/ae515d2dc44ec1480ac8a08024c132cb/3.png)

“Add To Home Screen” using web app manifest

#### Web Components

This was my first project with web components and after realizing the potential of it I became addicted. A web component may even encapsulate javascript functionality like [making an ajax request](https://elements.polymer-project.org/elements/iron-ajax), or fetching a [firebase collection](https://elements.polymer-project.org/elements/firebase-element). Using polymer you can create data binding between different components thus decreasing significantly the amount of javascript code. It is very easy to create custom web components and use them where ever you want in your website or even in different websites.

#### Performance

Our main objective when talking about performance is to keep our website rendering at 60 frames per second when animating and scrolling. I will share with you the core concepts of web performance but in order to get into details I invite you to follow [Paul Lewis blog](https://aerotwist.com/). The first thing to know is that different properties are more costly than others. Some trigger only the composite process and some cause a full repaint including layout (which can be very intensive process). You can use [css triggers](http://csstriggers.com/) to get more details about each property. As a thumb rule animate only the transform and opacity properties, chrome can handle clip animation as well (this is just a thumb rule, any case should be judged differently). Use chrome devtools to track fps and repaints. When debugging the repaints try to minimize the green rectangle appearances when animating, because it can decrease drastically the fps. Adding _transform: translateZ(0)_ to an animated element or its parent forces the browser to put this element in a separated layer from the others thus not causing them to repaint (it depends on the animated property, of course).

Performance is not just running at 60 fps is also about loading time. In most cases promotional websites are full of pictures which take time to load specially on mobile connectivity. This is why we must use responsive images. A responsive image is basically a collection of the same image in different sizes for different screens in order to minimize the data consumption of the website.

#### The Wow Factor

The aforementioned is nice to have and can really improve the impression about your company. Though it is definitely not enough for a team who pursues perfection. We want to be remarkable and to deliver new user experience. We came up with two wow factors for our website after a long extensive brainstorming. The first is the team section and the second is the analytics section at the bottom, words are not enough, just [try it yourselves](http://bit.ly/1LtB0qx). Every time I show the website to a friend, it makes me smile when I see his face while he scrolls. Come up with ideas for wow factors and implement one of them no matter how hard it is.

![Team](//images.ctfassets.net/de3wzrfouuq1/2uxWaZgDNmqxyYySSfIxhI/a26f25f1f1857c067c6c909a2e69a436/4.png)

The faces follow the mouse

#### SEO &amp; Shareability

The last thing left to do is making your website visible to the outer world. Search engines see our websites differently than humans, they heavily rely on meta tags. This is our responsibility to add those tags to the website to help search engines index the site easily. The first meta tag is the description which will be shown in the search results. The other one is the keyword which is used to set the relevant search phrases for your website.

Another important family of meta tags is the [Open Graph](http://ogp.me/). With these tags you can set how your website link will look when shared on different social networks.

![Facebook Share](//images.ctfassets.net/de3wzrfouuq1/3ShbDtFREKhuE6g1xvmWUW/2321525fd2294728673e542599815570/5.png)

OG meta tags



#### Summary

Throughout this post I tried to give you a glimpse look of all the latest and unknown features in web development so you can build the next generation of promotional websites. I also shared some of my resources so you can follow them yourselves and get better. Our website is a great use case for the topics I covered so [check it out](http://bit.ly/1LtB0qx) if you haven’t done it already.
