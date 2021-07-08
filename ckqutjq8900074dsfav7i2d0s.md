## Make Your Logged JSON Readable


I am tired of logging JSON and seeing `[Object]` or `[Array]` on my terminal.

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1625743224424/EzjXN85gL.png)

So I set out on a mission to do something about it. Enter [jq](https://stedolan.github.io/jq), a lightweight and flexible command-line JSON processor. You can follow the installation guide in the [docs](https://stedolan.github.io/jq/download/) for your OS.

Now all you have to do is `console.log(JSON.stringify(data))` and pipe the command you run to jq. Like shown below:

```
node index.js | jq
```


Which will output this beautiful JSON ✨ that we wanted in the first place.

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1625743228297/8H28hy_4K.png)
> *Hope you found the article useful. Let me know your thoughts in the comments below or you can @ me on [twitter](https://twitter.com/@VamsiRao7)*