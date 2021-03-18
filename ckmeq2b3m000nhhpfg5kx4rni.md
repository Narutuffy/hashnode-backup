## How to Update a State in a React Component in a Scroll Event Listener?

Sometimes, we may want to update a state in a React component in a scroll event listener.

In this article, we’ll look at how to update a state in a React component in a scroll event listener.

### Adding a Scroll Event Listener into a React Component

We can add the code to add the scroll event listener into a React component’s `useEffect` hook.

The `useEffect` hook lets us commit side effects, so it’s appropriate for using it to watch scrolling location and update a state accordingly.

For instance, we can write:

    import React, { useState, useEffect, useRef } from "react";
    
    export default function App() {
      const prevScrollY = useRef(0);
    
      const [goingUp, setGoingUp] = useState(false);
    
      useEffect(() => {
        const handleScroll = () => {
          const currentScrollY = window.scrollY;
          if (prevScrollY.current < currentScrollY && goingUp) {
            setGoingUp(false);
          }
          if (prevScrollY.current > currentScrollY && !goingUp) {
            setGoingUp(true);
          }
    
          prevScrollY.current = currentScrollY;
          console.log(goingUp, currentScrollY);
        };
    
        window.addEventListener("scroll", handleScroll, { passive: true });
    
        return () => window.removeEventListener("scroll", handleScroll);
      }, [goingUp]);
    
      return (
        <div>
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

Next, we add the `useEffect` hook with a callback that has the `handleScroll` function to let us compare the previous and current `scrollY` values.

If th `prevScrollY.current` value is less than the current one and `goingUp`is `true`, then we call `setGoingUp` to `false` to indicate that we’re scrolling down.

Otherwise, if we have `prevScrollY.current` value that is bigger than the current one and `goingUp` is `false`, then we call `setGoingUp` with `true` to indicate that we’re scrolling up.

We then set `prevScrollY.current` to the `currentScrollY` value since it’s going to become the previous value in the next render cycle.

Then we call `window.addEventListener` to add the scroll event listener.

`window` is the browser tab, so we watch the tab’s scrolling.

`passive` set to `true` means `preventDefault` will never be called in the event listener.

Then we return a function that calls `removeEventListener` to clear the scroll listener once we unmount the component.

Below that, we have an array of text we render into the page.

Now when we scroll up and down, we should see the console log log the `goingUp` value and the scroll Y position.

### Conclusion

We can add a scroll event listener within the `useEffect` callback to listen to scrolling events.

The post [How to Update a State in a React Component in a Scroll Event Listener?](https://thewebdev.info/2021/03/16/how-to-update-a-state-in-a-react-component-in-a-scroll-event-listener/) appeared first on [The Web Dev](https://thewebdev.info).