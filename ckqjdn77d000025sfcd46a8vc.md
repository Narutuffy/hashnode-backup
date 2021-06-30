## How to Detect Scroll End with JavaScript?

Sometimes, we want to detect scroll end with JavaScript.

In this article, we’ll look at how to detect scroll end with JavaScript.

### Detect Scroll End with JavaScript

To detect scroll end with JavaScript, we can listen to the `scroll` event.

Then in the event listener, we check if `offsetHeight + scrollTop` is bigger than or equal to `scrollHeight` .

For instance, if we have the following div:

    <div style='overflow-y: auto; height: 100px'>  
      Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed dapibus aliquam iaculis. Pellentesque interdum elit sapien, quis interdum enim laoreet sed. Mauris varius magna ac dapibus molestie. Sed porttitor sapien eget ipsum aliquet, lacinia venenatis lacus finibus. Phasellus in nibh mauris. Interdum et malesuada fames ac ante ipsum primis in faucibus. Sed placerat tristique augue, id lacinia massa iaculis eu. Donec sed vestibulum odio. Fusce sit amet congue odio, eu consequat neque. Sed sed mauris id sem malesuada blandit eu at quam.  
    </div>
    

Then we can check fi we scrolled to the bottom of the div by writing:

    const myDiv = document.querySelector('div')  
    myDiv.addEventListener('scroll', () => {  
      if (myDiv.offsetHeight + myDiv.scrollTop >= myDiv.scrollHeight) {  
        console.log('scrolled to bottom')  
      }  
    })
    

We get the div with `document.querySelector` .

Then we call `addEventListener` with `'scroll'` to add a scroll event listener to it.

Then in the event listener function, we have:

    myDiv.offsetHeight + myDiv.scrollTop >= myDiv.scrollHeight
    

to check check if `offsetHeight + scrollTop` of the div is bigger than or equal to `scrollHeight` of it.

If it’s `true` , then we see the `'scrolled to bottom'` string logged.

### Conclusion

To detect scroll end with JavaScript, we can listen to the `scroll` event.

Then in the event listener, we check if `offsetHeight + scrollTop` is bigger than or equal to `scrollHeight` .

The post [How to Detect Scroll End with JavaScript?](https://thewebdev.info/2021/06/27/how-to-detect-scroll-end-with-javascript/) appeared first on [The Web Dev](https://thewebdev.info).