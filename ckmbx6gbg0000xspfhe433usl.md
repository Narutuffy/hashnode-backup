## How to Use the React useEffect Hook with Debounce?

Sometimes, we may not want to run the code in the `useEffect` hook immediately after a state update.

In this article, we’ll look at how to use the `useEffect` hook with the code inside the `useEffect` callback debounced.

### Use the React useEffect Hook with Debounce

We can create our own hook that uses the `useEffect` hook to run code with the `useEffect` callback debounced.

To do this, we can write:

    import React, { useEffect, useRef, useState } from "react";
    
    const useDebounce = (value, delay) => {
      const [debouncedValue, setDebouncedValue] = useState("");
      const firstDebounce = useRef(true);
    
      useEffect(() => {
        if (value && firstDebounce.current) {
          setDebouncedValue(value);
          firstDebounce.current = false;
          return;
        }
    
        const handler = setTimeout(() => {
          setDebouncedValue(value);
        }, delay);
    
        return () => clearTimeout(handler);
      }, [value, delay]);
    
    return debouncedValue;
    };
    
    export default function App() {
      const value = useDebounce("abc", 1000);
    
      useEffect(() => {
        console.log(value);
      }, [value]);
    
      return <div>{value}</div>;
    }
    

We create the `useDebounce` hook with the `value` and `delay` parameters.

`value` is the value we want to set.

`delay` is the denounce delay for the `useEffect` callback code.

We have the `firstDebounce` ref to keep track of whether the denounced code is running the first time.

In the `useEffect` callback, we set `firstDebounce.current` to `false` so that we know that it’s not the first time that the denounced code is run it.

Then we call the `setTimeout` function with a callback with the denounced code.

In the callback, we call `setDebouncedValue` to set the `debouncedValue` state value.

Then we return the call that runs `clearTimeout` which runs when the component is unmounted.

In `App` , we call `useDebounce` with the value we want to set and the delay.

Then we log the value in the `useEffect` callback when the `value` value changes.

And we also render the `value` below that.

Now we should see the `'abc'` string rendered and logged after 1 second.

### Conclusion

We can create our own hook to run code that we want to denounce within the `useEffect` callback.

The post [How to Use the React useEffect Hook with Debounce?](https://thewebdev.info/2021/03/14/how-to-use-the-react-useeffect-hook-with-debounce/) appeared first on [The Web Dev](https://thewebdev.info).