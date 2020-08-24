`...in progress...`

# ReactJs Cheat Sheet
Basics of ReactJs and recommended VS Code extensions.
* **React tutorial**: https://reactjs.org/docs/getting-started.html
* **W3 Schools React tutorial**: https://www.w3schools.com/react/default.asp
* **VS Code Extensions**: https://github.com/stesvis/ReactJs-Cheat-Sheet/blob/master/vscode-extensions.md
* **React Developer Tools**: https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en

## Summary
* <a href="#installation">Installation</a>
* <a href="#components">Components</a>
* <a href="#state">State</a>
* <a href="#events">Events</a>
* <a href="#component-lifecycle">Component Lifecycle</a>
* <a href="#lists">Lists</a>
* <a href="#forms">Forms</a>
* <a href="#css">CSS</a>
* <a href="#consuming-apis">Consuming APIs</a>
* <a href="#routing">Routing</a>
* <a href="#hooks">Hooks</a>
* <a href="#context">Context</a>
* <a href="#other-topics">Other Topics</a>

## Installation

* Install NodeJs: https://nodejs.org/en/
* Create the app via command line:`npx create-react-app <app_name>`

Then:
```
cd <app_name>
npm start
```

## Components
### Functional vs Class
#### Function
* Takes `props` as an argument

```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

#### Class
* Accepts `props` as an argument in the constructor
* Can access props with `this.props`

```javascript
class Welcome extends React.Component {
  constructor(props) {
    super(props);
  }
  
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

## State
* State can only be used in **class** components
* State must be initialized in the **constructor** before you can use it
```javascript
constructor(props) {
  super(props);
  this.state = { name: 'John' };
}
```
* State can not be modified directly
```javascript
this.state.name = 'Michael'; // WRONG
this.setState({ name: 'John' }; // correct
```

## Events
React has the same events as HTML: click, change, mouseover etc.
React events are written in camelCase syntax:
* `onClick` instead of `onclick`.

React event handlers are written inside curly braces:
* `onClick={handleClick}`  instead of `onClick="handleClick()"`.

### Passing Arguments and Use `this`
To be able to use `this` in an event handler you have to use arrow functions:
```javascript
handleClick = () => {
  // you can use this
  console.log(this);
}
```
To pass arguments you have to use the arrow function when you define the event:
```javascript
onClick={() => this.handleClick('Goal')}
```
And of course use it in the handler too:
```javascript
handleClick = (arg) => { ... }
```

#### Binding an Event Handler
Another option to use `this` in an event handler is to bind it in the constructor:
```javascript
constructor(props) {
  super(props);

  // This binding is necessary to make `this` work in the callback
  this.handleClick = this.handleClick.bind(this);
}
```

### Passing the Event
You can pass the actual event and it will be automatically available:
```javascript
handleClick = (event) => {
  console.log(event);
}

onClick={this.handleClick}
```

#### Full Event Handler Example
```javascript
class Football extends React.Component {

  handleClick = (arg, event) => {
    console.log(arg); // 'Goal'
  }
  
  render() {
    // event is optional, but you can pass it like this:
    return (
      <button onClick={(event) => this.handleClick('Goal', event)}>Take the shot!</button>
    );
  }
  
}
```

## Component Lifecycle
The three phases are: **Mounting**, **Updating**, **Unmounting** and **ErrorHandling**.

### Mounting
1. `constructor(props) { ... }`: set the state, call APIs etc
```javascript
constructor(props) {
  super(props);
  this.state = {favoritecolor: "red"};
}
```
2. `static getDerivedStateFromProps(props, state) { ... }`: this is the natural place to set the `state` object based on the initial `props`
```javascript
static getDerivedStateFromProps(props, state) {
  return {favoritecolor: props.favcol };
}
```
3. `render() { ... }`: outputs HTML to the DOM
4. `componentDidMount() { ... }`: called after the component is rendered

### Updating
1. `static getDerivedStateFromProps(props, state) { ... }`: the first method that is called when a component gets updated. This is still the natural place to set the state object based on the initial props
2. `shouldComponentUpdate(nextProps, nextState) { ... }`: return a Boolean value that specifies whether React should continue with the rendering or not
3. `render() { ... }`
4. `getSnapshotBeforeUpdate(prevProps, prevState) { ... }`: you have access to the `props` and `state` before the update, meaning that even after the update, you can check what the values were before the update. If the `getSnapshotBeforeUpdate()` method is present, you **should** also include the `componentDidUpdate()` method, otherwise you will get an error.
```javascript
getSnapshotBeforeUpdate(prevProps, prevState) {
  // Are we adding new items to the list?
  // Capture the scroll position so we can adjust scroll later.
  if (prevProps.list.length < this.props.list.length) {
    const list = this.listRef.current;
    return list.scrollHeight - list.scrollTop;
  }
  return null;
}
```
5. `componentDidUpdate(prevProps, prevState, snapshot) { ... }`: called after the component is updated in the DOM
```javascript
componentDidUpdate(prevProps, prevState, snapshot) {
  // If we have a snapshot value, we've just added new items.
  // Adjust scroll so these new items don't push the old ones out of view.
  // (snapshot here is the value returned from getSnapshotBeforeUpdate)
  if (snapshot !== null) {
    const list = this.listRef.current;
    list.scrollTop = list.scrollHeight - snapshot;
  }
}
```

### Unmounting
1. `componentWillUnmount() { ... }`: called when the component is about to be removed from the DOM

### Error Handling
1. `static getDerivedStateFromError(error)`: returns a new state or null
```javascript
static getDerivedStateFromError(error) {
  // Update state so the next render will show the fallback UI.
  return { hasError: true };
}
```
2. `componentDidCatch(error, info)`: called during the “commit” phase, so side-effects are permitted. It should be used for things like logging errors
```javascript
componentDidCatch(error, info) {
  // Example "componentStack":
  //   in ComponentThatThrows (created by App)
  //   in ErrorBoundary (created by App)
  //   in div (created by App)
  //   in App
  logComponentStackToMyService(info.componentStack);
}
```

## Lists
To render lists keep in mind two things:
1. You should use the javascript `map()` function
2. Every list item must have a `key`

```javascript
const persons = [
  { id: 1, name: 'John' },
  { id: 2, name: 'Mary' },
  { id: 3, name: 'Michael' },
];

const listItems = persons.map((person) =>
  <li key={person.id}>
    {person.name}
  </li>
);
```

## Forms
React forms tutorial: https://reactjs.org/docs/forms.html
W3 Schools React forms tutorial: https://www.w3schools.com/react/react_forms.asp

Formik: https://formik.org/docs/overview

#### Notes
* The `state` field names must match the `form` field names
* You can make a `handleChange` event handler to handle changes from each `form` field
* The `handleSubmit` event handler needs to call `event.preventDefault()` to **prevent page reload**, check the form **validation**, and **submit** the form via API call

#### Submitting a Form
```javascript
class MyForm extends React.Component {

  constructor(props) {
    super(props);
    
    // state field names must match the form field names
    this.state = {
      username: 'Initial value',
      age: null,
      isFriendly: false,
      gender: null,
      myCar: 'Volvo',
      errorMessage: ''
    };
  }
  
  handleSubmit = (event) => {
    event.preventDefault(); // avoids page reload
    
    let age = this.state.age;
    
    // you can validate on submit
    if (!Number(age)) {
      //alert("Your age must be a number");
      error = <strong>Your age must be a number</strong>;
      this.setState({ errorMessage: error });
      return;
    }
    
    // call the POST api
  }
  
  handleChange = (event) => {
    //let fieldName = event.target.name;
    //let value = event.target.value;
    
    let { fieldName, value, type, checked } = event.target; // extract those two values
    
    // you can do live validation in this handler or in the submit handler
    if (fieldName === "age") {
      if (!Number(value)) {
        //alert("Your age must be a number");
        error = <strong>Your age must be a number</strong>;
      }
    }
    
    this.setState({ errorMessage: error });
    
    type === 'checkbox' ? this.setState({ [name]: checked }) : this.setState({ [fieldName]: value }); // access the field via state array
  }
  
  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <h1>Hello {this.state.username} {this.state.age}</h1>
        
        {this.state.errorMessage}
        
        <!-- Text -->
        <p>Enter your name:</p>
        <input
          type='text'
          name='username'
          value='{this.state.username}'
          onChange={this.handleChange}
        />
        
        <!-- Number -->
        <p>Enter your age:</p>
        <input
          type='text'
          name='age'
          onChange={this.handleChange}
        />
        
        <!-- Checkbox -->
        <label>
          <input type="checkbox" name="isFriendly" checkeck={this.state.isFriendly} onChange="{this.handleChange} />
          Is friendly
        </label
        
        <!-- Radio buttons -->
        <label>
          <input type="radio" name="gender" value="Male" checkeck={this.state.gender === 'male'} onChange="{this.handleChange} />
          Male
        </label>
        <label>
          <input type="radio" name="gender" value="Female" checkeck={this.state.gender === 'female'} onChange="{this.handleChange} />
          Female
        </label>
        
        <!-- Select -->
        <select value={this.state.myCar} onChange={this.handleChange}>
          <option value="Ford">Ford</option>
          <option value="Volvo">Volvo</option>
          <option value="Fiat">Fiat</option>
        </select>
        
        <
      </form>
    );
  }
}
```

## CSS
You can style components using CSS, but the property names must be **camelCased** like for the events, for example: `backgroundColor` vs `background-color`

### Inline CSS
```javascript
<h1 style={{ backgroundColor: 'blue' }}>This is a Title</h1>
```

### Object CSS
```javascript
render() {
  const bigBlueTitleStyle = {
    color: DodgerBlue;
    padding: 40px;
    font-family: Arial;
    text-align: center;
  };

  return (
    <h1 style={bigBlueTitleStyle}>This is a Title</h1>
  );
}
```

### Stylesheets
You can write your CSS styling in a separate file, just save the file with the `.css` file extension, and import it in the application.

#### App.css
```css
body {
  background-color: #282c34;
  color: white;
  padding: 40px;
  font-family: Arial;
  text-align: center;
}

<!-- Define other styles as usual -->
```

#### App.js
```javascript
import './App.css';
```

### Modules
Create the CSS module with the `.module.css` extension:
#### myStyle.module.css
```css
.bigBlueTitle {
  color: DodgerBlue;
  padding: 40px;
  font-family: Arial;
  text-align: center;
}
```

Import the stylesheet in your component and use the style:
#### App.js
```javascript
import styles from './mystyle.module.css';
```
```javascript
return <h1 className={styles.bigBlueTitle}>This is a Title</h1>;
```

## Consuming APIs
You can use the javascript `fetch()` or a library like **Axios**.

### Fetch
Usually you fetch data in the `componentDidMount()` method:
```javascript
componentDidMount() {
  fetch('https://jsonplaceholder.typicode.com/posts')
    .then(response => response.json())
    .then(data => console.log(data));
}
```

### Axios
Axios: https://github.com/axios/axios

#### Available Methods
* `axios.request(config)`
* `axios.get(url[, config])`
* `axios.delete(url[, config])`
* `axios.head(url[, config])`
* `axios.options(url[, config])`
* `axios.post(url[, data[, config]])`
* `axios.put(url[, data[, config]])`
* `axios.patch(url[, data[, config]])`

GET example:
```javascript
const axios = require('axios');

// Make a request for a user with a given ID
axios.get('/user?ID=12345')
  .then(function (response) {
    // handle success
    console.log(response);
  })
  .catch(function (error) {
    // handle error
    console.log(error);
  })
  .then(function () {
    // always executed
  });
```

ALL example:
```javascript
axios.all([
  axios.get('https://jsonplaceholder.typicode.com/posts'),
  axios.get('https://jsonplaceholder.typicode.com/users')
])
.then(response => {
  console.log('Date created: ', response[0].data.created_at);
  console.log('Date created: ', response[1].data.created_at);
});
```

## Routing
You need to install **react-router-dom**:
* `$ npm install --save react-router-dom`
And then you can start using components like `<Router>` (BrowserRouter), `<Link>`, `<Switch>` and `<Route>` to build navbars and handle your menus.

#### Example
```javascript
import React from "react";
import {
  BrowserRouter as Router,
  Switch,
  Route,
  Link
} from "react-router-dom";

// This site has 3 pages, all of which are rendered
// dynamically in the browser (not server rendered).
//
// Although the page does not ever refresh, notice how
// React Router keeps the URL up to date as you navigate
// through the site. This preserves the browser history,
// making sure things like the back button and bookmarks
// work properly.

export default function BasicExample() {
  return (
    <Router>
      <div>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/about">About</Link>
          </li>
          <li>
            <Link to="/dashboard">Dashboard</Link>
          </li>
        </ul>

        <hr />

        {/*
          A <Switch> looks through all its children <Route>
          elements and renders the first one whose path
          matches the current URL. Use a <Switch> any time
          you have multiple routes, but you want only one
          of them to render at a time
        */}
        <Switch>
          <Route exact path="/">
            <Home />
          </Route>
          <Route path="/about">
            <About />
          </Route>
          <Route path="/dashboard">
            <Dashboard />
          </Route>
        </Switch>
      </div>
    </Router>
  );
}

// You can think of these components as "pages"
// in your app.

function Home() {
  return (
    <div>
      <h2>Home</h2>
    </div>
  );
}

function About() {
  return (
    <div>
      <h2>About</h2>
    </div>
  );
}

function Dashboard() {
  return (
    <div>
      <h2>Dashboard</h2>
    </div>
  );
}
```

## Hooks
Hooks allow to use functional components with `state`.

**Note**: Don’t call Hooks inside loops, conditions, or nested functions.

### `useState()`
With this hook you can use `state` in a functional component.

https://reactjs.org/docs/hooks-state.html

It takes an argument which is the initial value of the state property and it returns an array of two elements:
1. The state property
2. A function to update that property

```javascript 
const [<variable_name>, <function>] = useState(<variable_initial_value>);
```

#### Example
```javascript
import React, { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

### `useEffect()`
The Effect Hook lets you perform side effects in function components.

https://reactjs.org/docs/hooks-effect.html

```javascript
import React, { useState, useEffect } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  // Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    // Update the document title using the browser API
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
#### `useEffect()` dependencies
- if you do not pass a second argument it will execute `useEffect` every time the component is re-rendered (so on every change of `state` and/or `props`
- if you pass an array of `state` and/or `props` a second argument to `useEffect` it will execute `useEffect` every time one of those variables are updated
- if you pass an empty array it will execute `useEffect` only once, and it's the equivalent of `componentDidMount()`:
```javascript
  useEffect(() => {
    // Update the document title using the browser API
    document.title = `You clicked ${count} times`;
  }, []); // will only execute it once
```


## Context
`Context` allows you to pass props from a parent component directly to any child component without passing it to every component in the tree.

`TODO`

https://reactjs.org/docs/context.html

## Other Topics

### Fragment
You should use `<Fragment>...</Fragment>` to wrap the return value of a component instead of wrapping everything inside a `<div>...</div>`

#### Example
```javascript
class Columns extends Component {
  render() {
    return (
      <Fragment>
        <td>Hello</td>
        <td>World</td>
      </Fragment>
    );
  }
}
```
The code above results in the following html
```html
<table>
  <tr>
    <td>Hello</td>
    <td>World</td>
  </tr>
</table>
```

### Spread operator
It expands the array into individual elements. The syntax is `[...<name_of_the_array_to_spread>]`.

#### Example
```javascript
const react = ['React'];
const spelling = [...react];

// ['R', 'e', 'a', 'c', 't']
```
