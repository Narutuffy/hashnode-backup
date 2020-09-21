## Make Your Logged JSON Readable

I am tired of logging JSON and seeing `[Object]` or `[Array]` on my terminal.
![Screenshot 2020-09-15 at 4.46.12 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1600168720811/oul9aQXsK.png)

So I set out on a mission to do something about it. Enter [jq](https://stedolan.github.io/jq), a lightweight and flexible command-line JSON processor. You can follow the installation guide in the [docs](https://stedolan.github.io/jq/download/) for your OS.

 Now all you have to do is `console.log(JSON.stringify(data))` and pipe the command you run to jq. Like shown below:

```
node index.js | jq
```
Which will output this beautiful JSON ✨ that we wanted in the first place.
![Screenshot 2020-09-15 at 5.00.08 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1600169388616/YhMR4ydoc.png)

Sometimes the output isn't always just stringified JSON, there can also be other logs and errors in some cases. For jq to function properly it needs to ignore those logs. You can ignore it by using

```
 node index.js | jq -R 'fromjson? | select(type == "object")'
```

<hr/>

>Hope you found the article useful. Let me know your thoughts in the comments below or you can @ me on [twitter](https://twitter.com/@VamsiRao7)