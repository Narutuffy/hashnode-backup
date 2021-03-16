## # How to Detect Clicks Outside a React Component Using Hooks?

Sometimes, we may want to detect clicks outside a React component for situations like when we want to create a popup menu that closes when we click outside.

In this article, we’ll look at how to detect clicks outside a React component with hooks.

### Listening for Clicks on the Page

We can listen for clicks on the page to detect where we clicked on the page.

If we click outside the component, then we remove the React component from the DOM.

Checking for where we clicked on and running code accordingly is called event delegation.

For instance, we can write:

    import React, { useEffect, useRef, useState } from "react";
    
    const useComponentVisible = (initialIsVisible) => {
      const [isComponentVisible, setIsComponentVisible] = useState(
        initialIsVisible
      );
      const ref = useRef(null);
    
      const handleClickOutside = (event) => {
        if (ref.current && !ref.current.contains(event.target)) {
          setIsComponentVisible(false);
        }
      };
    
      useEffect(() => {
        document.addEventListener("click", handleClickOutside, true);
        return () => {
          document.removeEventListener("click", handleClickOutside, true);
        };
      });
    
    return { ref, isComponentVisible, setIsComponentVisible };
    };
    
      const DropDown = () => {
      const { ref, isComponentVisible } = useComponentVisible(true);
    
      return <div ref={ref}>{isComponentVisible && <p>drop down</p>}</div>;
    };
    
    export default function App() {
      return (
        <div>
          <DropDown />
        </div>
      );
    }
    

We create the `useComponentVisible` hook to watch for clicks on the page.

We first define the `isComponentVisible` state to track when the component should be visible.

We set that to the `initialVisible` prop value.

Then we define a `ref` that’s assigned to the component we want to close when we click outside.

We do the element check in the `handleClickOutside` function.

This is whee we call `contains` to check if the element we clicked on is outside the component.

If `!ref.current.contains(event.target)` is `true` , then we know we clicked outside the component.

Next, we call `document.addEventListener` to listen for clicks on the whole page so we can do the check above.

We return a callback that calls `removeEventListener` to remove the event listener when we unmount the component.

Next, we define the `DropDown` component to that gets the ref and `isComponentVisible` state that we returned.

Then we assign the ref to the div so we use the click event listener to do the comparison.

Then finally, we add the `DropDown` component to `App` so we can see it.

Now when we click outside the text, the text should go away.

### Conclusion

We can detect clicks outside a React component by assign a ref to the component we want to close when we click outside it.

Then we can attach an event handler to `document` and check whether we clicked outside the element that’s assigned the ref.

The post [\# How to Detect Clicks Outside a React Component Using Hooks?](https://thewebdev.info/2021/03/14/how-to-detect-clicks-outside-a-react-component-using-hooks/) appeared first on [The Web Dev](https://thewebdev.info).