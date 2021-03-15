## How to Set State with a Deeply Nested Objects with React Hooks?

Sometimes, we may want to set the value of a state with a deeply nested object in our React components.

In this article, we’ll look at how to set a state with a deeply nested object with React hooks.

### Setting a State Value to Deeply Nested Object

We can set a state value to a deeply nested object with the state setter function returned from the `useState` hook.

For instance, we can write:

    import React from "react";
    
    export default function App() {
      const [nestedState, setNestedState] = React.useState({
        propA: "apple",
        propB: "bar"
      });
    
      const changeSelect = (event) => {
        const newValue = event.target.value;
        setNestedState((prevState) => {
          return {
            ...prevState,
            propA: newValue
          };
        });
      };
    
      return (
        <React.Fragment>
          <div>{JSON.stringify(nestedState)}</div>
          <select value={nestedState.propA} onChange={changeSelect}>
            <option value="apple">apple</option>
            <option value="grape">grape</option>
            <option value="orange">orange</option>
          </select>
        </React.Fragment>
      );
    }
    

We have the `nestedState` state defined with the `useState` hook.

Its initial value is set to an object with the `propA` and `propB` properties.

Next, we define the `changeSelect` function with the `event` parameter.

We get the drop down’s value with the `event.target.value` property.

Then we call `setNestedState` with a callback that has the `prevState` parameter.

`prevState` has the previous value of `nestedState` .

Then we return an object with the `prevState` spread into a new object.

And `propA` is set to `newValue` to set the new value of the `propA` property.

Below that, we have a stringified version of `nestedState` rendered.

And below that we have the select dropdown that has the `value` prop set to `nestedState.propA` .

And `onChange` is set to the `changeSelect` function to get the selected value and use the value to set `nestedState` with `setNestedState` .

Now when we select an option from the drop-down, then the latest value of the stringified `nestedState` object displayed.

### Conclusion

We can set a state value to a deeply nested object by calling a state setter function with a callback that returns the latest value of a state.

The post [How to Set State with a Deeply Nested Objects with React Hooks?](https://thewebdev.info/2021/03/14/how-to-set-state-with-a-deeply-nested-objects-with-react-hooks/) appeared first on [The Web Dev](https://thewebdev.info).