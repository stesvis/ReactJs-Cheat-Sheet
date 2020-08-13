# ReactJs Cheat Sheet
Basics of ReactJs and recommended VS Code extensions.
* React tutorial: https://reactjs.org/docs/getting-started.html
* W3 Schools React tutorial: https://www.w3schools.com/react/default.asp

## Installation

* Install NodeJs: https://nodejs.org/en/
* Create the app via command line:`npx create-react-app <app_name>`

Then:
```
cd <app_name>
npm start
```

## React App Lifecycle
The three phases are: **Mounting**, **Updating**, and **Unmounting**.

### Mounting
1. `constructor(props) { ... }`: set the state, call APIs etc
```javascript
  constructor(props) {
    super(props);
    this.state = {favoritecolor: "red"};
  }
```
2. `getDerivedStateFromProps(props, state) { ... }`: this is the natural place to set the `state` object based on the initial `props`
```javascript
  static getDerivedStateFromProps(props, state) {
    return {favoritecolor: props.favcol };
  }
```
3. `render() { ... }`: outputs HTML to the DOM
4. `componentDidMount() { ... }`: called after the component is rendered

### Updating
1. `getDerivedStateFromProps() { ... }`: the first method that is called when a component gets updated. This is still the natural place to set the state object based on the initial props
2. `shouldComponentUpdate() { ... }`: return a Boolean value that specifies whether React should continue with the rendering or not
3. `render() { ... }`
4. `getSnapshotBeforeUpdate() { ... }`: you have access to the `props` and `state` before the update, meaning that even after the update, you can check what the values were before the update. If the `getSnapshotBeforeUpdate()` method is present, you should also include the `componentDidUpdate()` method, otherwise you will get an error.
```javascript
  getSnapshotBeforeUpdate(prevProps, prevState) {
    document.getElementById("div1").innerHTML =
    "Before the update, the favorite was " + prevState.favoritecolor;
  }
```
5. `componentDidUpdate() { ... }`: called after the component is updated in the DOM
```javascript
  componentDidUpdate() {
    document.getElementById("mydiv").innerHTML =
    "The updated favorite is " + this.state.favoritecolor;
  }
```

### Unmounting
1. `componentWillUnmount() { ... }`: called when the component is about to be removed from the DOM

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
    let fieldName = event.target.name;
    let val = event.target.value;
    
    // you can do live validation in this handler or in the submit handler
    if (fieldName === "age") {
      if (!Number(val)) {
        //alert("Your age must be a number");
        error = <strong>Your age must be a number</strong>;
      }
    }
    
    this.setState({ errorMessage: error });
    this.setState({ [fieldName]: val }); // access the field via state array
  }
  
  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <h1>Hello {this.state.username} {this.state.age}</h1>
        
        {this.state.errorMessage}
        
        <p>Enter your name:</p>
        <input
          type='text'
          name='username'
          value='{this.state.username}'
          onChange={this.handleChange}
        />
        
        <p>Enter your age:</p>
        <input
          type='text'
          name='age'
          onChange={this.handleChange}
        />
        
        <select value={this.state.myCar}>
          <option value="Ford">Ford</option>
          <option value="Volvo">Volvo</option>
          <option value="Fiat">Fiat</option>
        </select>
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
