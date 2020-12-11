## Optimistic Offline-First Apps With Vuex

_TL;DR — Use_ [_Vuex plugins_](https://vuex.vuejs.org/guide/plugins.html) _together with_ [_localForage_](https://github.com/localForage/localForage) _to easily persist application data in an optimistic manner_

You probably wondering what stands behind these buzzwords, so let’s start explaining them one by one.

#### Optimistic Locking

A concurrency control strategy which assumes conflict will generally not occur, thus first act as if the operation completed and revert in case of a failure. This strategy is quite common these days in the web development domain. You can see it in action when you “Like” on Facebook, when you write a blog post on Medium and actually almost everywhere. Implementation wise, you have to remember to handle the failures as well and how to revert back when the operation fails. User experience wise, this strategy is the best as the user does not have to wait for the request to complete.

#### Offline-First
> We live in a disconnected &amp; battery powered world, but our technology and best practices are a leftover from the always connected &amp; steadily powered past. — [offlinefirst.org](http://offlinefirst.org/)

Offline capability is a must and honestly not so hard to achieve as you will see in this blog post. Basically it means that your application can work even without connectivity at all, of course it does not mean that all features will be provided but a subset. For this purpose we can use Service Workers, Web SQL, IndexedDB and many other tools provided by the browser. I use [localForage](https://github.com/localForage/localForage) as an abstraction for an offline storage instead of working with low level API.

#### Vuex
> A state management pattern + library for Vue.js applications. It serves as a centralized store for all the components in an application — [vuex.vuejs.org](https://vuex.vuejs.org/)

Vuex is very similar to Redux and to many other centralized stores. I will try to keep everything pretty much stack agnostic as the concepts are almost the same in every centralized store. The most important thing for us is the ability to subscribe to mutations happen to the store. This way we can cache the relevant data or sync it back to our server. As long as you can subscribe to mutations any other library will do.Let’s start with the easy part, caching data for offline capability.

First we need a service for handling the cache operations, let’s call it Storage. It should export 3 functions for managing the state, get, set and delete. Everything here is pretty much straight forward the only thing left here is to write a function to map a state object to cache object. Usually you would like to remove some temporary fields or manipulate the state in one way or another.




Now we have to create a plugin which utilizes our Storage service to update the cached state on almost every mutation. Again the implementation is very simple, the only thing left here is to write a function which decides whether the cache should be updated or not based on the mutation (if needed also the state).




Our state is being cached on every mutation but we still have to load it when the application starts. Let’s add a mutation which sets the state from the cache and our plugins (for now please ignore the sync plugin, we will get to it). The `loadFromCache` mutation receives the cached object and updates the state accordingly (in vuex you have to iterate through the properties for reactive reasons). I also like to keep a flag in the state to know whether the state was already initialized from cache.




The only thing left is to call the `loadFromCache` mutation every time the app boots up. You can use it as a Vue Router guard or how ever you want. Below is general concept of implementation.




Our application now persists all the data to a cache storage of sorts, making sure we can access it even when there is no connectivity. Now we need to make sure that the relevant data is sent to the server. The plugin here is full with business logic but I will provide the scheme so you can write your own. The plugin subscribes to the store mutations and based on the mutation type you have to send the respectively request to the server. In case of a failure, the plugin has to commit a mutation to the store to let the application know that the request failed.




That’s it, we made it! Hopefully now you understand what is an optimistic offline-first application and how easily you can achieve it. It also helps improving the user experience as the application become more tolerant to things like connectivity, long request time, etc. In the end your users will have a much better experience, making them to come back and engage more with your app.We have applied this technique and many more while building [Daily Go](https://www.producthunt.com/posts/daily-go/), give it a shot!
