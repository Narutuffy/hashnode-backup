## How to Update a State Inside the setInterval Callback in a React Hook?

The `setInterval` function lets us run code in our JavaScript apps periodically.

We may sometimes use this in React component to update a state periodically with new values.

In this article, we’ll look at how to update a state inside the `setInterval` callback in a React hook.

### Running setInterval in a React Component

To run `setInterval` in a React component, we should put it in the `useEffect` hook.

The `useEffect` hook is used for committing side effects, which includes creating timers.

Therefore, we can write something like:

    import React, { useEffect, useState } from "react";
    
    export default function App() {
      const [gamePlayTime, setGamePlayTime] = useState(0);
    
      useEffect(() => {
        const gameStartInternal = setInterval(() => {
          setGamePlayTime((t) => t + 1);
        }, 1000);
    
        return () => {
          clearInterval(gameStartInternal);
        };
      }, []);
    
      return <div>{gamePlayTime}</div>;
    }
    

We have the `gamePlayTime` state which we update by calling `setGamePlayTime` .

Next, we call `useEffect` with a callback and create the timer inside the callback by calling `setInterval` .

We pass in 1000 as the 2nd argument so that the `setInterval` callback only runs 1000 milliseconds.

It returns a timer ID so that we can call `clearInterval` on it when the component unmounts.

And we did that in the callback we return in the `useEffect` callback.

The callback we return is run when we unmount the component.

We pass in an empty array into the 2nd argument so that the `useEffect` callback runs only once when the component mounts.

This way, we don’t create the timer more than once.

Then below that, we display the `gamePlayTime` .

Now we should see the `gamePlayTime` value update every second on the screen and the latest value displayed.

### Conclusion

We just call `setInterval` in the `useEffect` callback to create the timer.

When we unmount the component, we call `clearInterval` in the callback we return to clear it when we unmount the component.

The post [How to Update a State Inside the setInterval Callback in a React Hook?](https://thewebdev.info/2021/03/14/how-to-update-a-state-inside-the-setinterval-callback-in-a-react-hook/) appeared first on [The Web Dev](https://thewebdev.info).