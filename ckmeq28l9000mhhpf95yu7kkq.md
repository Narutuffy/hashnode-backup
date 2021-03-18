## How to Add a Scroll Event Listener to a Scrollable Element in a React Component?

Sometimes, we may want to add a scroll event listener to a scrollable element in a React component.

In this article, we’ll look at how to add a scroll event listener to a scrollable element in a React component.

### Add a Scroll Event Listener to a Scrollable Element in a React Component

We can pass in a callback function as the value of the `onScroll` prop of the scrollable element.

For instance, we can write:

    import React, { useState, useRef } from "react";
    
    export default function App() {
      const prevScrollY = useRef(0);
      const [goingUp, setGoingUp] = useState(false);
    
      const onScroll = (e) => {
        const currentScrollY = e.target.scrollTop;
        if (prevScrollY.current < currentScrollY && goingUp) {
          setGoingUp(false);
        }
        if (prevScrollY.current > currentScrollY && !goingUp) {
          setGoingUp(true);
        }
        prevScrollY.current = currentScrollY;
        console.log(goingUp, currentScrollY);
      };
    
      return (
        <div onScroll={onScroll} style={{ height: 300, overflowY: "scroll" }}>
          {Array(50)
            .fill("foo")
            .map((f, i) => {
              return <p key={i}>{f}</p>;
            })}
        </div>
      );
    }
    

We have the `prevScrollY` ref to store the previous value of `window.scrollY` so we can compare with the current `scrollY` value to see if we’re scrolling up or down.

Then we define the `goingUp` state to let us track whether we’re scrolling up or down.

We get the `e.target.scrollTop` value to get the current vertical scroll position.

Then we compare the `currentScrollY` against the `prevScrollY.current` value.

If `currentScrollY` is bigger than the `prevScrollY.current` and `goinUp` is `true` , then we’re going down.

So we call `setGoingUp` with `false` to to indicate that we’re scrolling down.

On the other hand, if we have `prevScrollY.current` bigger than `currentScrollY` and `goinUp` is `false` , then we call `setGoingUp` to `true` to indicate that we’re scrolling up.

Then we set `prevScrollY.current` to `currentScrollY` to store the previous vertical scroll position

And then we login the value of `goingUp` and `currentScrollY` .

Below that, we add our scrollable div with the `onScroll` prop set to the `onScroll` function.

We set the `height` to a finite number so that we can make the div scrollable.

And we render the content of the div inside that.

Now when we scroll up and down, we see the `goingUp` and `currentScrollY` values logged.

### Conclusion

We can watch the scroll position of a scrollable element by passing in a scroll event handler to the `onScroll` prop.

The post [How to Add a Scroll Event Listener to a Scrollable Element in a React Component?](https://thewebdev.info/2021/03/16/how-to-add-a-scroll-event-listener-to-a-scrollable-element-in-a-react-component/) appeared first on [The Web Dev](https://thewebdev.info).