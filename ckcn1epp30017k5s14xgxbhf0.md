## Faster Debugging with Chrome Dev Tools

A lot of times you want to look at request objects and response objects of an API a bit more extensively and with a lot more freedom than what your browser allows. For this, I use this neat shortcut to copy the JSON to my editor.


![Screenshot 2020-07-15 at 12.31.06 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1594796636954/Fc2L_tzr7.png)

Go to the Network tab -> right click on the Response object -> select **Store as global variable**

Chrome will save this as a variable `temp1`, which you can access from the console.


![Screenshot 2020-07-15 at 12.31.48 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1594796795321/mhxMaXfaV.png)

Now, run the command `copy(temp1)`. This will copy the whole object to your clipboard.

PS: You can use "Store as global variable" for the logged objects in your console too.
