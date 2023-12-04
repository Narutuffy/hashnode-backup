---
title: "Faster Debugging with Chrome Dev Tools"
datePublished: Wed Jul 15 2020 07:22:38 GMT+0000 (Coordinated Universal Time)
cuid: ckcn1epp30017k5s14xgxbhf0
slug: faster-debugging-with-chrome-dev-tools
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1594797600513/xGqEqQYvp.jpeg
tags: programming, chrome, debugging, 2articles1week

---

A lot of times you want to look at request objects and response objects of an API a bit more extensively and with a lot more freedom than what your browser allows. For this, I use this neat shortcut to copy the JSON to my editor.


![Screenshot 2020-07-15 at 12.31.06 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1594796636954/Fc2L_tzr7.png)

Go to the Network tab -> right click on the Response object -> select **Store as global variable**

Chrome will save this as a variable `temp1`, which you can access from the console.


![Screenshot 2020-07-15 at 12.31.48 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1594796795321/mhxMaXfaV.png)

Now, run the command `copy(temp1)`. This will copy the whole object to your clipboard.

PS: You can use "Store as global variable" for the logged objects in your console too.

<hr/>


>This post is part of the series [Bytes](https://hashnode.com/series/bytes-ckckqh5km0000mks1fghg1rc5).
 Feel free to share some nifty tricks you use in your day-to-day in the comments. You can also @ me on [twitter](https://twitter.com/vamsirao7)