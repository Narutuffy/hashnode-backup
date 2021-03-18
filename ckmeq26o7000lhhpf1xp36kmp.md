## How to Get the Height of the Entire Document with JavaScript?

Sometimes, we may want to get the height of the entire document with JavaScript.

In this article, we’ll look at how to get the height of the entire document with JavaScript.

### Getting the Height of a Document

To get the height of a document, we can get the max of the `scrollHeight`, `offsetHeight`, or `clientHeight` properties.

For instance, we can write:

    const body = document.body;
    const html = document.documentElement;
    
    const height = Math.max(body.scrollHeight, body.offsetHeight,
      html.clientHeight, html.scrollHeight, html.offsetHeight);
    console.log(height)
    

The document can be stored in the `document.body` or `document.documentElement` properties depending on the browser used.

`scrollHeight` is a read-only property is a measurement of the height of an element’s content including the area that’s not visible on the page.

`offsetHeight` is a read-only property that returns the height of the element, including the vertical padding and borders as an integer.

`clientHeight` is a read-only property is the inner height of an element in pixels.

It includes the padding but excludes borders, margins, and horizontal scrollbars if they’re present.

Therefore, the max value between those would be the document’s height which includes everything.

### The getBoundingClientRect Method

We can also use the `getBoundClientRect` method on the document element to get the height of it.

To use it, we write:

    const body = document.body;
    const html = document.documentElement;
    
    const height = Math.max(body.getBoundingClientRect().height, html.getBoundingClientRect().height);
    console.log(height)
    

We get the height of content of the document with the `getBoundingClientRect` method.

It returns an object with the `height` property to get the height of the content of the document.

The height is in pixels.

### Conclusion

There’re various properties we can use to get the height of a document with JavaScript.

The post [How to Get the Height of the Entire Document with JavaScript?](https://thewebdev.info/2021/03/16/how-to-get-the-height-of-the-entire-document-with-javascript/) appeared first on [The Web Dev](https://thewebdev.info).