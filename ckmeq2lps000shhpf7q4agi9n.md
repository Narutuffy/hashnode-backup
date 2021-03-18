## How to Run the React useEffect Hook Callback Only When All Dependencies Change?

Sometimes, we only want to run the `useEffect` hook callback when all the dependencies change.

In this article, we’ll look at how to run the React `useEffect` hook callback only when all the dependencies change.

### React useEffect Hook Callback Only When All Dependencies Change

To run the `useEffect` hook callback only when all the dependencies change, we can store the previous values in a ref and then compare them to the latest values.

If the previous values are different from the previous values, then we run the `useEffect` callback code.

For instance, we can write:

    import React, { useEffect, useRef, useState } from "react";
    
    export default function App() {
      const [name, setName] = useState("");
      const [age, setAge] = useState("");
    
      const previousValues = useRef({ name, age });
    
      useEffect(() => {
        if (
          previousValues.current.name !== name &&
          previousValues.current.age !== age
        ) {
          console.log(name, age);
          console.log(previousValues.current);
          previousValues.current = { name, age };
        }
      });
    
      return (
        <div>
          <input
            placeholder="name"
            value={name}
            onChange={(e) => setName(e.target.value)}
          />
          <input
            placeholder="age"
            value={age}
            onChange={(e) => setAge(e.target.value)}
          />
        </div>
      );
    }
    

We have the `name` and `age` states that we defined with the `useState` hook.

Then we create the `previousValues` ref to store the previous values of `name` and `age` in an object.

We store them in a ref so that when the value of it changes, it won’t trigger a re-rendering of the component.

This lets us get both the previous and current values since re-rendering will erase the previous state values if we don’t store them in a ref.

Then in the `useEffect` callback, we compare the values of the current and previous values of `name` and `age` .

And if they’re both different, then we log the values of `name` and `age` .

And then we store the previous values of the `name` and `age` in an object and assign them to `previousValues.current` to store them in a ref.

We don’t pass in a 2nd argument to `useEffect` so it runs on every render.

Below that, we have 2 inputs that we can type into to change the value of `name` and `age` .

Now when we type in different values into the inputs, we see the console log run.

### Conclusion

To run code in a `useEffect` callback when all the dependencies change, we can store the previous values of the states in a ref.

Then we can compare them in the `useEffect` callback and all the previous and current values are different, then we can run the code we want to run.

The post [How to Run the React useEffect Hook Callback Only When All Dependencies Change?](https://thewebdev.info/2021/03/14/how-to-run-the-react-useeffect-hook-callback-only-when-all-dependencies-change/) appeared first on [The Web Dev](https://thewebdev.info).