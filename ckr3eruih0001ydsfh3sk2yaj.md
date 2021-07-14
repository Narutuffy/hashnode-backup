## WebSocket Simplified


This is the first post of the WebSocket Series I will be writing, and the goal is to explain things in the simplest way possible. Let’s jump right into it.

WebSockets allows a user to send and receive messages to a server with a persistent, bi-directional connection. So basically, this is a way of communication between **Client** and **Server**. Let’s understand this **communication** first, we will return to WebSockets in a while.

## Client and Server

Web browsers (client) and servers communicate via TCP/IP. Hypertext Transfer Protocol (HTTP) is the standard application protocol *on top of* TCP/IP supporting requests (from the web browser) and their responses (from the server).

## How does this work?

Let’s go through these simple steps:-

1. The client sends a request to the server.

1. A connection is established.

1. The server sends back the response.

1. The client receives the response.

1. The connection is closed.

This is a simple overview of how standard communication between the client and server works. Now get a closer look at step no. 5.
> *Connection is closed.*

The HTTP request has served its purpose and it is no longer needed, hence the connection is closed.

### What if the server wants to send a message to Client

In our standard request/response scenario, the connection must be established from the request to start communicating. If the server wants to send a message, the client will have to send another request to establish a connection and receive the message.

### How will the client know that the server wants to send a message?

Consider this example:
*The client is starving and has ordered some food online. He is making one request per second to check if the order is ready.*
> *0 sec: Is the food ready? (Client)
0 sec: No, wait. (Server)
1 sec: Is the food ready? (Client)
1 sec: No, wait. (Server)
2 sec: Is the food ready? (Client)
2 sec: No, wait. (Server)
3 sec: Is the food ready? (Client)
3 sec: Yes sir, here is your order. (Server)*

This is what you call **HTTP Polling**. The client makes repeated requests to the server and checks if there is any message to receive.

As you can see, this not very efficient. We are using unnecessary resources and the number of failed requests is also troublesome.

### Is there any way to overcome this issue

Yes, there is a variation of polling technique that is used to overcome this deficiency and it is called as ***Long-Polling***.
> *Long Polling basically involves making an HTTP request to a server and then holding the connection open to allow the server to respond at a later time (as determined by the server).*

Consider the **Long-Polling** version of the above example:-
> *0 sec: Is the food ready? (Client)
3 sec: Yes sir, here is your order. (Server)*

Yay, problem solved.
Not exactly. Although Long Polling works, it is very expensive in terms of CPU, memory, and bandwidth (*as we are blocking resources in holding the connection open*).

What do we do now? It looks like things are getting out of hand. Let’s reach back to our savior: **WebSocket**.

## Why WebSockets

As you can see, Polling and Long-Polling are both quite expensive options in order to emulate real-time communication between a client and server. This performance bottleneck is the reason why you would want to use WebSocket instead.

WebSockets don’t need you to send a request in order to respond. They allow *bi-directional* data flow so you just have to listen for any data.
> *You can just listen to the server and it will send you a message when it’s available.*

Let’s look at the performance side of the WebSocket.

## Resource Consumption

The chart below shows the bandwidth consumption differences between WebSockets vs Long Polling in a relatively common use case:

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626262598629/rgZ1sWY9h.png)

The difference is huge and grows exponentially with the number of requests.

## Speed

Here are the results for 1, 10 and 50 requests served per connection in one second:

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626262601473/Q0AY8Fd9Zy.png)

As you can see, making a single request per connection is about 50% slower using [Socket.io](https://hashnode.com/util/redirect?url=http://Socket.io) since the connection has to be established first. This overhead is smaller but still noticeable for ten requests. At 50 requests from the same connection, [Socket.io](https://hashnode.com/util/redirect?url=http://Socket.io) is already 50% faster. To get a better idea of the peak throughput we will look at the benchmark with a more extensive number (500, 1000 and 2000) of requests per connection:

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626262604259/lDfXmh0wR.png)

Here you can see that the HTTP benchmark peaks at about~950 requests per second while [Socket.io](https://hashnode.com/util/redirect?url=http://Socket.io) serves about ~3900 requests per second. Effective, right?
> *Note: [Socket.io](https://hashnode.com/util/redirect?url=http://Socket.io) is a JavaScript library for real-time web applications. It implements WebSocket internally. Consider this as a wrapper for WebSocket which provides many more features *(Next Blog post of this series explains [Socket.io](https://hashnode.com/util/redirect?url=http://Socket.io) in detail)*.*

## How do WebSockets work

These are the steps involved in establishing a WebSocket connection.

1. The client (browser) sends an HTTP request to the Server.

1. A connection is established via the HTTP protocol.

1. If the server supports the WebSocket protocol, it agrees to upgrade the connection. *This is called a handshake.*

1. Now that the handshake is complete the initial HTTP connection is replaced by a WebSocket connection that uses the same underlying TCP/IP protocol.

1. At this point, data can flow back and forth freely between Client and Server.

We are going to create two files: one server and one client.
First, create a simple `&lt;html&gt;` document named as `client.html` containing a `&lt;script&gt;` tag. Let's see how it looks:-

## Client.html

```
<html>

<script>
    // Our code goes here
</script>

<body>
    <h1>This is a client page</h1>
</body>

</html>
```


## Server.js

Now create another file `server.js`. Import the HTTP module and create a server. Make it listen to `port 8000`.

This will work as a simple `http` server listening to `port 8000`. Let's look at that too:

```
//importing http module
const http = require('http');

//creating a http server
const server = http.createServer((req, res) => {
    res.end("I am connected");
});

//making it listen to port 8000
server.listen(8000);
```

> *Run command `node server.js` to start the listening to `port 8000`. *You can choose any port as you like, I just chose 8000 for no specific reason.

Our basic setup of the client and server is done now. That was simple, right? Let’s get to the good stuff now.

## Client setup

To construct a **WebSocket**, use the `WebSocket()` constructor which returns the `websocket` object. This object provides the API for creating and managing a WebSocket connection to the **Server**.

In simple words, this `websocket` object will help us establish a connection with the server and create a bi-directional data flow i.e. *send and receive data from both ends*.

Let us see how:

```
<html>

<script>
    //calling the constructor which gives us the websocket object: ws
    let ws = new WebSocket('url'); 
</script>

<body>
    <h1>This is a client page</h1>
</body>

</html>
```


The `WebSocket` constructor expects a URL to listen to. Which in our case, is `'ws://localhost:8000'` because that's where our server is running.
Now, this is might be a little different from what you are used to. We are not using the `HTTP` protocol, we are using `WebSocket` protocol. This will tell the client that **'Hey, we are using websocket protocol'** hence `'ws://'` instead of `'http://'`. Simple enough? Now let's actually create a WebSocket server in `server.js`.

## Server setup

We are going to need a third-party module `ws` in our node server to use and setup the `WebSocket` server.

First, we will import the `ws` module. Then we will create a websocket server and hand it the `HTTP` server listening to `port 8000`.
> *HTTP server is listening to port 8000 and WebSocket server is listening to this HTTP server. Basically, it is listening to the listener.*

Now our websocket is watching for traffic on `port 8000`. It means that it will try to establish the connection as soon as the client is available. Our `server.js` file will look like this:

```
const http = require('http');
//importing ws module
const websocket = require('ws');

const server = http.createServer((req, res) => {
    res.end("I am connected");
});
//creating websocket server
const wss = new websocket.Server({ server });

server.listen(8000);
```


As we have discussed before — the `WebSocket()` constructor returns a websocket object provides the API for creating and managing a WebSocket connection to the **Server**.

Here, the `wss` object will help us listen to the `Event` emitted when certain things happen. Like the connection is established or the handshake is complete or the connection is closed, etc.

Let's see how to listen to the messages:

```
const http = require('http');
const websocket = require('ws');

const server = http.createServer((req, res) => {
    res.end("I am connected");
});
const wss = new websocket.Server({ server });
//calling a method 'on' which is available on websocket object
wss.on('headers', (headers, req) => {
    //logging the header
    console.log(headers);
});

server.listen(8000);
```


The method `'on'` expects two arguments: Event name and callback. The event name is what we want to listen/emit and the callback specifies what to do with it. Here, we are just logging the `headers` event. Let's see what we get:

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626262606393/IiIIXbO1o.png)

This is our HTTP header and I want you to be inquisitive about it because this is exactly what’s going on behind the scenes. Let’s break it down to understand it better.

* The first thing you will notice is that we got the status code `101`. You may have seen `200`, `201`, `404` status code but this looks different. `101` is actually the Switching Protocols status code. It says **"Hey, I wanna upgrade"**.

* The second line shows the Upgrade information. It specifies that it wants to upgrade to `websocket` protocol.

* This is actually what happens during the handshake. The browser uses the `HTTP` connection to establish the connection using `HTTP/1.1` protocol and then it `Upgrade` it to `websocket` protocol.

Now this will make more sense.

The `Headers` event is emitted before the response headers are written to the socket as part of the handshake. This allows you to inspect/modify the headers before they are sent. This means you can modify the headers to accept, reject, or anything else as you like. By default, it accepts the request.

Similarly, we can add one more event `connection` which is emitted when the handshake is complete. We will send a message to the client upon successfully establishing a connection. Let's see how:

```
const http = require('http');
const websocket = require('ws');

const server = http.createServer((req, res) => {
    res.end("I am connected");
});
const wss = new websocket.Server({ server });

wss.on('headers', (headers, req) => {
    //console.log(headers); Not logging the header anymore
});

//Event: 'connection'
wss.on('connection', (ws, req) => {
    ws.send('This is a message from server, connection is established');
    //receive the message from client on Event: 'message'
    ws.on('message', (msg) => {
        console.log(msg);
    });
});

server.listen(8000);
```


We are also listening to the event `message` coming from the client. Let's create that:-

```
<html>

<script>
    let ws = new WebSocket('url'); 
    //logging the websocket property properties
    console.log(ws);
    //sending a message when connection opens
    ws.onopen = (event) => ws.send("This is a message from client");
    //receiving the message from server
    ws.onmessage = (message) => console.log(message);
</script>

<body>
    <h1>This is a client page</h1>
</body>

</html>
```


This is how it looks in the browser:-

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626262609333/KWSvYdUVS.png)

The first log is `WebSocket` listing all the properties on the websocket object and the second log is `MessageEvent` which has a `data` property. If you look closely, you will see that we got our message from the server.

The server log will look something like this:

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1626262612626/epVQRYxcf.png)

We received the client’s message correctly. This marks that our connection was established successfully. Cheers!

## Conclusion

To sum up, let’s go through what we learned:

* We have covered how HTTP server works, what is Polling, Long Polling.

* What are WebSockets and why we need them.

* We covered how they work behind the scene and got a better understanding of the headers used.

* We created our own client and server and successfully established the connection between them.

This is the basics of WebSockets and how they work. The next post in the series will cover `socket.io` and using it in more detail. We will also see why exactly we need `socket.io` when things are working just fine with the only native `WebSocket()`. Why use a heavy bloated library when we can successfully send and receive messages just fine?

Do share the post if you find it helpful and stay tuned for the next one.
Shad.

## Reference

* WebSocket — Web APIs | MDN: [https://developer.mozilla.org/en-US/docs/Web/API/WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)

* `ws` module for Node server | Docs: [https://github.com/websockets/ws/blob/HEAD/doc/ws.md#event-headers](https://github.com/websockets/ws/blob/HEAD/doc/ws.md#event-headers)

*Originally published at [https://iamshadmirza.hashnode.dev](https://iamshadmirza.hashnode.dev/websocket-simplified-cjxjzcu0m002i3hs1eewt2p80).*