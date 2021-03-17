## React Hooks in 5 Minutes

What are they?
--------------

A set of functions that provide a direct API to methods we access on `Component` instances. We can create stateful components or access the component lifecycle without `class` instances 🎉

For those in camp **TL;DR**, scroll down for a collection of demos 👍

Jumping in 👟
-------------

Consider this app that selects and displays a color value 🎨

![Select element that updates label content](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1615969084387/QYYk0tBTP.gif)

We need a `class` instance to add `state`.

    const colors = {
      Sea: '#a2ccb6',
      Sand: '#fc22b5',
      Peach: '#ee786e',
    }
    
    class App extends Component {
      state = {
        color: colors.Sea,
      }
      render = () => {
        const { color } = this.state
        return (
          <Fragment>
            <select
              value={color}
              onChange={e => this.setState({color: e.target.value})}
              >
              { Object.entries(colors).map(c => (
                <option key={`color--${c[0]}`} value={c[1]}>
                  {c[0]}
                </option>
              ))}
            </select>
            <h2>{`Hex: ${color}`}</h2>
          </Fragment>
        )
      }
    }

But with hooks

    const { useState } = React
    const App = () => {
      const [color, setColor] = useState(colors.Sea)
      return (
        <Fragment>
          <select value={color} onChange={e => setColor(e.target.value)}>
            {Object.entries(colors).map(([name, value]) => (
              <option value={value}>{name}</option>
            ))}
          </select>
          <h1>{`Hex: ${color}`}</h1>
        </Fragment>
      )
    }

`useState` is a hook that allows us to use and update stateful values.

Check out [this pen](https://codepen.io/jh3y/pen/NeLrjd) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

useState
--------

The `useState` hook provides a stateful value and a way to update it. The argument is the default value. That value can be any type too! 👍

No need for a `class` instance 🙌

Don’t be afraid of that syntax. `useState` makes use of `Array` destructuring.

It’s equal to

    const state = useState(Colors.Sea)
    const color = state[0]
    const setColor = state[1]

Why not class? 📗
-----------------

*   Minification isn’t great.
*   Loss of context where classes try to take on too much.
*   Poor separation of concerns in lifecycle methods.
*   Requires unstable syntax transforms for `class` properties.
*   HMR issues.
*   Subjective use cases, when to use as opposed to stateless function.

If classes work for you, no need to change. Hooks aren’t replacing classes.

Other hooks
-----------

There are several hooks. The ones you’ll likely spend the most time with are `useState` and `useEffect`. Check out the others in the [Hooks reference](https://reactjs.org/docs/hooks-reference.html).

useEffect
---------

We use this hook when we want to hook into lifecycle stages.

    useEffect === componentDidMount + componentDidUpdate + componentWillUnmount

We pass a function to the `useEffect` hook that runs on every render.

Let’s update our color choosing app from earlier using `useEffect`.

    const App = () => {
      const [color, setColor] = useState(colors.Sea)
      useEffect(
        () => {
          document.body.style.background = color
        }
      )
      return (
        <Fragment>
          <select value={color} onChange={e => setColor(e.target.value)}>
            {Object.entries(colors).map(([name, value]) => (
              <option key={`color--${name}`} value={value}>
                {name}
              </option>
            ))}
          </select>
          <h1>{color}</h1>
        </Fragment>
      )
    }
    

Now when the state is updated the body color will change 👍

![Select element that changes background color](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1615969090567/BlXmWOAT8.gif)

That’s running every render? Yep. It doesn’t have to though. There’s an optional second parameter for `useEffect`. You can pass an `Array` of values and if those values don’t change between render, the effects won’t execute. An empty `Array` would mean that the effect only runs once. But in most cases, there's a [better solution](https://reactjs.org/docs/hooks-effect.html#tip-optimizing-performance-by-skipping-effects) to achieve that result,

    useEffect(
      () => {
        document.body.style.background = color
      },
      [color]
    )

Now we only set the background when `color` changes 👍 In this example it will still run every render though as `color` is the only thing triggering a render.

Check out [this pen](https://codepen.io/jh3y/pen/NeLrxr) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

If we had a second stateful value, we could see that optional parameter in action. Let’s add a counter value that increments on button click.

    const App = () => {
      const [color, setColor] = useState(colors.Sea)
      const [count, setCount] = useState(0)
      // Only run when color is updated 👍
      useEffect(
        () => {
          console.info('Color changed')
          document.body.style.background = color
        },
        [color]
      )
      return (
        <Fragment>
          <select value={color} onChange={e => setColor(e.target.value)}>
            {Object.entries(colors).map(([name, value]) => (
              <option key={`color--${name}`} value={value}>
                {name}
              </option>
            ))}
          </select>
          <h1>{color}</h1>
          <h1>{`Count: ${count}`}</h1>
          <button onClick={() => setCount(count + 1)}>Increment Count</button>
        </Fragment>
      )
    }

That `console.info` will only fire when color changes 👍

Check out [this pen](https://codepen.io/jh3y/pen/bzGaZa) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

How about other effects such as making API requests or binding user input?

Let’s make a small app that tracks mouse movement.

![Mouse moving around the screen and updating on-screen labels with position](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1615969096057/pD9TasNra.gif)

We use `useEffect` to bind mouse movement to update some stateful values.

    const App = () => {
      const [x, setX] = useState()
      const [y, setY] = useState()
      useEffect(
        () => {
          const update = (e) => {
            setX(e.x)
            setY(e.y)
          }
          window.addEventListener('mousemove', update)
        },
        [setX, setY]
      )
      return x && y ? (<h1>{`x: ${x}; y: ${y};`}</h1>) : null
    }

How do we clear up that bind if the component becomes unmounted? We can return a function from our `useEffect` function for clean up.

    useEffect(
      () => {
        const update = (e) => {
          setX(e.x)
          setY(e.y)
        }
        window.addEventListener('mousemove', update)
        return () => {
          window.removeEventListener('mousemove', update)
        }
      },
      [setX, setY]
    )

Nice 👊

Check out [this pen](https://codepen.io/jh3y/pen/pqOmME) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

Separation of concerns
======================

Hooks allow us to have a better separation of concerns.

Ever seen a `class` lifecycle method where a lot seems to be going on?

    componentDidMount = () => {
      makeSomeAPIRequest()
      makeOtherAPIRequest()
      bindTouchListener()
      bindClickEvents()
      doOtherUnrelatedStuff()
    }

We can avoid this with hooks. As long as our hooks are at the top level we can use as many as we like.

Consider updating our app to also listen for `resize` events. We don’t need this to happen in our `mousemove` effect. We can create a separate one. This is a good habit to get into. Especially when we start creating custom hooks.

    const App = () => {
      const [dimensions, setDimensions] = useState(getDimensions())
      const [x, setX] = useState()
      const [y, setY] = useState()
      // Effect for mousemove
      useEffect(
        () => {
          const update = e => {
            setX(e.x)
            setY(e.y)
          }
          window.addEventListener('mousemove', update)
          return () => {
            window.removeEventListener('mousemove', update)
          }
        },
        [setX, setY]
      )
      // Effect for window resizing
      useEffect(
        () => {
          const updateSize = () => setDimensions(getDimensions())
          window.addEventListener('resize', updateSize)
          return () => {
            window.removeEventListener('resize', updateSize)
          }
        },
        [setDimensions]
      )
      return (
        <Fragment>
          {x && y && <h1>{`x: ${x}; y: ${y};`}</h1>}
          <h1>
            {`Height: ${dimensions.height}; Width: ${dimensions.width};`}
          </h1>
        </Fragment>
      )
    }

Here's a demo 👍

Check out [this pen](https://codepen.io/jh3y/pen/zyeYaZ) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

Creating custom hooks
---------------------

The component in that last example is starting to grow. One of Hook’s greatest attributes is that we can extract their use into custom hooks.

This is a big sell for hooks. You may be familiar with `Higher Order Components` and `render` props. We often need a certain structure or style that can prove hard to maintain or justify. This isn’t the case using Hooks.

Consider our example. Tracking mouse movement could be common in our application. Sharing that logic would be ideal. Let’s do it!

    const useMousePosition = () => {
      const [x, setX] = useState()
      const [y, setY] = useState()
      useEffect(
        () => {
          const update = e => {
            setX(e.x)
            setY(e.y)
          }
          window.addEventListener('mousemove', update)
          return () => {
            window.removeEventListener('mousemove', update)
          }
        },
        [setX, setY]
      )
      return { x, y }
    }

Note how our new custom hook returns the current state value. Now any component could use this custom hook to grab the mouse position.

    const App = () => {
      const { x, y } = useMousePosition()
      return x && y ? <h1>{`x: ${x}; y: ${y};`}</h1> : null
    }

Now we have logic we can share across other components 💪

Check out [this pen](https://codepen.io/jh3y/pen/bGNaEJQ) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

Let’s consider another example. We have various watches. They look different but they all use the same time ⌚️ We could have a custom hook for grabbing the time. Here’s an example;

Check out [this pen](https://codepen.io/jh3y/pen/REePOq) by Jhey ([@jh3y](https://codepen.io/jh3y)) on [CodePen](https://codepen.io).

DOs 👍
------

*   Use when you need to hook into state or a lifecycle stage
*   Separate concerns with hooks

DON’Ts 👎
---------

*   Use in loops
*   Nest them
*   Use them based on conditions.

NOTES ⚠️
--------

*   Available as of react@16.7.0-alpha
*   No breaking changes 🙌
*   eslint-plugin-react-hooks@next 👍

That’s it!
----------

A 5 minute intro to React Hooks!

*   Dive further [here](https://reactjs.org/docs/hooks-intro.html)
*   Grab all the code [here](https://codepen.io/collection/DowNNY/)