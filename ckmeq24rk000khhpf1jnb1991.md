## How to Delete a Client-Side Cookie with JavaScript?

Sometimes, we may store cookies on the client-side to keep track of things on the user’s device.

And sometimes, we want to delete cookies from the user’s browser.

In this article, we’ll look at how to delete a client-side cookie with JavaScript.

### Create Our Own Function

Browsers don’t comes with any API to let us delete cookies easily.

Therefore, we’ve to create our own function to delete cookies from a user’s browser.

To do this, we write:

    const getCookie = (name) => {
      return document.cookie.split(';').some(c => {
        return c.trim().startsWith(name + '=');
      });
    }
    
    const deleteCookie = (name, path, domain) => {
      if (getCookie(name)) {
        document.cookie = name + "=" +
          ((path) ? ";path=" + path : "") +
          ((domain) ? ";domain=" + domain : "") +
          ";expires=Thu, 01 Jan 1970 00:00:01 GMT";
      }
    }
    
    document.cookie = 'foo=bar'
    deleteCookie('foo')
    

We create the `getCookie` function to check for a cookie with the given `name` value.

We do that by splitting the `document.cookie` string by the semicolon.

Then we call `startsWith` on the split string with the `name` and equal sign to check if there’s a cookie with the given key.

Next, we define the `deleteCookie` function that checks if the cookie with the given key exists with `getCookie` .

If it does, then we add a `expires` date and time to the end of the string by appending:

    ";expires=Thu, 01 Jan 1970 00:00:01 GMT"
    

to it.

The way to delete a cookie is to set its expiry date to a date and time before the current date and time.

Therefore, when we set the cookie in the 2nd last line and call `deleteCookie` with the same key right after, we won’t see the cookie being set since it’s added and removed immediately after.

### Conclusion

We can delete a client-side cookie from the user’s browser by setting the expiry date of the cookie with the given key to a date and time before the current date and time.

The post [How to Delete a Client-Side Cookie with JavaScript?](https://thewebdev.info/2021/03/16/how-to-delete-a-client-side-cookie-with-javascript/) appeared first on [The Web Dev](https://thewebdev.info).