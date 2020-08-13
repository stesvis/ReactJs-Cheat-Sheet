# ReactJs-Cheat-Sheet
Basics of ReactJs and recommended VS Code extensions.

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
1. `constructor() { ... }`
2. `getDerivedStateFromProps() { ... }`
3. `render() { ... }`
4. `componentDidMount() { ... }`

### Updating
1. `getDerivedStateFromProps() { ... }`
2. `shouldComponentUpdate() { ... }`
3. `render() { ... }`
4. `getSnapshotBeforeUpdate() { ... }`
5. `componentDidUpdate() { ... }`

### Unmounting
1. `componentWillUnmount() { ... }`

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
onClick={() => this.handleClick("Goal")}
```
#### Full Event Handler Example
```javascript
class Football extends React.Component {

  handleClick = (arg) => {
    console.log(arg); // "Goal"
  }
  
  render() {
    return (
      <button onClick={() => this.handleClick("Goal")}>Take the shot!</button>
    );
  }
  
}
```
