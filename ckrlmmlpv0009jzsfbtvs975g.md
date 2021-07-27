## How to add an emoji picker to an input field in react app


### Creating a new react app

```
npx create-react-app 
```


### Starting the app -

```
# npm
npm start
# yarn
yarn start
```


### Installing the required dependencies -

```
# npm
npm install emoji-mart
# yarn
yarn add emoji-mart
```


### Cleanup process

* Delete everything inside the div in App.js and remove the import of logo.svg on top.

* Delete logo.svg file.

* Delete everything in App.css.

* In index.css add this line of code -

```css
* {
  margin: 0;
}
```

### Creating an input


**Mapping to a state**

```javascript
import { useState } from "react";
import "./App.css";

function App() {
  const [input, setInput] = useState("");

  return (
    <div className="app">
      <input
        value={input}
        onChange={(e) => setInput(e.target.value)}
        type="text"
        placeholder="emoji picker demo"
      />
    </div>
  );
}

export default App;

```

**Creating an svg button for emojis**


**Adding some styles in** ***App.css***

```css
.icon {
  height: 40px;
  width: 40px;
}

.button {
  background: transparent;
  outline: none;
  border: none;
}

```

**Creating a state for showing picker**

```
const [showEmojis, setShowEmojis] = useState(false);
```


**Attaching it to the onClick of button**

```
<button *className*="button" *onClick*={() => setShowEmojis(!showEmojis)}>
```


**Rendering the emoji picker**

```javascript
 {showEmojis && (
        <div>
          <Picker />
        </div>
      )}
```

We will import Picker and the CSS like this


**Adding the emojis with the text in the input**

Add an *onSelect *to the picker

```
<Picker *onSelect*={addEmoji} />
```


Create the addEmoji function


Now our emoji picker is working! 🥳

![](https://cdn.hashnode.com/res/hashnode/image-dev/upload/v1627364194569/2-Op8yZRu.png)

### Useful links:

* [Github repository](https://github.com/avneesh0612/emoji-picker-demo)

* [React docs](https://reactjs.org/)

* [All socials](https://avneesh-links.vercel.app/)