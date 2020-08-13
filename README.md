# ReactJs-Cheat-Sheet
Basics of ReactJs and recommended VS Code extensions.

### Installation

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
* Does not take `props` as an argument
* Can access props with `this.props`

```javascript
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```
