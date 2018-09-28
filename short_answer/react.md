## Why we need React? Do we have to use it?

Of course that we don't *have* to use React, but React makes it easier to create interactive UIs. We design simple views for each state in our application, and React will efficiently update and render just the right components when our data changes. Declarative views make the code more predictable and easier to debug. Also, React provides these benefits:

* JSX

JSX allows us to write HTML-like code right into their JavaScript. React takes care of the heavy lifting by transforming the JSX into React compatible code. Most developers are familiar with HTML syntax and it allows us to picture the structure of the component or page we are building more easily.

* Reusable Components

React provides a component based structure. Each component decides how it should be rendered. Each component has its own internal logic. We can re-use components. As a result, (1) our app has consistent look and feel, (2) code re-use makes it easier to maintain and grow our codebase, and (3) it is easier to develop an app.

* Fast render with Virtual DOM

We won't have to deal with the DOM directly at all. React will handle all that for us (and likely do a better/faster job than we would have.)


## What is the difference between state and props?

### props

This simply is shorthand for properties. Props  are how components talk to each other. Props flow downwards from the parent component. Components receive data from the parent. There is also the case that you can have default props so that props are set even if a parent component doesn’t pass props down.

**Props are used to pass data from parent to child or by the component itself. They are immutable and thus will not be changed.**

### state

Normally components don’t have state and so are referred to as stateless. A component using state is known as stateful.

State is used so that a component can keep track of information in between any renders that it does. When you `setState()` it updates the state object and then re-renders the component. 

**State is used for mutable data, or data that will change. This is particularly useful for user input. Think search bars for example. The user will type in data and this will update what they see.**

## Explain the lifecycle methods of React (componentWillMount...)

Each React component has several “lifecycle methods” that we can override to run code at particular times in the process. Methods prefixed with **will** are called right before something happens, and methods prefixed with **did** are called right after something happens.

### Initialization
Setup props and states in `constructor()`

The constructor for a React component is called before it is mounted. When implementing the constructor for a `React.Component` subclass, you should call `super(props)` before any other statement. 

### Mounting
```js
componentWillMount() -> render() -> componentDidMount()
```

These methods are called when an instance of a component is being created and inserted into the DOM.

* `UNSAFE_componentWillMount()` (was previously named `componentWillMount()`) is invoked just before mounting occurs. It is called before `render()`, therefore calling `setState()` synchronously in this method will not trigger an extra rendering. 

* `componentDidMount()` is invoked immediately after a component is mounted. This method is a good place to set up any subscriptions.

### Updation
#### props
```js
componentWillReceiveProps(nextProps) -> shouldComponentUpdate(nextProps, nextState) -> true -> componentWillUpdate() -> render() -> componentDidUpdate(prevProps, prevState, snapshot)
```

#### states
```js
shouldComponentUpdate(nextProps, nextState) -> true -> componentWillUpdate() -> render() -> componentDidUpdate(prevProps, prevState, snapshot)
```

* `UNSAFE_componentWillReceiveProps()` (was previously named `componentWillReceiveProps()`) is invoked before a mounted component receives new props. 

* `shouldComponentUpdate()` is invoked before rendering when new props or state are being received. Defaults to `true`. If `shouldComponentUpdate()` returns `false`, then `componentWillUpdate()`, `render()`, and `componentDidUpdate()` will not be invoked.

* `UNSAFE_componentWillUpdate()` (was previously named `componentWillUpdate()`) is invoked just before rendering when new props or state are being received. 

* `componentDidUpdate()` is invoked immediately after updating occurs. 

### Unmonting
```js
componentWillUnmount()
```

* `componentWillUnmount()` is invoked immediately before a component is unmounted and destroyed. Perform any necessary cleanup in this method, such as invalidating timers, canceling network requests, or cleaning up any subscriptions that were created in `componentDidMount()`.

