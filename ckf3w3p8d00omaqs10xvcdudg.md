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
