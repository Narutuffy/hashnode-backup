## Executing functions in node.js from CLI

Sometimes you just want to execute a particular function in an existing node.js backend, instead of making whole network requests to run that function you can run it using your CLI.

Let's say you have a file `hello.controller.js`
```
const byeController = require("./bye.controller");
exports.hello = () => {
    console.log("Hello There");
    byeController.bye(); 
}
``` 

```
//bye.controller.js
exports.bye = () => {
    console.log("See you again!");
}
```

you can run this function from your CLI by executing


```
node -e 'require("./controllers/hello.controller").hello()';
``` 

Output:
![Screenshot 2020-09-02 at 11.58.00 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1599028087764/v8zOubZZo.png)


PS: Functions involving DB operations cannot run using this. For that, you have to hit the endpoint.

<hr/>


>This post is part of the series [Bytes](https://hashnode.com/series/bytes-ckckqh5km0000mks1fghg1rc5).
 Feel free to share some nifty tricks you use in your day-to-day in the comments. You can also @ me on [twitter](https://twitter.com/vamsirao7)