## How to handle click outside a div in React with a custom hook


This is a very important thing, especially when creating dropdowns. The user expects the dropdown to close when they click outside the dropdown. So we are going to see how can we do that.

## Creating a new react app

Prerequisites for creating a react app-

* [Node.js](https://nodejs.org/) (Latest version recommended)

After Node is installed you need to run the following command-

```
npx create-react-app handle-click-demo # you can name it anything you want
```


## Starting the app -

```
# npm
npm start
# yarn
yarn start
```


## Cleanup process

* Delete everything inside the div in App.js and remove the import of logo.svg on top.

* Delete App.test.js, SetupTests.js, logo.svg files.

* Delete everything in App.css.

* In index.css add this line on the top-


We are going to create our own dropdown instead of using the select tag.

### Creating a dropdown

We will create a button to open and close the dropdown.

```
<button>Open</button>
```


We can see this button on the top left part of the screen.

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1627364202654/AWkrYu9EG.png)

Now we will create a state which will check if the dropdown is open or closed.

```javascript
  const [isOpen, setIsOpen] = useState(false);
```

Now we will add an onClick event on the button.


We will change the text of the button if the state is true or false with a ternary operator.


Finally, we will create the dropdown.

I have created a simple dropdown below the button that has 3 options.


I have applied some basic stylings to the dropdown and the app div to center the elements.


### Rendering the dropdown based onClick of the button

```javascript
     {isOpen ? (
        <div className="dropdown_container">
          <p>Option 1</p>
          <p>Option 2</p>
          <p>Option 3</p>
        </div>
      ) : (
        <></>
      )}
```

What this code is doing is, if the isOpen is true then show dropdown otherwise show nothing.

But this has a problem, if you click outside the div the dropdown won’t close.


### Creating a custom hook

We will create a custom hook called useComponentVisible. So create a file useComponentVisible.js inside the src directory and add this snippet of code.


This code is using the state and doing a similar thing that we did while creating the dropdown. Then it is adding and removing the Event listener.

### Using the custom hook

Now we will use the custom hook inside of our app.

Calling the hook. We will replace the useState with this. This will give some errors for now.


Importing the hook.

```
import useComponentVisible from "./useComponentVisible";
```


Now we will attach the ref to the button and dropdown container as we want the dropdown to close when we click elsewhere.

We will change isOpen to isComponentVisible and setisOpen to setIsComponentVisible like this -

```javascript
import "./App.css";
import useComponentVisible from "./useComponentVisible";

function App() {
  const { ref, isComponentVisible, setIsComponentVisible } =
    useComponentVisible(false);

  return (
    <div className="app">
      <button
        ref={ref}
        onClick={() => setIsComponentVisible(!isComponentVisible)}
      >
        {isComponentVisible ? "Close" : "Open"}
      </button>
      {isComponentVisible ? (
        <div ref={ref} className="dropdown_container">
          <p>Option 1</p>
          <p>Option 2</p>
          <p>Option 3</p>
        </div>
      ) : (
        <></>
      )}
    </div>
  );
}

export default App;

```

Now are dropdown is working perfectly.


Hope you learned from this tutorial. Let me know what you want to see next. ✌

Useful links -

[Github repository](https://github.com/avneesh0612/handle-click-outside-div)

[ReactJS docs](https://reactjs.org/)

[All socials](https://avneesh-links.vercel.app/)