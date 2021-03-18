## How to Use React Context API with Multiple Values for Providers?

The React Context API is handy for letting us share data between components easily.

Sometimes, we may want to share multiple states between components.

In this article, we’ll look at how to share multiple states between components with one context provider.

### Use React Context API with Multiple Values for Providers

We can pass in anything we want to the `value` prop of the context provider component.

So sharing multiple states with one provider is no problem.

For instance, we can write:

    import React, { useContext, useState } from "react";
    const CountContext = React.createContext("count");
    
    const DescendantA = () => {
      const { count, setCount, count2, setCount2 } = useContext(CountContext);
    
      return (
        <>
          <button onClick={() => setCount((c) => c + 1)}>Click me {count}</button>
          <button onClick={() => setCount2((c) => c + 1)}>Click me {count2}</button>
        </>
      );
    };
    const DescendantB = () => {
      const { count, setCount, count2, setCount2 } = useContext(CountContext);
    
    return (
        <>
          <button onClick={() => setCount((c) => c + 1)}>Click me {count}</button>
          <button onClick={() => setCount2((c) => c + 1)}>Click me {count2}</button>
        </>
      );
    };
    export default function App() {
      const [count, setCount] = useState(0);
      const [count2, setCount2] = useState(0);
      return (
        <CountContext.Provider value={{ count, setCount, count2, setCount2 }}>
          <DescendantA />
          <DescendantB />
        </CountContext.Provider>
      );
    }
    

We create the `CountContext` with the `React.createContext` method.

Then we wrap the `CountContext.Provider` component around the `DescendantA` and `DescendantB` components.

We pass in the state getter and setters we defined in `App` all to one object into the `value` prop.

Then in the `DescendantA` and `DescendantB` components, we call `useConext` with `CountContext` to return the object we passed into the `value` prop.

We destructured all the properties and use them.

We show `count` and `count2` in the buttons.

And we call `setCount` and `setCount2` in the button click handlers.

Now when we click on the buttons, we see the counts go up.

### Conclusion

We can pass in anything we want into the `value` prop of the context provider component in React.

So we can share anything between components as long as we pass them into the `value` prop.

The post [How to Use React Context API with Multiple Values for Providers?](https://thewebdev.info/2021/03/14/how-to-use-react-context-api-with-multiple-values-for-providers/) appeared first on [The Web Dev](https://thewebdev.info).