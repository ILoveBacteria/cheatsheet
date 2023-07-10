# React Cheatsheet

## Table Of Contents

- [React Cheatsheet](#react-cheatsheet)
  - [Table Of Contents](#table-of-contents)
  - [Design Patterns](#design-patterns)
  - [Component Lifecycle](#component-lifecycle)
  - [Precompile](#precompile)
  - [Webpack](#webpack)
    - [Config file with multiple entry points:](#config-file-with-multiple-entry-points)
    - [Parallel Multiple Export](#parallel-multiple-export)
  - [JSX](#jsx)
  - [Hooks](#hooks)
  - [Handwrite Notes](#handwrite-notes)

## Design Patterns

A common React programming pattern is to use a parent
stateful component to manage state and define state updating methods. Then, it will render stateless child components.
One or more of those child components will be
responsible for updating the parent state (via methods
passed as props). One or more of those child
components will be responsible for displaying that state.

Here is a snippet code that Menu.js is a stateless component and will update the state of its parent component.

```javascript
class App extends React.Component {
  constructor(props) {
    super(props);
    this.chooseVideo = this.chooseVideo.bind(this);
  }

  chooseVideo(newVideo) {
    this.setState({
      // Change something
    });
  }

  render() {
    return (
      <div>
        <h1>Video Player</h1>
        <Menu chooseVideo={this.chooseVideo} />
      </div>
    );
  }
}
```

```javascript
class Menu extends React.Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  }
  
  handleClick(e) {
    // The e is an event object
    this.props.chooseVideo(e.target.value);
  }

  render() {
    return (
      <form onClick={this.handleClick}>
      </form>
    );
  }
}
```

## Component Lifecycle

![Component Lifecycle](./component_lifecycle.jpg)

## Precompile

**Some tools for a React app**

1- Precompile the code with JSX format: `npx babel <src> --out-dir <out> --presets react-app/prod`

2- Good dependencies:
  - babel-cli
  - babel-runtime
  - babel-preset-react-app = "^3.1.2"
  - babel-plugin-transform-runtime
  - webpack-cli
  - webpack

3- Webpack creates a bundle. [Read more][1]

## Webpack

### Config file with multiple entry points:

```javascript
const path = require('path');

module.exports = {
  entry: {
    app: './src/app.js',
    search: './src/search.js',
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist',
  },
};
```

### Parallel Multiple Export

In case you export multiple configurations, you can use the parallelism option on the configuration array to specify the maximum number of compilers that will compile in parallel.

```javascript
module.exports = [
  {
    //config-1
  },
  {
    //config-2
  },
];
module.exports.parallelism = 1;
```

## JSX

- **Conditional:** `&&` is commonly used to render an element based on a boolean condition.
```jsx
{condition && <Element />}
{condition && <p>A paragraph</p>}
```

## Hooks

How to use hooks with the previous state:

```javascript
function Counter({ initialCount }) {
  const [count, setCount] = useState(initialCount);
  return (
    <div>
      Count: {count}
      <button onClick={() => setCount(initialCount)}>Reset</button>
      <button onClick={() => setCount((prevCount) => prevCount - 1)}>-</button>
    </div>
  );
}
```

## Handwrite Notes

- Passing Arguments to Event Handlers
It is common to want to pass an extra parameter to an event handler. For example, if id is the row ID, either of the following would work:
```jsx
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

- You should add type module to the script tag in the HTML file
```html
<script defer type="module" src=""></script>
```

[1]: https://webpack.js.org/guides/getting-started/