# ReactJs-Cheat-Sheet
Basics of ReactJs and recommended VS Code extensions.
* React tutorial: https://reactjs.org/docs/getting-started.html
* W3 Schools React Tutorial: https://www.w3schools.com/react/default.asp

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
* State must be initialized in the **constructor**
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
