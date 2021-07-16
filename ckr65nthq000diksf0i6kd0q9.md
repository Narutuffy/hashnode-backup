## Socket.IO — What, Why and How?


This is the second blog post in continuation of the series **WebSockets Simplified**. If you haven’t read the first part, check it out [here](https://levelup.gitconnected.com/websocket-simplified-b532f266cc9f). It is essential that you go through the first part to understand this one better. Again, the goal is to keep things as simple as possible. So I’m going to approach this topic a little differently. Let’s start.

In the previous post, we successfully built our basic WebSocket server and client, and it is functional. So you might be wondering why to use socket.io? Why use a heavy bloated library when things are working just fine without it? Let’s consider this scenario:

Suppose your boss tells you to add real-time communication to the website, and you decided to use native WebSocket and not any library whatsoever.
You coded the WebSockets correctly and it’s mostly working fine *EXCEPT* the testing reveals that there are some issues.

After some debugging, you found that it’s not working when the person is behind a *firewall* or an *antivirus.*
> *WebSocket doesn’t work ideally in presence of Firewall and Antivirus.*

You also found that if the client is behind a proxy or load balancer, then also WebSocket will not work.
> *WebSocket doesn’t work ideally in presence of Proxy and Load Balancer either.*

It means you have to deal with all these issues yourself. You worked for a few hours and added extra code to solve all these problems and your WebSocket connection is finally working properly. See the visual representation of what we did so far:

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626428662998/PbDGtRF__.jpeg)

Now suppose the server goes down for some reason, and the connection got disconnected. In this situation, the browser is now sitting ideally not doing anything because the handshaking is again necessary to establish the connection. So you have one more thing to do now:
> *Websockets should reconnect automatically in case of disconnection due to server failure or some other reason.*

So now you added more code and tackled this problem too. Pretty good, you’re a very good developer.

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626428668883/_1Ql5adPG.gif)

Your boss appreciated your efforts, but now he tells you to add support for images (blobs) too.
> *Note: We were sending JSON data so far (plain text).*

Not only this, you have to add chatroom for group chats, multiple endpoints so that you can connect multiple WebSockets. Also, there might be some old browsers which just doesn’t support WebSockets so you have to take care of that too.

This seems like a lot now, doesn’t it?

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626428679966/iOyn4Gvh_.gif)

With some more code and hard work, you tackled these too. Now your WebSocket implementation looks something like this:

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626428683848/QXsDWWyMN.jpeg)

Guess what, this whole collection is actually what you call [Socket.IO](https://hashnode.com/util/redirect?url=http://Socket.IO). Check out the image below just to make things crystal clear.

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626428687169/mL5x1IkVo.jpeg)

*Socket.io* uses WebSockets behind the scenes and takes care of all these problems so that we don’t have to deal with them ourselves. It’s just a wrapper over native WebSocket which provides a bunch of other cool features which makes our job easy as a developer.

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626428691507/N-i74HVzl.gif)

## An overview of socket.io

Socket.IO is a JavaScript library for realtime web applications. It enables realtime, bi-directional communication between web clients and servers.

Socket.IO primarily uses the WebSocket protocol with ***polling as a fallback option***, while providing the same interface. Although it can be used as ***simply a wrapper for WebSockets***, it ***provides many more features***, including broadcasting to multiple sockets, storing data associated with each client, and asynchronous I/O.
> *Note: Read that again and give extra attention to the text in bold. We’ve been talking just that in the Why section. Now it makes sense right?*

We’ll learn about the core features of Socket.IO in the third part of this series. Let’s convert our WebSocket client and server files (created in the[ first part](https://levelup.gitconnected.com/websocket-simplified-b532f266cc9f)) to use Socket.IO quickly.

## How?

I’m assuming you’ve read the [first part](https://levelup.gitconnected.com/websocket-simplified-b532f266cc9f) and know the working of native WebSockets. Let’s quickly upgrade that to Socket.io.
> *Commented code is WebSocket usage. I’ve added it for comparison.*

## Server

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626428696232/wnrw0Utt6.png)

**Explanation**

1. Instead of importing `websocket from ‘ws’`, we’re importing `socketio from ‘socket.io’` library.

1. Creating a server and handing it to `socketio` instead of `websocket`.

1. Instead of `send`, we’re now using `emit` function on `io` object. The only difference is we have added *event name* as the first argument. We’ll use this *event name* to retrieve the message at the client’s end.

1. Method ‘on’ still works the same, the only difference is the addition of *event name*. Simple enough?

## Client

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626428700434/6dP0PmGMA.png)

**Explanation**

1. Importing ‘socket.io’ library as it is not provided by JavaScript out of the box. This will expose ‘io’ namespace.

1. Providing URL for io to connect. (We are not using new keyboard unlike WebSocket usage because it is optional. It’s up to you if you wanna use it or not.)

1. Again ‘send’ is replaced by ‘emit’ and we have added event name (check line 21 of Server part).

1. Method ‘on’ is still the same with the addition of *event name* (check line 20 of the Server part). Makes sense?

Cheers! We have created our simple Socket.IO Server and Client.

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626428705666/8B-X6B9GI.gif)

I’ve tried to keep things as simple as possible. We’ll dig deeper into [Socket.IO](https://hashnode.com/util/redirect?url=http://Socket.IO) in the third part of this series. Let me know if this was helpful.
Shad

## Reference

* Socket.IO Server API — [socket.io/docs/server-api](https://hashnode.com/util/redirect?url=https://socket.io/docs/server-api/)

* Socket.IO Client API — [socket.io/docs/client-api](https://hashnode.com/util/redirect?url=https://socket.io/docs/client-api/)

*Originally published at [https://iamshadmirza.hashnode.dev](https://iamshadmirza.hashnode.dev/socketio-what-why-and-how-cjyb5k1k6000cbes17g4yasd2).*