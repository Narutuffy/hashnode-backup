## What is Cache and Why do Devs Have a Love-Hate Relationship With It?


Let me set the context with this quote
> *There are only two hard things in Computer Science: cache invalidation and naming things.*
> *~ Phil Karlton*

Before we start talking about Cache Invalidation, let’s see what cache actually is. Wikipedia says cache (pronounced as “Cash”) is simply a component that saves data so that future requests for that data can be served faster.

In this article, we’ll talk about three types of cache that are most commonly used

* Browser Cache

* Content Delivery Networks (CDN)

* Database Caching

### Browser Cache

Whenever you visit a website for the first time, the browser stores web page resources (like html, css, js, image, etc) locally for a snappier experience and lesser bandwidth consumption for your next visit. So the next visit, most of the assets of the website will be served from browser cache. Below is what goes on when your browser requests for `foobar.com`

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626429985431/DXy5gvJkJ.png)

### Browser Cache Invalidation

Cache Invalidation is also called `cache-busting` and `purging cache`. All it means is to clear the cache. It is done so that the latest resources are fetched and shown to the user.

Suppose you had already visited `foobar.com/contact-us.html` and this is cached in your browser. The creators of foobar have recently updated their contact info, to purge the cache they can do the following:

* Change the resource URI from `foobar.com/contact-us.html` to `foobar.com/contact-us2.html` (least preferred way)

* Change the version parameter in the query string which will look like `foobar.com/contact-us.html?v=2`

* By changing the value of `cache-control` in the header (You can learn more about it here [cache-control](https://www.cloudflare.com/learning/cdn/glossary/what-is-cache-control/))

* Ask visitors to hard reload their site ( Mac: `cmd + shift + r`, Windows: `ctrl + shift + r` )

### Content Delivery Networks (CDN)

A CDN can do more than caching, it stores data in geographically distributed locations so that round-trip times to and from a geographically local browser are reduced. So your browser fetches data from the nearest CDN located.

CDN also follows the same rules as your browser but just becomes another intermediary player. If the cache hasn’t expired yet, the first request from the browser in that time window will reach the server then the subsequent requests are served from the CDN itself.

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626429989959/VIT52UzmA.jpeg)

This type of caching not only helps for faster response time but also reduces the load on your server.

### CDN Cache Invalidation

Different providers of CDN have different ways to invalidate the cache. You can describe how the cache should behave in your response headers from the server. Most of them also provide their own APIs and an option to purge the cache from their dashboard.

### Database Caching

In the previous section, we discussed about CDN and how it was the intermediary player between client and server, similarly, a database caching system is an intermediary player between server and database. There are many database caching systems like [redis](https://redis.io/), [memcache](https://memcached.org/) etc. The working of it is explained below

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626429996113/KqE6VanLl.jpeg)

### Database Cache Invalidation

Each of these systems provide their own methods to invalidate the cache. Refer to their docs to learn more.

Now coming to the question why do new developers struggle with it ? Generally, the modern stacks utilize the combination of these three caches. What devs sometimes stuck are at debugging. Imagine you have made some changes to your website but it is not reflecting the latest changes, assuming there is nothing wrong with the code, the culprit could be any of these three. That’s relatively a small downside (not even a downside if you are aware of it) compared to the huge upside it gives with scalability, less response time, and overall a better user experience.
> *Hope you enjoyed the article. Let me know your thoughts in the comments below or you can @ me on [twitter](https://twitter.com/vamsirao7)*

*Originally published at [https://dev.vamsirao.com](https://dev.vamsirao.com/what-is-cache-and-common-ways-of-using-it).*